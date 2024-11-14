<h1 id="1-같은-이름의-bean-등록">1. 같은 이름의 Bean 등록</h1>
<h2 id="--자동-bean-등록-vs-자동-bean-등록">- 자동 Bean 등록 VS 자동 Bean 등록</h2>
<pre><code class="language-java">        public interface ConflictService {
            void test();
        }

        // Bean의 이름을 service로 설정
        @Component(&quot;service&quot;)
        public class ConflictServiceV1 implements ConflictService {
            @Override
            public void test() {
                System.out.println(&quot;Conflict V1&quot;);
            }
        }

        // Bean의 이름을 service로 설정
        @Component(&quot;service&quot;)
        public class ConflictServiceV2 implements ConflictService {
            @Override
            public void test() {
                System.out.println(&quot;Conflict V2&quot;);
            }
        }

        // componentScan의 범위를 conflict 패키지 하위로 설정
        @ComponentScan(basePackages = &quot;com.example.springconcept.conflict&quot;)
        public class ConflictApp {
            public static void main(String[] args) {
                ApplicationContext context = new AnnotationConfigApplicationContext(ConflictApp.class);

                // Service 빈을 가져와서 실행
                ConflictService service = context.getBean(ConflictService.class);

                service.test();
            }
        }</code></pre>
<ul>
<li><code>ConflictingBeanDefinitionException</code> 발생</li>
</ul>
<h2 id="--수동-bean-등록-vs-자동-bean-등록">- 수동 Bean 등록 VS 자동 Bean 등록</h2>
<pre><code class="language-java">        // conflictService 이름으로 Bean 생성
        @Component
        public class ConflictService implements MyService {
            @Override
            public void doSomething() {
                System.out.println(&quot;ConflictService 메서드 호출&quot;);
            }
        }

        public class ConflictServiceV2 implements MyService {
            @Override
            public void doSomething() {
                System.out.println(&quot;ConflictServiceV2 메서드 호출&quot;);
            }
        }

        // 수동으로 Bean 등록
        @Configuration
        public class ConflictAppConfig {

                // conflictService 이름으로 Bean 생성
            @Bean(name = &quot;conflictService&quot;)
            MyService myService() {
                return new ConflictServiceV2();
            }

        }

        @ComponentScan(basePackages = &quot;com.example.springconcept.conflict2&quot;)
        public class ConflictApp2 {
            public static void main(String[] args) {
                ApplicationContext context = new AnnotationConfigApplicationContext(ConflictApp2.class);

                // Service 빈을 가져와서 실행
                MyService service = context.getBean(MyService.class);

                service.doSomething();
            }
        }     </code></pre>
<ul>
<li>수동 Bean 등록이 자동 Bean 등록을 오버라이딩해서 우선권을 가진다.</li>
<li>의도한 결과라면 다행이지만, 아닌 경우(실수)가 대부분이다. → 버그 발생</li>
<li>Spring Boot에서는 수동과 자동 Bean등록의 충돌이 발생하면 오류가 발생한다.</li>
</ul>
<blockquote>
<p>⛔ <strong>주의사항</strong></p>
</blockquote>
<ul>
<li>테스트 시 자동 빈 등록 충돌 <strong><code>conflict</code></strong> 패키지의 Bean Name을 지워주세요.</li>
<li><strong>테스트 시 반드시 <code>@SpringBootApplication</code> 으로 실행해야 합니다.</strong></li>
</ul>
<pre><code>Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=true
            ```

- 설정 변경(application.properties)

```java
        // 수동, 자동 Bean을 동시에 등록할 때 이름이 같으면 수동 Bean이 오버라이딩
        spring.main.allow-bean-definition-overriding=true 

        // 기본값
        spring.main.allow-bean-definition-overriding=false</code></pre><h1 id="2같은-타입의-bean-충돌-해결-방법">2.같은 타입의 Bean 충돌 해결 방법</h1>
<h2 id="1-autowired--필드명-사용">1) @Autowired + 필드명 사용</h2>
<ul>
<li><code>@Autowired</code> 는 타입으로 먼저 주입을 시도하고 같은 타입의 Bean이 여러개라면 필드 이름 혹은 파라미터 이름으로 매칭한다.<pre><code class="language-java">public interface MyService { ... }
</code></pre>
</li>
</ul>
<p>@Component
public class MyServiceImplV1 implements MyService { ... }</p>
<p>@Component
public class MyServiceImplV2 implements MyService { ... }</p>
<p>@Component
public class ConflictApp {</p>
<pre><code>// 필드명을 Bean 이름으로 설정
@Autowired
private MyService myServiceImplV2;
...</code></pre><p>}</p>
<pre><code>
## 2) @Qualifier 사용
- Bean 등록 시 추가 구분자를 붙여 준다.
 - 생성자 주입, setter 주입 사용 가능

```java
  @Component
@Qualifier(&quot;firstService&quot;)
public class MyServiceImplV1 implements MyService { ... }

@Component
@Qualifier(&quot;secondService&quot;)
public class MyServiceImplV2 implements MyService { ... }

@Component
public class ConflictApp {

        private MyService myService;

        // 생성자 주입에 구분자 추가
        @Autowired
        public ConflictApp(@Qualifier(&quot;firstService&quot;) MyService myService) {
                this.myService = myService;
        }

        // setter 주입에 구분자 추가
        @Autowired
        public void setMyService(@Qualifier(&quot;firstService&quot;) MyService myService) {
                this.myService = myService;
        }
    ...
}</code></pre><h2 id="3-primary-사용">3. @Primary 사용</h2>
<ul>
<li><code>@Primary</code>로 지정된 Bean이 우선 순위를 가진다.</li>
</ul>
<pre><code class="language-java">@Component
public class MyServiceImplV1 implements MyService { ... }

@Component
@Primary
public class MyServiceImplV2 implements MyService { ... }

@Component
public class ConflictApp {

        private MyService myService;

        @Autowired
        public ConflictApp(MyService myService) {
                this.myService = myService;
        }
    ...
}</code></pre>
<ul>
<li><strong>실제 적용 사례</strong><ul>
<li>Database가 (메인 MySQL, 보조 Oracle) 두개 존재하는 경우<ul>
<li>기본적으로 MySQL을 사용할 때 <code>@Primary</code>를 사용하면 된다.</li>
<li>필요할 때 <code>@Qualifier</code>로 Oracle을 사용하도록 만들 수 있다.</li>
<li>동시에 사용되는 경우 <code>@Qualifier</code> 의 우선순위가 높다.</li>
</ul>
</li>
</ul>
</li>
</ul>
<blockquote>
</blockquote>
<p>💡 같은 타입의 Bean이 여러개 조회되었지만 모든 Bean이 필요하다면, Java의 자료구조 Map, List를 사용하는 방법도 있다.</p>