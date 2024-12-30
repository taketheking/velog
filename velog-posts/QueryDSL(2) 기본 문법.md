<h1 id="2-querydsl-의-기본문법">2. Querydsl 의 기본문법</h1>
<h2 id="1-querydsl과-jpql-비교">1) Querydsl과 JPQL 비교</h2>
<p>먼저 Querydsl과 JPQL을 비교해보겠다.</p>
<p>Querydsl vs JPQL</p>
<pre><code class="language-java">JPAQueryFactory qf;

  @Test
  public void startJPQL(){

  // JPQL을 사용한 member1 찾기

    String qlString = &quot;select m from Member m &quot; +
                      &quot;where m.username = :username&quot;;

        Member findMember = em.createQuery(qlString, Member.class)
         .setParameter(&quot;username&quot;, &quot;member1&quot;)
         .getSingleResult();

         assertThat(findMember.getUsername()).isEqualTo(&quot;member1&quot;);

  }

  @Test
  public void startQuerydsl(){

  //Querydsl 사용한 member1 찾기
    JPAQueryFactory qf = new JPAQueryFactory(em);

    Memeber findMember =qf
                          .select(Qmember.member)
                          .from(Qmember.member)
                          .where(Qmember.member.name.eq(&quot;member1&quot;) //파라미터 바인딩 처리
                          .fetchOne();

    assertThat(findMember.getUsername()).isEqualTo(&quot;member&quot;1);

  }</code></pre>
<p>EntityManager 로 JPAQueryFactory 생성</p>
<p>Querydsl 은 JPQL 빌더</p>
<p>JPQL: 문자(실행 시점 오류), Querydsl: 코드(컴파일 시점 오류)
JPQL: 파라미터 바인딩 직접, Querydsl: 파라미터 바인딩 자동 처리</p>
<p>JPAQueryFactory 를 필드로 설정 할 수도 있다.</p>
<blockquote>
</blockquote>
<p>*<em>JPQQueryFactory를 필드로 제공하면 동시성 문제는 어떻게 될까? *</em>
동시성 문제는 JPAQueryFactory를 생성할 때 제공하는 EntiryManager(em)에 달려있다. 스프링 프레임워크는 여러 쓰레드에서 동시에 같은 EntityManager에 접근해도
트랙재션 마다 별도의 영속성 컨텍스트를 제공하기 때문에, 동시성 문제는 걱정하지 않아도 된다.</p>
<h2 id="2-qclass">2) Qclass</h2>
<p>기본적으로 QueryDsl을 사용할때 QClass가 생성이 된다.  초기에 QueryDSL을 사용하면서 궁금했던 내용은 그냥 Entity를 사용해도 될거같은데 굳이 QClass를 만들어서 사용을 할까? 어떻게 만드는거지? 라는 기본적인 궁금증에서 래퍼런스 문서 부터 많은 블로그의 내용을 찾아봤으며 해당 내용을 정리 해보려고 한다.
 
JPA_APT(JPAAnnotationProcessorTool)가 @Enttiy 와 같은 특정 어노테이션을 찾고 해당 클래스를 분석해서 QClass를 만들어 준다.  빌드 도구를 통해서 만드는 방법은 다른곳을 찾아봐도 나오니 생략한다. 
(Gradle의 경우, 버전별로 설정을 하는 방식이 다르기 때문에 버전에 맞게 잘 찾아서 사용 해야 한다.)
 </p>
<h3 id="✋-apt-란-">✋ APT 란 ?</h3>
<p>Annotation 이 있는 기존코드를 바탕으로 새로운 코드와 새로운 파일들을 만들 수 있고, 이들을 이용한 클래스에서 compile 하는 기능도 지원해준다.
쉬운 예시로는 Lombok의 @Getter, @Setter가 있다. 해당 어노테이션을 사용하는 경우 apt가 컴파일 시점에 해당 어노테이션을 기준으로 getter 와 setter를 만들어 주기 때문에 코드를 작성하지 않고 사용이 가능해진다.
 </p>
