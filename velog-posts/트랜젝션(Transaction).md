<h1 id="트랜젝션이란">트랜젝션이란?</h1>
<p>트랜잭션(Transaction)은 데이터베이스 관리 시스템(DBMS)에서 하나의 작업 단위를 의미하며, 데이터베이스의 상태를 변환하는 논리적 작업 집합입니다. 
쉽게 설명하면 트랜잭션(Transaction)은 &quot;더이상 분할이 불가능한 업무처리의 단위&quot;를 의미한다.</p>
<p>여기서 하나의 작업 단위는 하나의 SQL문이 아니라 하나의 작업에 필요한 여러 SQL을 묶어서 작업 단위라고 한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/ba73d4b7-b2b9-4cfc-8f64-c9052fbb91bc/image.png" /></p>
<p>예를 들어, 송금(계좌이체)가 있다. 돈을 주는 사람의 계좌는 해당 금액이 차감되어야 하고, 돈을 받는 사람은 계좌에 금액이 증가되어야한다. 두 작업은 따로 분리하면 완전한 송금이 아니게 된다.
이처럼 분리할 수 없는 작업들을 모으면 하나의 트랜젝션 작업 단위라고 한다.</p>
<p>트랜잭션은 데이터의 무결성과 일관성을 보장하기 위해 다음과 같은 ACID 속성을 만족해야 합니다.</p>
<h1 id="acid">ACID</h1>
<hr />
<p>하나의 트랜잭션이 발생했을 때  안전성을 보장하기 위한 중요한 4가지 속성을 말합니다.</p>
<table>
<thead>
<tr>
<th>ACID</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td><strong>Atomicity (원자성)</strong></td>
<td>모든 작업이 성공하거나 아무 작업도 일어나지 않음</td>
</tr>
<tr>
<td><strong>Consistency (일관성)</strong></td>
<td>하나의 트랜잭션 끝난 뒤에도 모든 상태는 이전과 같이 유효</td>
</tr>
<tr>
<td><strong>Isolation (격리성)</strong></td>
<td>모든 트랜잭션은 다른 트랜잭션으로부터 독립적</td>
</tr>
<tr>
<td><strong>Durability (지속성)</strong></td>
<td>하나의 트랜잭션이 완료되었다면 영구적으로 저장</td>
</tr>
</tbody></table>
<p>이 개념은 1970년대 말에 짐 그레이(Jim Gray)가 신뢰할 수 있는 트랜잭션 시스템의 이러한 특성으로 정의했고, 자동으로 이들을 수행하는 기술을 개발했습니다.</p>
<p><strong>원자성</strong>은 한 트랜잭션의 연산들이 모두 성공하거나, 반대로 전부 실패되는 성질을 말한다.</p>
<blockquote>
</blockquote>
<p>하나의 단위로 묶여있는 여러 작업이 부분적으로 실행된다면, 업데이트가 일어났지만 누가 업데이트했는지 모르거나, 업데이트 날짜가 누락되는 등 데이터가 오염될 수 있다.
예를 들어 계좌이체를 할 때에는 다음과 같은 두 단계가 있다.
A 계좌에서 출금한다.
B 계좌에 입금한다.
계좌이체를 하려는데 A 계좌에서는 출금이 이뤄지고, B 계좌에 입금되지 않았다고 가정한다.
어디서 문제가 발생했는지 파악할 수 없다면, A 계좌에서 출금된 돈은 세상에서 사라지는 돈이 된다.
만약 은행에서 이런 일이 발생한다면, 은행은 더이상 제 기능을 할 수 없을 것이다다.
A 계좌에서 출금하는 일에 성공했지만, B 계좌에 입금하는 작업에 실패한다면 계좌 A에서 출금하는 작업을 포함하여 모든 작업이 실패로 돌아가야 한다는 것이 Atomicity(원자성)이다.
원자성을 지켰다면 1번과 2번, 두 작업이 모두 성공적으로 완료되어야 한다.
그렇지 않으면(둘 중 하나의 작업이라도 실패한다면), 하나의 단위로 묶여있는 모든 작업이 실패하게 만들어 기존 데이터를 보호한다. (롤백 시킨다.)
SQL에서도 마찬가지이다.
특정 쿼리를 실행했는데 부분적으로 실패하는 부분이 있다면, 전부 실패하도록 구현되어 있다.
때때로 충돌 요인에 대해서 선택지를 제공한다.</p>
<p><strong>일관성</strong>은 데이터베이스의 일관성 있는 규칙은 트랜잭이 완료되었을 때에도 이 일관성을 유지할 수 있어야 합니다. 즉 이 일관성을 깨는 데이터에 대해서는 트랜잭션이 성공적으로 실행되지 않아야 한다는 의미입니다.</p>
<blockquote>
</blockquote>
<p>예를 들어 ‘모든 고객은 반드시 이름을 가지고 있어야 한다’는 데이터베이스의 제약이 있다고 가정한다.
다음과 같은 트랜잭션은 Consistency(일관성)를 위반한다.
이름 없는 새로운 고객을 추가하는 쿼리
기존 고객의 이름을 삭제하는 쿼리
데이터베이스의 유효한 상태는 다를수 있지만, 데이터의 상태에 대한 일관성은 변하지 않아야 한다.
이 예시는 ‘이름이 있어야 한다’ 라는 제약을 위반한다.
따라서 예시 트랜잭션이 일어난 이후의 데이터베이스는 일관되지 않는 상태를 가지게 된다.</p>
<p><strong>격리성</strong>은 모든 트랜잭션은 다른 트랜잭션으로부터 독립되어야 한다는 뜻이다.
실제로 동시에 여러 개의 트랜잭션들이 수행될 때, 각 트랜젝션은 고립(격리)되어 있어 연속으로 실행된 것과 동일한 결과를 나타낸다.</p>
<blockquote>
</blockquote>
<p>예를 들어 게좌에 만 원이 있다고 가정한다.
이 계좌로부터 계좌 B로 6천 원을, 계좌 C로 6천 원을 동시에 계좌 이체하는 경우, 계좌 B에 먼저 송금한 뒤 계좌 C에 보내는 결과와 동일해야 한다.
동시에 트랜잭션을 실행한다고 해서 계좌 B와 C에 각각 6천 원씩 송금하여 마이너스 통장이 되는 것이 아니다.
각각의 송금 작업을 연속으로 실행하는 것과 동일한 결과가 나타나야 한다.
격리성을 지키는 각 트랜젝션은 철저히 독립적이기 때문에, 다른 트랜젝션의 작업 내용을 알 수 없다.
그리고 트랜잭션이 동시에 실행될 때와 연속으로 실행될 때의 데이터베이스 상태가 동일해야 한다.</p>
<p><strong>지속성</strong>은 다른말로 내구성을 의미하며 트랜잭션이 성공적으로 수행된 이후에는 데이터베이스에 어떤 시스템적인 문제가 발생하더라도 데이터를 영구적으로 보존할 수 있어야 한다는 의미입니다.
만약 런타임 오류나 시스템 오류가 발생하더라도, 해당 기록은 영구적이어야 한다는 뜻이다.</p>
<blockquote>
</blockquote>
<p>예를 들어 은행에서 게좌이체를 성공적으로 실행한 뒤에, 해당 은행 데이터베이스에 오류가 발생해 종료되더라도 계좌이체 내역은 기록으로 남아야 한다.
마찬가지로 계좌이체를 로그로 기록하기 전에 시스템 오류 등에 의해 종료가 된다면, 해당 이체 내역은 실패로 돌아가고 각 계좌들은 계좌이체 이전 상태들로 돌아가게 된다.</p>
<h1 id="전파-속성-propagation">전파 속성 (Propagation)</h1>
<hr />
<p>트랜잭션에서 전파 속성은 하나의 트랜잭션이 시작된 상태에서, 또 다른 트랜잭션이 시작되었을 때 이를 어떻게 처리할지를 정의하는 것입니다.</p>
<table>
<thead>
<tr>
<th><strong>종류</strong></th>
<th><strong>기존 트랜잭션 X</strong></th>
<th><strong>기존 트랜잭션 O</strong></th>
<th>적용 사례</th>
</tr>
</thead>
<tbody><tr>
<td><strong>REQUIRED</strong></td>
<td>새 트랜잭션 생성</td>
<td>기존 트랜잭션에 참여</td>
<td>기본값. 대부분의 비지니스 로직</td>
</tr>
<tr>
<td><strong>REQUIRES_NEW</strong></td>
<td>새 트랜잭션 생성</td>
<td>기존 트랜잭션 일시중단, 새로운 트랜잭션 생성</td>
<td>독립적인 작업 처리</td>
</tr>
<tr>
<td><strong>SUPPORTS</strong></td>
<td>트랜잭션 없이 진행</td>
<td>기존 트랜잭션 참여</td>
<td>트랜잭션이 필수가 아닌 작업</td>
</tr>
<tr>
<td><strong>NOT_SUPPORTED</strong></td>
<td>트랜잭션 없이 진행</td>
<td>기존 트랜잭션 일시중단, 트랜잭션 없이 진행</td>
<td>로그 저장 등 트랜잭션과 독집적인 작업</td>
</tr>
<tr>
<td><strong>MANDATORY</strong></td>
<td>IllegalTransactionStateException 발생</td>
<td>기존 트랜잭션 참여</td>
<td>트랜잭션 내부에서만 호출 가능한 메소드</td>
</tr>
<tr>
<td><strong>NEVER</strong></td>
<td>트랜잭션 없이 진행</td>
<td>IllegalTransactionStateException 발생</td>
<td>외부 시스템 호출시</td>
</tr>
<tr>
<td><strong>NESTED</strong></td>
<td>새 트랜잭션 생성</td>
<td>중첩 트랜잭션 생성</td>
<td>부분적으로 롤백 가능한 작업</td>
</tr>
</tbody></table>
<h1 id="격리-수준-isolation">격리 수준 (Isolation)</h1>
<p>트랜잭션 격리 수준은 여러 트랜잭션이 동시 발생할 때 어떻게 데이터 일관성을 보장 할지를 정의하는 것입니다.</p>
<table>
<thead>
<tr>
<th><strong>Isolation Level</strong></th>
<th><strong>설명</strong></th>
<th><strong>방지되는 문제</strong></th>
<th><strong>적용 사례</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>READ_UNCOMMITTED</strong></td>
<td>커밋되지 않은 데이터도 읽을 수 있음</td>
<td>없음</td>
<td>데이터 정확성보다 성능이 중요한 경우</td>
</tr>
<tr>
<td><strong>READ_COMMITTED</strong></td>
<td>커밋된 데이터만 읽을 수 있음</td>
<td>Dirty Read</td>
<td>대부분의 애플리케이션 기본 설정</td>
</tr>
<tr>
<td><strong>REPEATABLE_READ</strong></td>
<td>같은 트랜잭션 내에서 읽은 데이터가 변경되지 않음</td>
<td>Dirty Read, Non-repeatable Read</td>
<td>은행 계좌 조회와 같은 데이터 무결성 요구</td>
</tr>
<tr>
<td><strong>SERIALIZABLE</strong></td>
<td>트랜잭션이 순차적으로 실행, 가장 엄격한 격리 수준</td>
<td>Dirty Read, Non-repeatable Read, Phantom Read</td>
<td>금융 시스템의 이체 작업 등 높은 신뢰성이 필요한 경우</td>
</tr>
</tbody></table>
<p>이를 정리하면 아래와 같습니다.</p>
<table>
<thead>
<tr>
<th><strong>수준</strong></th>
<th><strong>Dirty Read</strong></th>
<th><strong>Non-repeatable Read</strong></th>
<th><strong>Phantom Read</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>READ_UNCOMMITTED</strong></td>
<td>O</td>
<td>O</td>
<td>O</td>
</tr>
<tr>
<td><strong>READ_COMMITTED</strong></td>
<td>X</td>
<td>O</td>
<td>O</td>
</tr>
<tr>
<td><strong>REPEATABLE_READ</strong></td>
<td>X</td>
<td>X</td>
<td>O</td>
</tr>
<tr>
<td><strong>SERIALIZABLE</strong></td>
<td>X</td>
<td>X</td>
<td>X</td>
</tr>
</tbody></table>
<p>각 항목에 대한 예제코드를 보고 어떤 결과가 나오는지 확인해보겠습니다.</p>
<p><strong>READ UNCOMMITTED</strong></p>
<pre><code class="language-java">@Service
public class ReadUncommittedExample {

