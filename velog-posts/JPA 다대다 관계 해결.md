<blockquote>
<p>intro
팀 프로젝트에서 유저와 쿠폰 엔티티 사이를 다대다 연관관계로 맺어야 되는 문제가 발생했다.</p>
</blockquote>
<p>관계형 데이터베이스는 정규화된 2개의 테이블로 다대다 관계를 표현할 수 없다.
그리고 다대다는 연관관계 설정하는 것도 문제이지만 각 엔티티의 관계도가 복잡해진다.</p>
<p>그래서 이것을 풀기위해 중간 테이블을 놓기로 했다.
그리고 중간 테이블을 각각 유저(1):중간테이블(N) 과 중간테이블(N):쿠폰(1) 의 관계로 설정해서 다대다 문제를 해결했다.</p>
<ul>
<li>ERD 관계도</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/ce7f151e-4d30-4d62-9e95-fe799e853d47/image.PNG" /></p>
<ul>
<li><p>유저 테이블</p>
<pre><code class="language-java">@Entity
@Table(name = &quot;user&quot;)
@Getter
public class User extends BaseEntity {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @Column(nullable = false)
  @Enumerated(EnumType.STRING)
  private Role role;

  @Column(nullable = false, length = 320, unique = true)
  private String email;

  @Column(nullable = false)
  private String password;

  @Column(nullable = false)
  private String name;

  @Column(nullable = false)
  private String phone;

  @Column (nullable = false)
  @Enumerated(EnumType.STRING)
  private Status status = Status.NORMAL;

  @OneToMany(mappedBy = &quot;user&quot;, cascade = CascadeType.ALL, orphanRemoval = true)
  private List&lt;UserCoupon&gt; userCouponList = new ArrayList&lt;&gt;();

</code></pre>
</li>
</ul>
<pre><code>...</code></pre><p>}</p>
<pre><code>

- 중간 테이블 (유저-쿠폰 테이블)
```java
@Getter
@Entity
@Table(name = &quot;user-coupon&quot;)
public class UserCoupon extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long userCouponId;

    @Enumerated(EnumType.STRING)
    private UserCouponStatus status;

    private LocalDateTime usedAt;

    @ManyToOne
    @JoinColumn(name = &quot;coupon_id&quot;, nullable = false)
    private Coupon coupon;

    @ManyToOne
    @JoinColumn(name = &quot;user_id&quot;)
    private User user;


    ...
}</code></pre><ul>
<li><p>쿠폰 테이블</p>
<pre><code class="language-java">@Getter
@Entity
@Table(name = &quot;coupon&quot;)
public class Coupon extends BaseEntity {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long couponId;

  @Column(nullable = false)
  @Enumerated(EnumType.STRING)
  private CouponType couponType;

  @Column(nullable = false)
  private Integer amount;

  // 현재로부터 일주일 뒤
  @Column(nullable = false)
  private LocalDate expirationTime;

  private Integer maxDiscountAmount;

  @Column(nullable = false)
  private Integer totalQuantity;

  private Integer todayQuantity;

  private Integer todayFixQuantity;

  @OneToMany(mappedBy = &quot;coupon&quot;, cascade = CascadeType.ALL, orphanRemoval = true, fetch = FetchType.EAGER)
  private List&lt;UserCoupon&gt; userCouponList = new ArrayList&lt;&gt;();

  @ManyToOne
  @JoinColumn(name = &quot;store_id&quot;)
  private Store store;

  @Column(nullable = false)
  @Enumerated(EnumType.STRING)
  private Status status;</code></pre>
</li>
</ul>