<h3 id="✋-qclass-란">✋ QClass 란?</h3>
<p>엔티티 클래스의 메타 정보를 담고 있는 클래스로, Querydsl은 이를 이용하여 타입 안정성(Type safe)을 보장하면서 쿼리를 작성할 수 있게 된다.
 
QClass는 엔티티 클래스와 대응되며  엔티티의 속성을 나타내고 있다. 이러한 QClass를 사용하여 쿼리를 작성하면 엔티티 속성을 직접 참조하고 조합하여 쿼리를 구성할 수 있다. QClass를 사용하면 컴파일 시점에 오류를 확인할 수 있고, IDE의 자동완성 기능을 활용하여 쿼리 작성을 보다 편리하게 할 수 있다.
 
그렇다면 굳이 엔티티 클래스 대신 Q클래스를 만들어서 사용하는 이유에 대해서 정리 하려고 한다. 
 
QClass와 엔티티 클래스는 많은 장점을 공유하고 있지만 그럼에 QClass를 사용하는 이유는 다음과 같다.</p>
<p>QClass는 엔티티 속성을 정적인 방식으로 표현하므로 IDE의 자동 완성 기능을 활용할 수 있고, 속성 이름을 직접 기억하거나 확인하지 않아도 된다는 장점을 가지고 있다. 
QClass는 엔티티 속성의 타입을 정확하게 표현하므로, 타입에 맞지 않는 연산이나 비교를 시도하면 컴파일러가 오류를 감지할 수 있다.</p>
<p>QClass는 엔티티 클래스의 확장으로 생각할 수 있다. 엔티티 클래스는 데이터베이스 테이블의 매핑을 담당하고, QClass는 쿼리 작성을 위한 편의성과 안전성을 제공을 해주면서 유지보수의 편의성 및 실수 방지를 하지 않도록 해준다고 생각한다.</p>
<p>기본 Q-Type 활용
Q클래스 인스턴스를 사용하는 3가지 방법</p>
<pre><code class="language-java">
QMember qMember = new QMember(&quot;m&quot;); //별칭 직접 설정
QMember qMember = QMember.member; //기본 인스턴스 사용

import static kbds.querydsl.domain.QMember.member; //static 상수 설정
</code></pre>
<blockquote>
<p>참고 : 같은 테이블을 조인해야하는 경우가 아니면 기본 인스턴스를 사용하자.
검색 조건 쿼리</p>
</blockquote>
<pre><code class="language-java"> @Test
 public void search(){

  Member findMember =  qf
    .selectFrom(member)
    .where(member.username.eq(&quot;member1&quot;)
    .and(member.age.eq(10))
    .fetchOne();

 }</code></pre>
<h2 id="2-기본-문법">2) 기본 문법</h2>
<h3 id="1-셋팅">(1) 셋팅</h3>
<p>사용할 데이터</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/45f5525f-0855-496a-aeb0-d079e45e17fc/image.png" /></p>
<p> 
 </p>
<ol>
<li>Author : Book = 1 : N
Author(저자)는 여러 개의 Book(책)을 가진다.
 </li>
<li>Author : Organization = N : 1
Author(저자)는 한곳의 Organization(조직)에 속한다.
 </li>
<li>Book : Review = 1 : N
Book(책)은 여러 개의 Review(리뷰)를 가진다.</li>
</ol>
<pre><code class="language-java">@Entity
@Table(name = &quot;Organization&quot;)
@Getter
@Setter
public class Organization {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String orgName;

    @OneToMany(mappedBy = &quot;organization&quot;, cascade = CascadeType.ALL)
    private List&lt;Author&gt; authors = new ArrayList&lt;&gt;();

}</code></pre>
<pre><code class="language-java">@Entity
@Table(name = &quot;Author&quot;)
@Getter
@Setter
public class Author {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToMany(mappedBy = &quot;author&quot;, cascade = CascadeType.ALL)
    private List&lt;Book&gt; book = new ArrayList&lt;&gt;();