    @Transactional(isolation = Isolation.READ_UNCOMMITTED)
    public void transactionA(UserRepository userRepository) {
        User user = userRepository.findById(1L).orElseThrow();
        user.setBalance(500); // 커밋하지 않고 값만 변경
        try { Thread.sleep(5000); } catch (InterruptedException e) { e.printStackTrace(); }
    }

    @Transactional(isolation = Isolation.READ_UNCOMMITTED)
    public void transactionB(UserRepository userRepository) {
        User user = userRepository.findById(1L).orElseThrow();
        System.out.println(&quot;Tx2 Read balance: &quot; + user.getBalance());
        // 결과: Transaction 1에서 커밋되지 않은 500 출력 (Dirty Read 발생).
    }
}</code></pre>
<p><strong>READ COMMITTED</strong></p>
<pre><code class="language-java">@Service
public class ReadCommittedExample {

    @Transactional(isolation = Isolation.READ_COMMITTED)
    public void transactionA(UserRepository userRepository) {
        User user = userRepository.findById(1L).orElseThrow();
        user.setBalance(500); // 커밋하지 않고 값만 변경
        try { Thread.sleep(5000); }
            catch (InterruptedException e) { e.printStackTrace(); }
    }

    @Transactional(isolation = Isolation.READ_COMMITTED)
    public void transactionB(UserRepository userRepository) {
        User user = userRepository.findById(1L).orElseThrow();
        System.out.println(&quot;Tx2 Read balance: &quot; + user.getBalance());
        // 결과: Transaction 1에서 커밋되지 않은 상태에서는 변경 전 값 출력.
    }
}</code></pre>
<p><strong>REPEATABLE READ</strong></p>
<pre><code class="language-java">@Service
public class RepeatableReadExample {

