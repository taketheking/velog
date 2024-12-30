<h1 id="낙관적-락-비관적-락에-대해">낙관적 락, 비관적 락에 대해</h1>
<p>JPA를 사용하여 데이터베이스와 연결된 애플리케이션을 개발할 때, 동시성 처리와 관련된 이슈가 발생할 수 있다. 이러한 이슈를 해결하기 위한 방법 중 하나는 락(lock)을 사용하는 것이다. 이번 포스팅에는 낙관적 락과 비관적 락에 대해 알아보고, 예제코드와 이를 사용하는 이유 및 장단점을 함께 써보겠다.</p>
<h2 id="낙관적-락optimistic-lock">낙관적 락(Optimistic Lock)</h2>
<p>낙관적 락은 충돌이 거의 발생하지 않을 것이라고 가정하고, 충돌이 발생한 경우에 대비하는 방식이다. 낙관적 락은 JPA에서 버전(Version) 속성을 이용하여 구현할 수 있다. 낙관적 락의 특징으로는 충돌 발생확률이 낮고, 지속적인 락으로 인한 성능저하를 막을 수 있다.아래는 예시 코드이다.</p>
<p><strong>1. Entity</strong></p>
<pre><code class="language-java">@Entity
public class SampleEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Version
    private Long version;

    private String data;
}</code></pre>
<p><strong>2. Repository</strong></p>
<pre><code class="language-java">@Repository
public interface SampleEntityRepository extends JpaRepository&lt;SampleEntity, Long&gt; {
}</code></pre>
<p><strong>3. Service</strong></p>
<pre><code class="language-java">@Service
public class SampleEntityService {
    @Autowired
    private SampleEntityRepository sampleEntityRepository;

    public SampleEntity updateData(Long id, String newData) {
        SampleEntity sampleEntity = sampleEntityRepository.findById(id).orElseThrow();
        sampleEntity.setData(newData);
        return sampleEntityRepository.save(sampleEntity);
    }
}</code></pre>
<p>작동원리로는 SampleEntity 클래스에 @Version 어노테이션을 이용하여 version 필드로 지정한다. 데이터를 수정할 때 같은 id 값이지만 다른 사용자에 의한 변경이 발생하면 version 값이 다르게 되고, 이때 예외가 발생하므로 충돌로부터 안전하게 처리할 수 있다.</p>
<h3 id="예제">예제</h3>
<p>@Version Annotation
JPA에서 version 속성을 정의할때 지켜야하는 몇가지 규칙이 있습니다.</p>
<p>각 Entity Class에는 @Version 속성이 하나만 있어야 한다
여러 테이블에 매핑된 Entity의 경우 기본 테이블에 배치되어야 한다
버전에 타입은 int , Integer , long , Long , short , Short , java.sql.Timestamp 중 하나여야 한다
이 field 의 값 혹은 시간이 처음 조회될 때의 버전과 commit될때의 버전이 서로 다르다면 이는 충돌이 발생한 것으로 판단하고 예외를 발생시킵니다.</p>
<p>재고를 차감하는 예를 들어보겠습니다.
치킨A라는 재고는 현재 단 한개가 남아 있습니다.</p>
<blockquote>
</blockquote>
<p>[transaction-1] : 치킨A의 재고를 확인 / 치킨A 재고: 1개, version: 1
[transaction-2] : 치킨A의 재고를 확인 / 치킨A 재고: 1개, version: 1</p>
<blockquote>
</blockquote>
<p>-- 이때 두 트랜잭션 중 transaction-1 가 먼저 완료되었다고 가정해보겠습니다.</p>
<blockquote>
</blockquote>
<p>[transaction-1] : 치킨A를 구매 / 치킨A 재고: 0개, version: 2 로 업데이트하고 커밋
[transaction-2] : 치킨A를 구매 / 치킨A 재고: 0개, version: 2 로 업데이트하고 커밋하려는데 version이 처음 조회했던 1이 아니라 [transaction-1]에서 2로 변경되어 현재 조회한 버전과 다르므로 업데이트 실패</p>
<blockquote>
</blockquote>
<pre><code>update stock
set
    availableStock = ?,
    version = 2