    @ManyToOne(fetch=FetchType.LAZY)
    @JoinColumn(name = &quot;organization_id&quot;)
    private Organization organization;

}</code></pre>
<pre><code class="language-java">@Entity
@Table(name = &quot;Book&quot;)
@Getter
@Setter
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;

    @ManyToOne(fetch=FetchType.LAZY)
    @JoinColumn(name = &quot;author_id&quot;)
    private Author author;

    @OneToMany(mappedBy = &quot;book&quot;, cascade = CascadeType.ALL)
    private List&lt;Review&gt; reviews = new ArrayList&lt;&gt;();

}</code></pre>
<pre><code class="language-java">@Entity
@Table(name = &quot;Review&quot;)
@Getter
@Setter
public class Review {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String comment;

    @ManyToOne
    @JoinColumn(name = &quot;book_id&quot;)
    private Book book;

}</code></pre>
<h3 id="2-검색select">(2) 검색(Select)</h3>
<p> </p>
<h4 id="리스트-조회">리스트 조회</h4>
<pre><code>public List&lt;Book&gt; findBookList() {
    List&lt;Book&gt; result = queryFactory
            .selectFrom(book)
            .fetch();
    return result;
}</code></pre><p> 
 </p>
<h4 id="단일-조회">단일 조회</h4>
<pre><code>public Book findBookByTitle(String title) {
    Book result = queryFactory.selectFrom(book)
            .where(book.title.eq(title))
            .fetchOne();
    return result;
}</code></pre><p> </p>
<ul>
<li>selectFrom(QType) : 해당 엔티티에서 모든 컬럼 조회</li>
<li>fetch() : 리스트를 조회, 데이터가 존재하지 않을 시 빈 리스트를 반환</li>
<li>fetchOne() : 단일 조회, 결과가 없으면 Null, 결과가 단일이 아닐 시 Exception 발생</li>
<li>fetchFirst() : 단일 조회, 결과가 여러개여도 맨 처음 레코드 반환</li>
</ul>
<p> </p>
<h4 id="컬럼-선택하여-조회">컬럼 선택하여 조회</h4>
<pre><code>public List&lt;String&gt; findBookListTitle() {
    List&lt;String&gt; result = queryFactory.select(book.title)
            .from(book)
            .fetch();
    return result;
}</code></pre><ul>
<li>select(QType.Column1, QType.Column2) : 엔티티의 해당 컬럼(들) 조회</li>
<li>from(QType) : 조회 대상 엔티티</li>
</ul>
<h3 id="3-조건where">(3) 조건(Where)</h3>
<p> </p>
<pre><code class="language-java">public Book findBookByTitle(String title) {
    Book result = queryFactory.selectFrom(book)
            .where(book.title.eq(title))
            .fetchOne();
    return result;
}</code></pre>
<p> </p>
<ul>
<li>where(조건) : 해당 조건과 일치하는 레코드(들)을 반환</li>
</ul>
<p> 
 
 </p>
<h4 id="다양한-조건-함수">다양한 조건 함수</h4>
<p> </p>
<pre><code class="language-java">// 동일 여부
author.name.eq(&quot;John&quot;); // 일치
author.name.ne(&quot;John&quot;); // 일치X
author.name.isNotNull(); // NullX

// 포함
author.age.in(20, 30, 40); // 포함
author.age.notIn(25, 35, 45); // 미포함

// 문자열
author.name.like(&quot;J%&quot;); // LIKE : J로 시작
author.name.startsWith(&quot;J&quot;); // J로 시작
author.name.contains(&quot;Jo&quot;); // J 포함

// 수 비교
author.age.between(25, 35); // 25 ~ 35
author.age.lt(30); // &lt; 30
author.age.loe(30); // &lt;= 30
author.age.gt(30); // &gt; 30
author.age.goe(30); // &gt;= 30</code></pre>
<p> 
 </p>