    @Transactional(isolation = Isolation.REPEATABLE_READ)
    public void transactionA(UserRepository userRepository) {
        User user = userRepository.findById(1L).orElseThrow();
        System.out.println(&quot;Tx1 First balance: &quot; + user.getBalance());

        try { Thread.sleep(5000); }
            catch (InterruptedException e) { e.printStackTrace(); }

        User userAfter = userRepository.findById(1L).orElseThrow();
        System.out.println(&quot;Tx1 Second balance: &quot; + userAfter.getBalance());
        // 결과: First balance와 Second balance 동일 (Non-Repeatable Read 방지).
    }

    @Transactional(isolation = Isolation.REPEATABLE_READ)
    public void transactionB(UserRepository userRepository) {
        User user = userRepository.findById(1L).orElseThrow();
        user.setBalance(500); // 변경된 값은 Transaction 1에 영향 없음
        userRepository.save(user);
    }
}</code></pre>
<p><strong>SERIALIZABLE</strong></p>
<pre><code class="language-java">@Service
public class SerializableExample {

    @Transactional(isolation = Isolation.SERIALIZABLE)
    public void transactionA(UserRepository userRepository) {
        List&lt;User&gt; users = userRepository.findAll();
        System.out.println(&quot;Tx1 Users: &quot; + users.size());

        try { Thread.sleep(5000); }
            catch (InterruptedException e) { e.printStackTrace(); }
    }