where
    id = ?
    and version = 1</code></pre><p>위와 같은 쿼리가 발생하지만 해당 재고의 version은 transaction-1 으로 인해 이미 2로 증가된 상태입니다. 이때 처음 조회했던 version값인 1을 전달하게 되니 업데이트할 대상을 찾지 못해 예외가 발생합니다.</p>
<h2 id="비관적-락pessimistic-lock">비관적 락(Pessimistic Lock)</h2>
<p>비관적 락은 충돌이 발생할 확률이 높다고 가정하여, 실제로 데이터에 액세스 하기 전에 먼저 락을 걸어 충돌을 예방하는 방식이다. 비관적 락은 JPA에서 제공하는 LockModeType을 사용하여 구현할 수 있다.아래는 예시코드이다1. </p>
<p><strong>Repository</strong></p>
<pre><code class="language-java">@Repository
public interface SampleEntityRepository extends JpaRepository&lt;SampleEntity, Long&gt; {
}
2. Service
@Service
public class SampleEntityService {
    @Autowired
    private SampleEntityRepository sampleEntityRepository;

    @Autowired
    private EntityManager entityManager;

    @Transactional
    public SampleEntity updateDataWithPessimisticLock(Long id, String newData) {
        SampleEntity sampleEntity = entityManager.find(SampleEntity.class, id, LockModeType.PESSIMISTIC_WRITE);
        sampleEntity.setData(newData);
        entityManager.flush();
        return sampleEntity;
    }
}</code></pre>
<p> 
위 코드를 설명해보면 EntityManager의 find 메소드에 락 타입(LockModeType.PESSIMISTIC_WRITE)을 지정하여 데이터에 락을 걸어두고, 변경 작업이 끝난 후에 락을 해제한다. 이를 통해 다른 트랜잭션이 동시에 수정할 수 없어 동시성 처리 이슈를 방지할 수 있게 된다.</p>
<hr />
<h2 id="장단점">장단점</h2>
<p>이제 낙관적 락과 비관적 락에 대해 알아보았으니 이들의 장단점에 대해서도 알아보자</p>
<h3 id="낙관적-락">낙관적 락</h3>
<p>장점: 리소스 경쟁이 적고 락으로 인한 성능저하가 적다.
단점: 충돌 발생 시 처리해야 할 외부 요인이 존재한다.</p>
<h3 id="비관적-락">비관적 락</h3>
<p>장점: 충돌 발생을 미연에 방지하고 데이터의 일관성을 유지할 수 있다.
단점: 동시 처리 성능 저하 및 교착상태(Deadlock) 발생 가능성이 있다.</p>
<p>위에서 알아본 내용처럼 낙관적 락과 비관적 락은 각각의 사용 상황에 따라 선택할 수 있다. 충돌 발생 확률이 낮고 성능 저하를 예방하려면 낙관적 락을 사용하면 되고, 충돌을 미연에 방지하고 데이터의 일관성을 유지하려면 비관적 락을 사용하면 된다. 선택한 락 방식에 따라 JPA를 효과적으로 사용하여 동시성 처리 이슈를 해결할 수 있다.</p>
<p>출처: <a href="https://mozzi-devlog.tistory.com/37">https://mozzi-devlog.tistory.com/37</a> [우당탕탕:티스토리]
출처: <a href="https://devoong2.tistory.com/entry/JPA-%EC%97%90%EC%84%9C-%EB%82%99%EA%B4%80%EC%A0%81-%EB%9D%BDOptimistic-Lock%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%B4-%EB%8F%99%EC%8B%9C%EC%84%B1-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0">https://devoong2.tistory.com/entry/JPA-%EC%97%90%EC%84%9C-%EB%82%99%EA%B4%80%EC%A0%81-%EB%9D%BDOptimistic-Lock%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%B4-%EB%8F%99%EC%8B%9C%EC%84%B1-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0</a></p>