<h4 id="복합-조건-연산">복합 조건 연산</h4>
<p> </p>
<pre><code class="language-java">public List&lt;Author&gt; findAuthorByCondition() {

    List&lt;Author&gt; result = queryFactory.selectFrom(author)
            .where(
                    author.age.notBetween(20, 30)
                            .and(author.age.gt(10))
                            .and(author.age.lt(50))
            )
            .fetch();

    return result;
}</code></pre>
<pre><code class="language-java">select * from author
where age NOT BETWEEN 20 and 30 
and age &gt; 10 and age &lt; 50;</code></pre>
<p> </p>
<ul>
<li>and(조건) : AND 복합 조건</li>
<li>or(조건) : OR 복합 조건</li>
</ul>
<p> 
 
 </p>
<pre><code class="language-java">public List&lt;Author&gt; findAuthorByCondition2() {

    List&lt;Author&gt; result = queryFactory.selectFrom(author)
            .where(
                    (
                            author.age.notBetween(20, 30)
                                    .and(author.age.gt(10))
                                    .and(author.age.lt(50))
                    ).or(
                            author.name.like(&quot;%John%&quot;)
                    )
            )
            .fetch();


    return result;

}</code></pre>
<pre><code class="language-java">select * from author
where (age NOT BETWEEN 20 and 30 
and age &gt; 10 and age &lt; 50) or
(name LIKE &quot;%John%&quot;);</code></pre>
<p> </p>
<ul>
<li>() 로 조건들을 묶어 표현식 만들 수 있다.</li>
</ul>
<p> 
 
 
 
 </p>
<h4 id="연관된-엔티티의-컬럼을-조건으로-사용">연관된 엔티티의 컬럼을 조건으로 사용</h4>
<p> </p>
<pre><code class="language-java">public List&lt;Book&gt; findBooksByAuthorName(String name) {
    List&lt;Book&gt; result = queryFactory
            .selectFrom(book)
            .where(book.author.name.eq(name))
            .fetch();
    return result;
}</code></pre>
<p> </p>
<ul>
<li>book.author.name 을 보면 연관된 엔티티의 컬럼을 조건으로 사용할 수 있다. (Book과 Author은 N:1 관계)</li>
<li>SQL 쿼리문으로는 아래와 같다.</li>
</ul>
<p> </p>
<pre><code class="language-java">SELECT Book.*
FROM Book
INNER JOIN Author ON Book.author_id = Author.id
WHERE Author.name = 'John Doe';</code></pre>
<p> </p>
<h3 id="4-정렬-order-by">(4) 정렬 (Order By)</h3>
<p> </p>
<pre><code>public List&lt;Book&gt; findBookListOrderBy() {
    List&lt;Book&gt; result = queryFactory
            .selectFrom(book)
            .orderBy(book.title.desc())
            .fetch();
    return result;
}</code></pre><p> </p>
<ul>
<li>orderBy(QType.Column.(정렬조건)) : 해당 컬럼을 정렬조건에 따라 정렬 (디폴트는 asc)</li>
</ul>
<p>정렬조건</p>
<ul>
<li>desc() : 내림차순</li>
<li>asc() : 올림차순</li>
</ul>
<p> </p>
<pre><code class="language-java">public List&lt;Book&gt; findBookListOrderBy() {
    List&lt;Book&gt; result = queryFactory
            .selectFrom(book)
            .orderBy(book.title.desc().nullsLast())
            .fetch();
    return result;
}</code></pre>
<p> 
 
 </p>
<ul>
<li>nullsLast(), nullsFirst() : Null 데이터에 대한 순서를 부여한다(끝, 시작)</li>
</ul>
<p> </p>
<h3 id="5-페이지네이션-offset-limit">(5) 페이지네이션 (Offset, Limit)</h3>
<p> </p>
<pre><code class="language-java">public List&lt;Book&gt; findBookListPagenation(int offset, int limit) {
    List&lt;Book&gt; result = queryFactory
            .selectFrom(book)
            .offset(offset)
            .limit(limit)
            .fetch();
    return result;
}</code></pre>
<p> </p>
<ul>
<li>offset(long x) : 0부터 시작하는 결과에 대한 오프셋(시작 위치)</li>
<li>limit(long x) : 쿼리 결과에 대한 최대치 제한(Limit)</li>
</ul>
<p> 
 </p>
