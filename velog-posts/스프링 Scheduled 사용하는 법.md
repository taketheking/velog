<blockquote>
<p>intro
팀 프로젝트를 하면서 쿠폰과 포인트가 만료기간이 지나면 자동으로 만료 상태로 변경되는 기능을 구현하고 싶었다.</p>
</blockquote>
<h1 id="1-scheduled">1. @Scheduled</h1>
<p>@Scheduled는 일정 시간마다 어떤 동작을 실행되도록 해주는 어노테이션이다.</p>
<p>먼저, Application Class에 @EnableScheduling을 추가하여 @Scheduled 가 붙은 메서드를 읽을 수 있도록 한다.</p>
<pre><code class="language-java">@EnableScheduling
@EnableJpaAuditing
@SpringBootApplication
public class HexaDeliveryApplication {

    public static void main(String[] args) {
        SpringApplication.run(HexaDeliveryApplication.class, args);
    }

}
</code></pre>
<p>그리고 실제로 스케줄링 작업이 담긴 메서드를 만든다. 이 메서드는 @Component(@Service 등) 즉, 스프링 빈에 등록된 클래스의 안에 있어야 한다.</p>
<pre><code class="language-java">@Scheduled(cron = &quot;0 0 0 * * *&quot;)
    @Transactional
    public void clearCoupons() {
        log.info(&quot;clear coupons&quot;);
        List&lt;Coupon&gt; coupons = couponRepository.findAllByNORMAL();

        // coupon.getExpirationTime().isAfter(now)
        //만료기한 지난거 상태 변경
        LocalDate now = LocalDate.now();
        for (Coupon coupon : coupons) {
            if(now.isAfter(coupon.getExpirationTime())){
                coupon.updateStatus2Delete();
            }
        }

        // 하루 제한 발급량 초기화
        for (Coupon coupon : coupons) {
            coupon.resetToDayQuantity();
        }
        couponRepository.saveAll(coupons);
    }</code></pre>
<p>이렇게 설정하면 매일 00:00 시각에 해당 메서드가 실행된다.</p>
<p>여기서는 만료기간이 지난 쿠폰을 제거 상태로 변경하고, 하루 제한 발급량을 초기화하는 기능을 구현했다.</p>
<p>하지만 여기서 문제가 실무에서는 쿠폰이 몇 천만장씩 사용되고 만료가 될텐데,
이렇게 다 상태를 변경하게 되면 DB가 터질 수 있다.</p>
<p>그렇기 때문에 이렇게 매일 상태를 변경하는 방법보다 쿠폰을 사용하거나 정보를 가져올 때 만료 기간을 인덱스로 줘서 걸러서 가지고 오는 방법으로 해결하는 것이 좋다. </p>
<p>참고 : <a href="https://dev-coco.tistory.com/176">https://dev-coco.tistory.com/176</a></p>