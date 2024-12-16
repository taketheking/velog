<h2 id="배경">배경</h2>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/e593019c-bd4d-469c-92b0-bf1f74970cb7/image.PNG" /></p>
<p>위의 코드를 보면 reservation을 조회할 때, 자식 엔티티인 user와 item의 멤버변수가 필요하면 같이 조회한다.</p>
<h2 id="문제-발생">문제 발생</h2>
<blockquote>
<p>reservation을 한 번 조회하는데 총 3번의 쿼리가 발생한다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/5282732e-5da9-4c4b-bd88-111ace385fdd/image.PNG" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/5c0c5b39-7727-4284-952c-e7300d5e30cd/image.PNG" /></p>
<p>이렇게 한 번은 큰 차이가 없을 지 몰라도, 계속되는 조회에서 위에 처럼 1번 조회에 3번의 접근이 발생하면 점점 성능 문제가 발생할 것이다.</p>
<h2 id="문제-해결-노력">문제 해결 노력</h2>
<h3 id="1-fetch-사용">1. Fetch 사용</h3>
<p>연관된 엔티티가 있는 엔티티를 조회할 때, <code>@OneToMany</code>, <code>@ManyToOne</code> 등에 대해 적절한 지연 로딩(Lazy)과 즉시 로딩(Eager)을 설정합니다. </p>
<p>기본적인 설정은 다음과 같다.</p>
<blockquote>
</blockquote>
<p><code>@ManyToOne</code> 은 FetchType.EAGER
<code>@OneToMany</code> 은 FetchType.LAZY </p>
<ul>
<li>기본적으로 <code>FetchType.LAZY</code>를 사용하는 것이 좋다.</li>
<li><code>FetchType.LAZY</code>는 필요할 때만 명시적으로 데이터를 가져와 N+1 문제를 방지한다.</li>
</ul>
<p>하지만 이것만으로는 근본적인 해결이 되지 않는다. 결국엔 user와 item을 접근하려면 쿼리를 위와 똑같이 보내야하기 때문이다.</p>
<p>그래서 적은 양의 고정적인 개수가 연관되어 있을 때는 오히려 <code>FetchType.EAGER</code> 가 성능에 좋을 수 있다.</p>
<h3 id="2-entitygraph-사용">2. EntityGraph 사용</h3>
<blockquote>
</blockquote>
<p>연관관계가 있는 엔티티를 조회할 경우 지연 로딩으로 설정되어 있으면 연관관계에서 종속된 엔티티는 쿼리 실행 시 select 되지 않고 proxy 객체를 만들어 엔티티가 적용시킨다. 
그 후 해당 프락시 객체를 호출할 때마다 그때그때 select 쿼리가 실행된다.</p>
<blockquote>
</blockquote>
<p>위 같은 연관관계가 지연 로딩으로 되어있을 경우 fetch 조인을 사용하여 여러 번의 쿼리를 한 번에 해결할 수 있다.</p>
<p>@EntityGraph는 Data JPA에서 fect 조인을 어노테이션으로 사용할 수 있도록 만들어 준 기능이다.</p>
<pre><code class="language-java">    @Override
    @EntityGraph(attributePaths = {&quot;user&quot;, &quot;item&quot;})
    List&lt;Reservation&gt; findAll();</code></pre>
<p>이렇게 EntityGraph 을 사용해서 자식 엔티티과 함께 부모 엔티티를 조회하면 아래처럼 left join을 통해 한 번의 쿼리로 모두 조회해서 정보를 가지고 오게 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/bef24d35-941a-48bb-95c9-6e988c44fa59/image.PNG" /></p>
<h3 id="3-join-fetch-사용">3. JOIN FETCH 사용</h3>
<blockquote>
<p>JOIN FETCH은 SQL에서 이야기하는 조인의 종류는 아니다. JPQL에서 성능 최적화를 위해 제공하는 조인의 종류이다.
JPQL에서 성능 최적화를 위해 제공하는 기능으로 연관된 엔티티나 컬렉션을 한 번에 같이 조회할 수 있는 기능이다.</p>
</blockquote>
<pre><code class="language-java">    @Query(&quot;SELECT r FROM Reservation r JOIN FETCH r.item JOIN FETCH r.user &quot;)
    List&lt;Reservation&gt; findAllWithFetch();</code></pre>
<p>이렇게 하면 reservation을 조회할 때, 하나의 쿼리로 item과 user을 같이 조회한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/ee1730e3-5305-4d7f-bf6f-22de88870fa9/image.PNG" /></p>
<p>그런데 위와 같이 2번의 쿼리가 발생했다.</p>
<p>자세히 살펴보니 item 엔티티에서 owner와 manager에서 user의 정보가 필요해서 쿼리를 보내는 것이었다.</p>
<pre><code class="language-java">    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = &quot;owner_id&quot;)
    private User owner;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = &quot;manager_id&quot;)
    private User manager;</code></pre>
<p>그래서 해당 연관관계를 <code>FetchType.LAZY</code>로 설정하니 user에 대한 쿼리가 발생하지 않았다.
즉, 한 번의 쿼리로 reservation과 item, user을 조회되었다.</p>
<p>아마도 이것은 JOIN FETCH 가 LAZY인 연관관계에서 적용되기 때문이라 생각된다.</p>
<h2 id="오해했던-fetchtypeeager">오해했던 FetchType.EAGER</h2>
<p>기본적으로<code>@ManyToOne</code> 은 FetchType.EAGER 인데, 왜 즉시로딩 때 한번에 조인해서 가져오지 않을까?</p>
<pre><code class="language-java">@Entity
@Getter
public class Reservation {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    @JoinColumn(name = &quot;item_id&quot;)
    private Item item;

    @ManyToOne
    @JoinColumn(name = &quot;user_id&quot;)
    private User user;

    private LocalDateTime startAt;

    private LocalDateTime endAt;

    @Enumerated(EnumType.STRING)
    private ReservationStatus status; // PENDING, APPROVED, CANCELED, EXPIRED

    public Reservation(Item item, User user, ReservationStatus status, LocalDateTime startAt, LocalDateTime endAt) {
        this.item = item;
        this.user = user;
        this.status = status;
        this.startAt = startAt;
        this.endAt = endAt;
    }

    public Reservation() {}

    public void updateStatus(ReservationStatus status) {
        this.status = status;
    }
}</code></pre>
<blockquote>
<p>여기서 주의할 것이 FetchType.EAGER 가 쿼리에서 한 번에 join을 통해 가져온다는 의미가 아니다!</p>
</blockquote>
<p>FetchType.EAGER는 당장 연관관계의 필드에 접근하지않아도 일단 조회해서 가져온다는 의미이지 join으로 가져온다는 것이 아니다. 오히려 join을 통해 가져올 때 비효율이 발생할 수도 있다.</p>
<p>그래서 N+1 문제가 발생한다.</p>
<h2 id="결론">결론</h2>
<blockquote>
<p>N+1 문제는 join을 해서 가져오는 EntityGraph나 join fetch 방법을 사용해서 조인을 통해 하나의 쿼리로 필요한 정보를 가져오도록 해야하는 것이다.</p>
</blockquote>