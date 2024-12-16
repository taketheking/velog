<blockquote>
<p><strong>intro</strong>
프로젝트를 하는 과정에서 많은 데이터의 상태를 모두 변경해야되는 문제에 직면했다.</p>
</blockquote>
<p>예를 들어, 사람들이 신고한 사용자의 id를 하루동안 받았다가 한 번에 처리할 때나 쿠폰이 만료 기간이 지나서 한번에 상태를 만료로 변경해야 할 때가 있다.</p>
<h2 id="문제-발생">문제 발생</h2>
<blockquote>
<p>수 백만 건의 데이터를 수정하려니 수백 만 개의 sql문이 데이터베이스에 날아가는 문제가 발생했다.</p>
</blockquote>
<p>보통은 데이터를 수정할 때, 데이터베이스에서 특정 데이터를 가져와서 해당 데이터의 상태를 변경하고 저장하는 단건 수정 방식을 사용한다. 그런데 이 방법을 쓰면 수정하려는 데이터가 수백만, 수천만 개가 되면 데이터베이스가 마비될 것이다.</p>
<pre><code class="language-java">@Transactional
    public void reportUsers(List&lt;Long&gt; userIds) {
        for (Long userId : userIds) {
            User user = userRepository.findById(userId).orElseThrow(() -&gt; new IllegalArgumentException(&quot;해당 ID에 맞는 값이 존재하지 않습니다.&quot;));

            user.updateStatusToBlocked();

            userRepository.save(user);
        }
    }</code></pre>
<p>위에 경우에 백만 개를 처리한다고 하면 백만 번의 select문 처리와 백만 번의 update문 처리가 이루어져야 된다. </p>
<p>그래서 위 문제를 해결하기 위해 한 번의 데이베이스 접근으로 모두 수정하는 방법을 찾아보았다.</p>
<h2 id="해결방법">해결방법</h2>
<blockquote>
<p>벌크 연산을 사용하자!</p>
</blockquote>
<p>벌크 연산은 데이터베이스에서 UPDATE, DELETE 시 대량의 데이터를 한 번에 처리하기 위한 작업이다. 
즉, JPA에서 벌크 연산은 단 건 데이터를 변경(더티 체킹)하는 것이 아닌, 여러 데이터에 변경 쿼리를 날리는 작업을 말한다.</p>
<p>@Modifying을 변경이 일어나는 쿼리와 함께 사용해야 JPA에서 변경 감지와 관련된 처리를 생략하고 더 효율적인 실행이 가능하다.</p>
<pre><code class="language-java">@Modifying(clearAutomatically = true)
@Query(&quot;UPDATE User u SET u.status = :status where u.id IN :userIds&quot;)
void updateStatusByIds(@Param(&quot;status&quot;) String status, @Param(&quot;userIds&quot;) List&lt;Long&gt; userIds);</code></pre>
<p>where ... in ... 조건으로 수정할 데이터들을 정하고 set 으로 수정할 내용으로 수정한다.
여기서 중요한 것은 Modifying 어노테이션이다.</p>
<p>JPA에서는 원래 수정할 때 영속성 컨텍스트가 변화를 감지(1차 캐시와 비교)하여 업데이트를 데이터베이스에 보내게 되는데, 위의 경우는 직접 쿼리문을 보내기 때문에 영속성 컨텍스트와 데이터베이스의 정보가 일치하지 않는다.
그래서 잘못해서 영속성 컨텍스트에 있는 데이터를 조회하면 실제 데이터와 다른 정보를 얻게 되는 불상사가 생기게 된다.</p>
<p>Modifying 어노테이션은 그래서 영속성 컨텍스트를 초기화하는 방법을 제공하는데 (clearAutomatically = true)라는 옵션을 통해 초기화할 수 있게 해준다.</p>
<pre><code class="language-java">...

entityManager.flush();
entityManager.clear();</code></pre>
<p>기본적으로는 영속성 컨텍스트를 초기화 해주는 방법이 있다.</p>
<pre><code class="language-java">@Transactional
    public void reportUsers(List&lt;Long&gt; userIds) {

//        List&lt;User&gt; users = userRepository.findAllById(userIds).stream().toList();
//
//        if(users.isEmpty()) {
//            throw new IllegalArgumentException(&quot;해당 ID에 맞는 값이 존재하지 않습니다.&quot;);
//        }

        // 위의 user 조회가 없어도 동작함
        // 해당 id의 레코드가 없어도 실행됨
        userRepository.updateStatusByIds(&quot;BLOCKED&quot;, userIds);
    }</code></pre>
<p>이 위의 코드는 한 번의 select문과 한 번의 update문으로 많은 데이터를 한 번씩의 접근으로 해결했다.</p>
<h2 id="주의점">주의점</h2>
<h3 id="변경-쿼리-동기화-문제">변경 쿼리 동기화 문제</h3>
<p>JPA에서는 1차 캐시라는 기능이 있다.
1차 캐시를 간단하게 설명하면 영속성 컨텍스트에 있는 1차 캐시를 통해 엔티티를 캐싱하고, DB의 접근 횟수를 줄임으로써 성능 개선 한다.</p>
<p>그런데 @Modifying과 @Query 를 사용한 벌크 연산에서 1차 캐시와 관련하여 문제가 발생한다.
JPA에서 조회를 실행할 시에 1차 캐시를 확인해서 해당 엔티티가 1차 캐시에 존재한다면 DB에 접근하지 않고, 1차 캐시에 있는 엔티티를 반환한다.
하지만 벌크 연산은 1차 캐시를 포함한 영속성 컨텍스트를 무시하고 바로 Query를 실행하기 때문에 영속성 컨텍스트는 데이터 변경을 알 수가 없다.
즉, 벌크 연산 실행 시, 1차 캐시(영속성 컨텍스트)와 DB의 데이터 싱크가 맞지 않게 되는 것이다.</p>
<p>그래서 데이터를 사용하기 전에 영속성 컨텍스트를 비워주는 작업이 필요한데,
@Modifying의 clearAutomatically=true 속성을 사용해 변경 후 자동으로 영속성 컨텍스트를 초기화 할 수 있다. 
해당 속성을 추가하게 되면, 조회를 실행할 때 1차캐시에 해당 엔티티가 존재하지 않기 때문에 DB 조회 쿼리를 실행하게 된다. ( 데이터 동기화 문제를 해결 )</p>
<h3 id="트랜잭션-관리">트랜잭션 관리</h3>
<p>@Modifying 애노테이션은 기본적으로 @Transactional과 함께 사용된다.
변경 작업은 트랜잭션 내에서 실행되어야 하며, 완료되지 않은 변경 작업이 여러 작업에 영향을 줄 수 있기 때문이다.
이를 통해 데이터베이스에 대한 변경 작업을 수행할 때 원자성(Atomicity), 일관성(Consistency), 독립성(Isolation), 지속성(Durability)을 보장할 수 있게 된다.</p>