    @Transactional(isolation = Isolation.SERIALIZABLE)
    public void transactionB(UserRepository userRepository) {
        User newUser = new User(&quot;New User&quot;, 1000);
        userRepository.save(newUser); // Transaction 1 종료 전까지 삽입 대기
    }
}</code></pre>
<h1 id="스프링-트랜잭션-어노테이션">스프링 트랜잭션 어노테이션</h1>
<hr />
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/0d322cf9-67c8-4b8f-9296-d6313b97a2b3/image.png" /></p>
<p>스프링에는 2가지 @Tx 어노테이션이 존재합니다.</p>
<p>이미지에서 첫번째는 Java에서 지원하는 어노테이션이고, 두번째는 스프링이 지원하는 어노테이션입니다.</p>
<p>두 가지는 트랜잭션의 기본 기능은 모두 동일하게 지원합니다.</p>
<p>하지만 스프링의 어노테이션이 더 많은 기능을 지원하고 있기 때문에 jakara 보다는 스프링의 @Tx 어노테이션을 사용하는 것이 더 좋습니다.</p>
<p><strong>&lt;스프링의 @Transactional 옵션&gt;</strong></p>
<table>
<thead>
<tr>
<th><strong>옵션</strong></th>
<th><strong>설명</strong></th>
</tr>
</thead>
<tbody><tr>
<td>isolation</td>
<td>Transaction의 Isolation level을 설정한다.</td>
</tr>
<tr>
<td>label</td>
<td>Transaction label을 설정한다.</td>
</tr>
<tr>
<td>noRollbackFor</td>
<td>transaction rollback 처리가 되지 않아야 할 예외 클래스를 명시한다.</td>
</tr>
<tr>
<td>noRollbackForClassName</td>
<td>transaction rollback 처리가 되지 않아야 할 예외 클래스 이름을 명시한다.</td>
</tr>
<tr>
<td>propagation</td>
<td>Transaction Propagation 타입을 설정한다.</td>
</tr>
<tr>
<td>readOnly</td>
<td>Transaction을 readOnly로 설정한다.</td>
</tr>
<tr>
<td>rollbackFor</td>
<td>transaction rollback이 되어야하는 예외 클래스를 명시한다.</td>
</tr>
<tr>
<td>rollbackForClassName</td>
<td>transaction rollback이 되어야하는 예외 클래스 이름을 명시한다.</td>
</tr>
<tr>
<td>timeout</td>
<td>Transaction의 Timeout을 설정한다. (단위 : 초,seconds)</td>
</tr>
<tr>
<td>timeoutString</td>
<td>Transaction의 Timeout을 설정한다. (단위 : 초,seconds)</td>
</tr>
<tr>
<td>transactionManager</td>
<td>특정 Transaction의 qualifier value를 설정한다.</td>
</tr>
<tr>
<td>value</td>
<td>transactionManager의 alias(별칭)을 설정한다.</td>
</tr>
</tbody></table>
<h1 id="트랜잭션-상태">트랜잭션 상태</h1>
<hr />
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/ffaad7c9-49cd-43b7-8899-91e41e36392e/image.png" /></p>
<ul>
<li>활동 상태 (active) : 초기 상태, 트랜잭션이 실행 중일 때 가지는 상태</li>
<li>부분 완료 상태 (partically committed) : 마지막 명령문이 실행된 후에 가지는 상태</li>
<li>완료 상태 (committed) : 트랜잭션이 성공적으로 완료된 후 가지는 상태</li>
<li>실패 상태 (failed) : 정상적인 실행이 더 이상 진행될 수 없을 때 가지는 상태</li>
<li>철회 상태 (aborted) : 트랜잭션이 취소되고 데이터베이스가 트랜잭션 시작 전 상태로 환원된 상태</li>
</ul>
<h1 id="스프링의-롤백">스프링의 롤백</h1>
<hr />
<p>우리는 앞서 트랜잭션의 ACID 중 Atomicity(원자성) 속성을 배웠습니다.</p>
<blockquote>
<p>모든 작업이 성공하거나 아무 작업도 일어나지 않음</p>
</blockquote>
<p>이 속성에 의하면 트랜잭션 중에 실패하면 반드시 아무 작업도 일어나지 않아야 합니다. 이는 즉 기존 완료된 작업이 Rollback이 되어야 한다는 의미가 됩니다.</p>
<p>여기서 다시 실패는 Exception 상황을 말할 수 있습니다.</p>
<p>또한, 지난 시간에 예외처리를 배우면서 Checked Exception과 Unchecked Exception을 이야기 했었습니다.</p>
<p>다시 짚고 넘어가자면 아래와 같습니다.</p>
<ul>
<li>Checked Exception : 반드시 try-catch를 사용하여 예외를 강제 처리해야 함</li>
<li>Unchecked Exception : 예외처리를 해도되고 하지 않아도 됨</li>
</ul>
<p>위 2가지 내용을 정리하자면, 아래와 같이 생각할 수 있습니다.</p>
<blockquote>
<p>Tx 내부에서 Exception 발생 → Atomicity 보장 필요 → Rollback</p>
</blockquote>
<p>하지만 이 말은 반은 맞고 반은 틀립니다.
위에 언급한 Unchecked/Checked Exception에 따라 각각 처리가 다릅니다.</p>
<h2 id="스프링의-롤백-기본-동작">스프링의 롤백 기본 동작</h2>
<p>각 Exception 종류에 따른 롤백 기본동작은 아래와 같습니다.</p>
<table>
<thead>
<tr>
<th><strong>조건</strong></th>
<th><strong>롤백 여부</strong></th>
<th><strong>설명</strong></th>
</tr>
</thead>
<tbody><tr>
<td>Unchecked Exception <br />(RuntimeException 또는 Error 계열)</td>
<td>O</td>
<td>기본적으로 스프링 트랜잭션은 RuntimeException 또는 Error 발생 시 롤백 처리</td>
</tr>
<tr>
<td>Checked Exception<br /> (Exception을 상속하고 RuntimeException이 아닌 예외)</td>
<td>X</td>
<td>Checked Exception은 기본적으로 롤백 트리거가 아님 (명시적으로 설정해야 롤백 가능)</td>
</tr>
<tr>
<td>@Transactional의 rollbackFor 설정에 포함된 Checked Exception 발생</td>
<td>O</td>
<td>rollbackFor 속성에 지정된 Checked Exception은 롤백 트리거로 작동</td>
</tr>
<tr>
<td>try-catch로 RuntimeException을 처리한 경우</td>
<td>O</td>
<td>RuntimeException 발생 시 이미 롤백 상태로 마킹되므로 catch 처리 여부와 무관하게 롤백</td>
</tr>
<tr>
<td>try-catch로 Checked Exception을 처리한 경우</td>
<td>X</td>
<td>Checked Exception은 기본적으로 롤백되지 않으며 catch로 처리 시 트랜잭션 상태 유지</td>
</tr>
<tr>
<td>@Transactional의 noRollbackFor 설정에 포함된 Exception 발생</td>
<td>X</td>
<td>noRollbackFor 속성에 지정된 Exception은 롤백하지 않음</td>
</tr>
<tr>
<td>Transactional 내부에서 catch 후 TransactionAspectSupport.currentTransactionStatus().setRollbackOnly(false) 호출</td>
<td>X</td>
<td>catch 이후 롤백 플래그를 false로 명시적으로 변경 시 롤백 방지</td>
</tr>
<tr>
<td>Transaction이 외부 트랜잭션에서 관리되는 경우 (Propagation.REQUIRED)</td>
<td>외부 트랜잭션에 따름</td>
<td>내부 트랜잭션에서 롤백 마킹이 되어도 외부 트랜잭션의 최종 상태에 의존</td>
</tr>
<tr>
<td>Transaction이 새 트랜잭션 (Propagation.REQUIRES_NEW)인 경우</td>
<td>내부 트랜잭션에 따름</td>
<td>내부 트랜잭션의 롤백은 외부 트랜잭션에 영향을 주지 않음</td>
</tr>
</tbody></table>
<p>위 오렌지 부분의 이야기가 바로 아래 배민 블로그의 이야기 입니다. 한 번정도 정독해보시면 좋을 것 같습니다.</p>
<blockquote>
</blockquote>
<p>⚠️ 아래 링크의 내용을 다르고 있습니다. 자세한 내용은 이 블로그를 보시는게 좋습니다. </p>
<ul>
<li><a href="https://techblog.woowahan.com/2606/">https://techblog.woowahan.com/2606/</a></li>
</ul>
<h1 id="트랜잭션-내부-호출-문제">트랜잭션 내부 호출 문제</h1>
<hr />
<p>다음 예제를 보겠습니다.</p>
<pre><code class="language-java">@Service
public class PostService {