<pre><code class="language-java">public List&lt;Book&gt; findBookListPagenation(int offset, int limit) {
    QueryResults&lt;Book&gt; res = queryFactory
            .selectFrom(book)
            .offset(offset)
            .limit(limit)
            .fetchResults();

    System.out.println(&quot;Total : &quot; + res.getTotal());
    System.out.println(&quot;Limit : &quot; + res.getLimit());
    System.out.println(&quot;Offset : &quot; + res.getOffset());
    List&lt;Book&gt; result = res.getResults();
    return result;
}</code></pre>
<ul>
<li>fetchResults : 결과를 가져올 때 페이지네이션 정보를 함께 가져온다. (total, limit, offset) 결과는 getResults()를 호출 시 얻을 수 있다.</li>
</ul>
<p> </p>
<h3 id="6-집계함수aggregation">(6) 집계함수(Aggregation)</h3>
<p> 
그룹 함수와 함께 주로 사용된다.</p>
<pre><code class="language-java">public List&lt;Tuple&gt; findAuthorAggregation() {
    List&lt;Tuple&gt; result = queryFactory
            .select(author.count(), author.age.avg())
            .from(author)
            .fetch();
    return result;
}</code></pre>
<p> </p>
<ul>
<li>count() : 집합의 행 수 계산</li>
<li>sum() : 합 계산</li>
<li>avg() : 평균 계산</li>
<li>max() : 최대 계산</li>
<li>min() : 최소 계산</li>
</ul>
<p> 
 </p>
<h3 id="7-그룹화-group-by-having">(7) 그룹화 (Group By, Having)</h3>
<p> 
 </p>
<pre><code class="language-java">public List&lt;Tuple&gt; findAuthorGroupByGender() {
    List&lt;Tuple&gt; result = queryFactory
            .select(author.gender, author.count(), author.age.avg())
            .from(author)
            .groupBy(author.gender)
            .fetch();
    return result;
}</code></pre>
<pre><code class="language-java">SELECT
    author.gender,
    COUNT(author.id),
    AVG(author.age)
FROM
    author
GROUP BY
    author.gender;</code></pre>
<p> </p>
<ul>
<li>groupBy(컬럼1, 컬럼2..) : 해당 컬럼을 기준으로 그룹화한다.</li>
</ul>
<p> 
 </p>
<pre><code class="language-java">public List&lt;Tuple&gt; findAuthorGroupBy() {
    List&lt;Tuple&gt; result = queryFactory
            .select(author.organization.orgName, author.count(), author.age.avg())
            .from(author)
            .groupBy(author.organization.id)
            .having(author.age.avg().gt(10))
            .fetch();
    return result;
}</code></pre>
<pre><code class="language-java">SELECT
    author_organization.org_name,
    COUNT(author.id),
    AVG(author.age)
FROM
    author
JOIN
    organization AS author_organization ON author.organization_id = author_organization.id
GROUP BY
    author.organization_id
HAVING
    AVG(author.age) &gt; 10;</code></pre>
<p> </p>
<ul>
<li>having(조건) : 특정 조건을 만족하는 그룹을 필터링한다.</li>
<li>해당 예시는 연관된 엔티티의 컬럼을 기준으로 GroupBy를 수행한 예시이다.</li>
</ul>
<p> </p>
<p>출처: <a href="https://ssow93.tistory.com/60">https://ssow93.tistory.com/60</a> [soTech:티스토리]
출처: <a href="https://sjh9708.tistory.com/175">https://sjh9708.tistory.com/175</a> [데굴데굴 개발자의 기록:티스토리]</p>