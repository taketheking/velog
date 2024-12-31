<h1 id="1-querydsl-세팅-및-설정">1. QueryDSL 세팅 및 설정</h1>
<h2 id="1-의존성-추가">1) 의존성 추가</h2>
<ul>
<li><p>build.gradle</p>
<pre><code class="language-java">dependencies {
//QueryDSL
  implementation &quot;com.querydsl:querydsl-jpa:5.0.0:jakarta&quot;
  annotationProcessor &quot;com.querydsl:querydsl-apt:5.0.0:jakarta&quot;
  annotationProcessor &quot;jakarta.annotation:jakarta.annotation-api&quot;
  annotationProcessor &quot;jakarta.persistence:jakarta.persistence-api&quot;

  ...
}</code></pre>
</li>
</ul>
<p>build.gradle에 dependencies 안에 queryDSL 의존성 추가한다.</p>
<h2 id="2-qclass-빌드">2) QClass 빌드</h2>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/cad7fc23-a709-4c0e-9459-dbe1641cf2d1/image.png" />
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/b84272d8-7ab8-47b3-99f6-0b31aa747c7b/image.png" />
Gradle의 Task에서 [build &gt; clean] -&gt;[other &gt; compileJava] 과정을 차례로 수행해보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/7e99fa05-fc9a-4100-b213-d2792ac48d3f/image.png" />
 
다음 사진과 같이 build 디렉터리에 Entity에서 생성된 Q Class가 성공적으로 빌드되었는지 확인해보자.
컴파일 단계에서 Entity와 형태가 같은 Static Class로 QClass을 생성하고, QueryDSL은 해당 클래스를 기반으로 쿼리 메서드를 실행시키게 된다. 따라서 타입의 불일치에 대한 에러 캐치에 관련하여 좋은 장점을 가질 수 있다.</p>
<h3 id="qclass-뜯어보기">QClass 뜯어보기</h3>
<p> </p>
<pre><code class="language-java">/**
 * QMember is a Querydsl query type for Member
 */
@Generated(&quot;com.querydsl.codegen.DefaultEntitySerializer&quot;)
public class QMember extends EntityPathBase&lt;Member&gt; {

    private static final long serialVersionUID = 1003972608L;

    public static final QMember member = new QMember(&quot;member1&quot;);

    public final ListPath&lt;Category, QCategory&gt; categories = this.&lt;Category, QCategory&gt;createList(&quot;categories&quot;, Category.class, QCategory.class, PathInits.DIRECT2);

    public final StringPath email = createString(&quot;email&quot;);

    public final NumberPath&lt;Long&gt; memberId = createNumber(&quot;memberId&quot;, Long.class);

    public final StringPath name = createString(&quot;name&quot;);

    public final StringPath password = createString(&quot;password&quot;);

    public final EnumPath&lt;com.example.tosshelperappserver.common.constant.RoleType&gt; role = createEnum(&quot;role&quot;, com.example.tosshelperappserver.common.constant.RoleType.class);

    public QMember(String variable) {
        super(Member.class, forVariable(variable));
    }

    public QMember(Path&lt;? extends Member&gt; path) {
        super(path.getType(), path.getMetadata());
    }

    public QMember(PathMetadata metadata) {
        super(Member.class, metadata);
    }

}</code></pre>
<p> 
생성된 QClass 내부를 살짝만 한번 들여다보고 가보자.
EntityPathBase를 상속받는데, 이는 엔티티에 대한 경로를 나타내는 QueryDSL의 추상 클래스이다.
멤버 변수로 매핑된 엔티티를 기반으로 자동 생성된 속성들이 존재한다.
 
Type으로 그냥 String이 아니라, StringPath, 그리고 createString 따위의 메서드를 사용하는 것을 볼 수 있는데, 이는 Builder Pattern을 활용하여 QType의 속성을 변경할 수 있도록 설계되었기 때문이다.</p>
<h2 id="2-querydsl-사용을-위한-factory를-bean으로-등록">2) QueryDSL 사용을 위한 Factory를 Bean으로 등록</h2>
<pre><code class="language-java">@Configuration
public class QueryDslConfig {

    @PersistenceContext
    private EntityManager entityManager;