    private final PostRepository postRepository;

    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    @Transactional
    public void saveWithTx() {
        postRepository.save(new Post(&quot;Outer method&quot;));
        saveInnerPost(); // 내부 호출로 트랜잭션 적용 실패
    }

    public void saveInnerPost() {
        postRepository.save(new Post(&quot;Inner method - no transaction&quot;));
        throw new RuntimeException(&quot;Exception in saveInnerPost&quot;); // 롤백 안됨
    }
}

@SpringBootTest
public class TransactionTest {

    private final PostService postService;

    public TransactionTest(PostService postService) {
        this.postService = postService;
    }

    @Test
    public void testInternalTransactionIssue() {
        try {
            postService.saveWithTx();
        } catch (RuntimeException ex) {
            System.out.println(&quot;Caught exception: &quot; + ex.getMessage());
        }

        // 데이터베이스 상태를 확인
        // &quot;Outer method&quot;는 저장되며, &quot;Inner method - no transaction&quot;은 저장된 후 롤백되지 않음
    }
}</code></pre>
<p>saveWithTx() 안에서 saveInnerPost()를 호출하고 있습니다. saveWithTx는 @Tx가 적용되어 있으므로 내부에서 호출된 saveInnerPost() 또한 @Tx가 적용 될 것이라 생각할 수 있습니다.</p>
<h3 id="하지만-아닙니다"><strong>하지만 아닙니다.</strong></h3>
<p><code>saveInnerPost()는 @Tx가 적용되지 않습니다.</code> 아래 이미지를 보겠습니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/0a6d35d9-bf37-4994-9d86-d03afb9cdc5a/image.png" /></p>
<p>스프링은 트랜잭션을 적용하기 위해서는 반드시 프록시 객체를 통해야 합니다. 프록시 객체는 우리가 Contoller, @Service, @Repository와 같은 어노테이션을 적용하면 사용할 수 있는 것입니다. 이미 익숙하게 쓰고 있는 어노테이션이고 이 어노테이션을 적용하면 우리는 프록시 객체를 쓰는 것입니다.</p>
<p>이 프록시 객체를 통해 메소드 호출이 일어나면 메소드가 호출 되는 시점, 종료되는 시점에 각각 트랜잭션 처리가 되는 원리입니다.</p>
<p>하지만 Service 내부에서 메소드를 호출하는 경우는 프록시 객체가 아닌 실체 객체의 메소드를 호출하게 되는 것입니다. 이 때문에 스프링 입장에서는 메소드가 호출되고 종료되는 시점을 알 수 없습니다.</p>
<p>이런 문제로 앞서 살펴본 예제에서는 트랜잭션이 적용되지 않는 것입니다.</p>
<ul>
<li>참고 : <a href="https://engineerinsight.tistory.com/277">https://engineerinsight.tistory.com/277</a></li>
</ul>
<p>참고 : <a href="https://hanamon.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%98-acid-%EC%84%B1%EC%A7%88/">https://hanamon.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%98-acid-%EC%84%B1%EC%A7%88/</a></p>