    @Bean
    public JPAQueryFactory jpaQueryFactory() {
        return new JPAQueryFactory(entityManager);
    }
}</code></pre>
<p>JPAQueryFactory는 QueryDSL에서 제공하는 주요 클래스 중 하나이다. 해당 Config 파일을 만들어 JPAQueryFactory를 QueryDSL을 이용한 JPA 쿼리를 빌드하는 Factory 역할로서 사용할 수 있도록 Bean으로 등록시켜 두자.</p>
<h2 id="3-repository에서-사용하기">3) Repository에서 사용하기</h2>
<p> 
 </p>
<pre><code>@Repository
public interface MemberRepository extends JpaRepository&lt;Member, Long&gt; {


    // 쿼리 메서드
    Member findMemberByEmail(String email);

    // 동적 쿼리 생성
    @Query(value= &quot;select m from Member m where m.memberId = :id and m.email = :email&quot; )
    Member findMemberByIdAndEmail(@Param(&quot;id&quot;) Long id, @Param(&quot;email&quot;) String email);


}</code></pre><p> 
기존에 JpaRepository를 사용하여 다음과 같은 쿼리들을 작성해 두었다고 가정해보자, 이제 해당 Repository에서 QueryDsl을 사용한 메서드를 작성할 수 있도록 해 보자.</p>
<h2 id="4-custom-repository에서-querydsl을-사용하여--쿼리-빌드">4) Custom Repository에서 QueryDSL을 사용하여  쿼리 빌드</h2>
<p> </p>
<pre><code class="language-java">public interface MemberCustomRepository {

    Member findAllLeftFetchJoin(Long id);
}</code></pre>
<pre><code class="language-java">package com.example.tosshelperappserver.repository;
import com.example.tosshelperappserver.domain.Member;
import com.querydsl.jpa.impl.JPAQueryFactory;
import lombok.AllArgsConstructor;
import org.springframework.stereotype.Repository;
import static com.example.tosshelperappserver.domain.QMember.member;


@Repository
@AllArgsConstructor
public class MemberCustomRepositoryImpl implements MemberCustomRepository{

    private final JPAQueryFactory jpaQueryFactory;


    @Override
    public Member findAllLeftFetchJoin(Long id) {
        return jpaQueryFactory.selectFrom(member)
                .where(member.memberId.eq(id))
                .leftJoin(member.categories)
                .fetchJoin()
                .fetchOne();
    }
}</code></pre>
<p> 
JpaRepository는 인터페이스이기 때문에 코드를 구현할 수 없다. 따라서 CustomRepository를 작성해주자.
내부에서는 JPAQueryFactory를 Injection하여 사용하고 있다.
 
쿼리문은 Builder 패턴으로 작성된다. 쿼리문을 작성하는 내용에 대해서는 다음에 다루어볼 예정이지만 해당 쿼리문은 memberId와 일치하는 Member를 Categories와 함께 Join하는 쿼리문이다. 
눈썰미가 있다면 눈치챘을 수도 있겠지만, jpaQueryFactory에서 사용되는 쿼리문의 내부에는 QClass가 사용되고 있다.</p>
<h2 id="5-jparepository에-추가-및-service에서의-사용">5) JpaRepository에 추가 및 Service에서의 사용</h2>
<p> </p>
<pre><code class="language-java">@Repository
public interface MemberRepository extends JpaRepository&lt;Member, Long&gt;, MemberCustomRepository {


    // 쿼리 메서드
    Member findMemberByEmail(String email);

    // 동적 쿼리 생성
    @Query(value= &quot;select m from Member m where m.memberId = :id and m.email = :email&quot; )
    Member findMemberByIdAndEmail(@Param(&quot;id&quot;) Long id, @Param(&quot;email&quot;) String email);

}</code></pre>
<p> 
이제 해당 Repository를 JpaRepository에서 상속받아 QueryDsl을 사용한 메서드들을 사용할 수 있도록 하자.
 
 
 </p>
<pre><code class="language-java">@Override
public MemberWithCategoryDto getMemberInfoWithOwnCategory(Long id) {
    Member member = memberJpaRepository.findAllLeftFetchJoin(id);
    MemberWithCategoryDto dto = modelMapper.map(member, MemberWithCategoryDto.class);
    return dto;
}</code></pre>
<p> </p>
<p>출처: <a href="https://sjh9708.tistory.com/174">https://sjh9708.tistory.com/174</a> [데굴데굴 개발자의 기록:티스토리]</p>