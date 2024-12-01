<h1 id="1-빈bean이란">1. 빈(Bean)이란?</h1>
<p>스프링에서 빈은 컨테이너가 생성하고 관리하는 객체를 말합니다. 빈은 애플리케이션의 서비스, DAO, 컨트롤러와 같은 다양한 역할을 수행하는 클래스의 인스턴스를 나타냅니다.</p>
<h3 id="빈의-주요-특징">빈의 주요 특징</h3>
<p><strong>스프링 컨테이너 관리</strong>: 빈은 스프링 컨테이너가 생성 및 소멸을 제어합니다.
<strong>의존성 주입 지원</strong>: 빈 간의 의존 관계를 설정하면 컨테이너가 이를 주입합니다.
<strong>싱글톤 기본</strong>: 빈은 기본적으로 싱글톤으로 관리되며, 필요 시 다른 스코프를 설정할 수 있습니다.
<strong>라이프사이클 관리</strong>: 초기화와 소멸 콜백 메서드 등을 지원합니다.</p>
<h1 id="2-빈-등록-방법">2. 빈 등록 방법</h1>
<p>스프링에서 빈을 등록하는 방법은 XML, 자바 기반 설정, 어노테이션 기반 설정으로 나눌 수 있습니다.</p>
<h3 id="1-xml-설정">1) XML 설정</h3>
<pre><code class="language-xml">&lt;bean id=&quot;exampleBean&quot; class=&quot;com.example.ExampleService&quot; /&gt;</code></pre>
<h3 id="2-자바-기반-설정">2) 자바 기반 설정</h3>
<pre><code class="language-java">@Configuration
public class AppConfig {
    @Bean
    public ExampleService exampleService() {
        return new ExampleService();
    }
}</code></pre>
<h3 id="3-어노테이션-기반-설정">3) 어노테이션 기반 설정</h3>
<pre><code class="language-java">@Component
public class ExampleService {
    public void performTask() {
        System.out.println(&quot;Task performed!&quot;);
    }
}</code></pre>
<p>이 경우, @ComponentScan으로 패키지를 지정해줘야 빈이 등록됩니다:</p>
<pre><code class="language-java">@Configuration
@ComponentScan(basePackages = &quot;com.example&quot;)
public class AppConfig {
}</code></pre>
<h1 id="3-빈의-스코프">3. 빈의 스코프</h1>
<p>스프링 빈은 <strong>스코프(scope)</strong>를 설정하여 생성 및 관리 방식을 정의할 수 있습니다.</p>
<table>
<thead>
<tr>
<th>스코프</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>싱글톤</td>
<td>기본값. 애플리케이션 컨텍스트당 하나의 인스턴스만 생성</td>
</tr>
<tr>
<td>프로토타입</td>
<td>요청마다 새로운 인스턴스를 생성</td>
</tr>
<tr>
<td>요청</td>
<td>HTTP 요청당 하나의 인스턴스 생성</td>
</tr>
<tr>
<td>세션</td>
<td>HTTP 세션당 하나의 인스턴스 생성</td>
</tr>
<tr>
<td>애플리케이션</td>
<td>웹 애플리케이션당 하나의 인스턴스 생성</td>
</tr>
<tr>
<td>웹소켓</td>
<td>WebSocket 세션당 하나의 인스턴스 생성</td>
</tr>
<tr>
<td>커스텀</td>
<td>비즈니스 요구사항에 따라 정의</td>
</tr>
</tbody></table>
<h3 id="1-싱글톤">1) 싱글톤</h3>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/35c93eb7-2f86-4554-b014-ed3896c0abab/image.png" /></p>
<ul>
<li><p>기본으로 설정되는 스코프이다.</p>
</li>
<li><p>Spring 컨테이너 내에서 Bean이 하나만 생성되고 모든 요청이 같은 객체를 사용한다.</p>
</li>
<li><p>대부분의 Sevice, Repository 등 Application 전체에서 공유되는 Bean</p>
</li>
<li><p>상태를 가지면 안된다.(상태가 공유됨)</p>
</li>
</ul>
<blockquote>
<p>생성되는 객체들</p>
</blockquote>
<ul>
<li>ApplicationContext</li>
<li>Spring Core Containers</li>
<li>Service Layer Beans</li>
<li>Repository Layer Beans</li>
<li>Configuration Beans</li>
<li>Spring Security Configurations</li>
</ul>
<h3 id="2-프로토타입">2) 프로토타입</h3>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/519d1b21-dffa-49f2-8171-75dcd1da3230/image.png" /></p>
<ul>
<li>스프링 컨테이너에게 요청할 때마다 새로운 인스턴스가 생성된다.</li>
<li>필요한 의존관계를 주입한다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/ae1f86b8-e84d-4c2a-9245-ddaa9f19fb72/image.png" /></p>
<ul>
<li><p>생성한 프로토타입 Bean을 클라이언트에게 반환한다.</p>
</li>
<li><p>컨테이너는 프로토타입 Bean의 생성, 의존관계 주입, 초기화 까지만 수행한다.</p>
</li>
<li><p>그 이후 생명주기(소멸, <code>@PreDestroy</code>)는 관리하지 않는다.</p>
<ul>
<li>클라이언트가 직접 종료 메서드를 호출해야 한다.</li>
</ul>
</li>
<li><p>매번 새로운 인스턴스가 필요한 경우</p>
</li>
<li><p>상태를 가지는 객체(특정 설정값이 다른 임시 작업 객체)</p>
</li>
</ul>
<blockquote>
<p>생성되는 객체들</p>
</blockquote>
<ul>
<li>상태를 가지는 Bean</li>
<li>멀티스레드 환경의 객체</li>
<li>동적 프록시 객체</li>
<li>요청별 새로운 인스턴스가 필요한 객체</li>
</ul>
<h4 id="싱글톤과-프로토-타입의-차이">싱글톤과 프로토 타입의 차이</h4>
<p>두 스코프의 차이는 싱글톤 스코프는 스프링 컨테이너가 올라올 때 시작해서 종료할 때 끝나지만, 프로토타입은 위에서 설명한대로 클라이언트가 요청할 때 마다 각각 생성되고, 생성 - 의존관계 주입 - 초기화 이후로는 관리 권한이 클라이언트에게 넘어가 종료 메소드는 호출 되지 않습니다.</p>
<blockquote>
</blockquote>
<ul>
<li>싱글톤 스코프 테스트 코드
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/e845568a-c91e-4cc9-8861-7c9f17527007/image.png" /><blockquote>
</blockquote>
</li>
<li>싱글톤 스코프 테스트 결과
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/48ab86df-024d-4e0f-b154-490b3d4f4a9b/image.png" /><blockquote>
</blockquote>
</li>
<li>프로토타입 스코프 테스트 코드
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/b01fa54c-abaa-4c77-a5c4-890ceed0ea32/image.png" /><blockquote>
</blockquote>
</li>
<li>프로토타입 스코프 테스트 결과
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/9e47d6c2-f8b2-47ed-b70d-f005f1d33243/image.png" /></li>
</ul>
<h4 id="싱글톤-빈을-함께-프로토타입-빈을-사용할-경우-생기는-문제점">싱글톤 빈을 함께 프로토타입 빈을 사용할 경우 생기는 문제점</h4>
<p>프로토타입 스코프의 빈은 요청할 때마다 항상 다른 객체 인스턴스를 생성해서 반환해 줍니다.
그러나 싱글톤 빈과 함께 사용할 때는 의도한 대로 잘 동작하지 않을 수 있어 주의해야 합니다.</p>
<blockquote>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/12b955ac-f9ab-4688-b6d6-620c19740b42/image.png" /></p>
<blockquote>
</blockquote>
<p>위의 그림을 보면 클라이언트 A와 B가 각각 addCount()를 통해 프로토타입 빈의 count를 늘리려고 시도하는데, 프로토타입 빈의 특성상 A와 B에게 각각 생성되어 count 값이 1이 되는것을 기대하지만, 1이 아닌 2를 값으로 가지게 됩니다.</p>
<blockquote>
</blockquote>
<p>스프링은 일반적으로 싱글톤 빈을 사용하기 때문에 싱글톤 빈이 프로토타입 빈을 사용하게 됩니다. 여기서 문제가 발생하는데, 싱글톤은 생성과 함께 의존관계 주입을 받기 때문에, 그 순간 프로토타입의 빈이 새로 생성되기는 하지만, 싱글톤 빈과 함께 계속유지되는 것이 문제입니다.</p>
<blockquote>
</blockquote>
<p>💡 같은 프로토타입 빈을 여러개의 빈이 주입받는다면, 각 빈마다 다른 프로토타입 빈이 생성되어 주입됩니다. 그러나 이 또한 사용할 때 새로 생성되지 않고 동일한 프로토 타입 빈을 사용하게 됩니다.</p>
<blockquote>
</blockquote>
<p>그렇다면 프로토타입 빈을 사용할 때 마다 새로 생성해서 사용하려면 어떻게 해야할까요??</p>
<blockquote>
</blockquote>
<h4 id="프로토타입-빈을-사용할-때-마다-새로-생성해서-사용하는-방법">프로토타입 빈을 사용할 때 마다 새로 생성해서 사용하는 방법</h4>
<p><strong>1. ObjectProvider 사용</strong>
스프링이 제공하는 ObjectProvider를 사용하면, 매번 새로운 프로토타입 빈을 생성할 수 있습니다.</p>
<pre><code class="language-java">import org.springframework.beans.factory.ObjectProvider;
import org.springframework.stereotype.Service;

@Service
public class TaskServiceCaller {

    private final ObjectProvider&lt;TaskService&gt; taskServiceProvider;

    public TaskServiceCaller(ObjectProvider&lt;TaskService&gt; taskServiceProvider) {
        this.taskServiceProvider = taskServiceProvider;
    }

    public void callService() {
        TaskService taskService1 = taskServiceProvider.getObject();
        taskService1.processTask();

        TaskService taskService2 = taskServiceProvider.getObject();
        taskService2.processTask();
    }
}</code></pre>
<p>실행 결과</p>
<pre><code>TaskService instance created
Processing task
TaskService instance created
Processing task</code></pre><p><strong>2. ApplicationContext 직접 사용</strong>
ApplicationContext를 사용하여 프로토타입 빈을 직접 가져옵니다.</p>
<pre><code class="language-java">import org.springframework.context.ApplicationContext;
import org.springframework.stereotype.Service;

@Service
public class TaskServiceCaller {

    private final ApplicationContext context;

    public TaskServiceCaller(ApplicationContext context) {
        this.context = context;
    }

    public void callService() {
        TaskService taskService1 = context.getBean(TaskService.class);
        taskService1.processTask();

        TaskService taskService2 = context.getBean(TaskService.class);
        taskService2.processTask();
    }
}</code></pre>
<p>실행 결과</p>
<pre><code>TaskService instance created
Processing task
TaskService instance created
Processing task</code></pre><p><strong>3. @Scope(proxyMode = ScopedProxyMode.TARGET_CLASS)</strong>
프록시 객체를 사용해 호출 시마다 프로토타입 빈을 생성할 수도 있습니다.</p>
<pre><code class="language-java">@Service
@Scope(value = &quot;prototype&quot;, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class TaskService {
    public TaskService() {
        System.out.println(&quot;TaskService instance created&quot;);
    }

    public void processTask() {
        System.out.println(&quot;Processing task&quot;);
    }
}</code></pre>
<pre><code class="language-java">@Component
public class AppRunner implements CommandLineRunner {

    @Autowired
    private TaskService taskService;

    @Override
    public void run(String... args) {
        taskService.processTask(); // 첫 번째 호출
        taskService.processTask(); // 두 번째 호출
    }
}
</code></pre>
<p>실행 결과</p>
<pre><code>TaskService instance created
Processing task
TaskService instance created
Processing task</code></pre><h3 id="3-웹-스코프">3) 웹 스코프</h3>
<ul>
<li><p>request</p>
<ul>
<li>HTTP 요청마다 새로운 Bean 이 생성된다.</li>
<li>웹 요청이 들어오면 Bean 생성되고 요청이 완료되면 소멸된다.</li>
<li><strong>Spring MVC로 만든 Web Application에서 사용하는 방식</strong></li>
<li>Web Application에서 요청별로 별도의 Bean이 필요한 경우</li>
<li>요청 데이터를 처리하는 객체<blockquote>
<p>생성되는 객체들</p>
</blockquote>
<ul>
<li>HTTP 요청 정보 저장 객체</li>
<li>요청별 로깅 컨텍스트</li>
<li>요청 추적 객체</li>
<li>사용자 입력 데이터 캐싱 객체</li>
</ul>
</li>
</ul>
</li>
<li><p>session</p>
<ul>
<li>HTTP 세션 동안 하나의 Bean 인스턴스를 유지한다.</li>
<li>웹 세션이 시작되면 생성되고 종료될 때 소멸한다.<blockquote>
<p>생성되는 객체들</p>
</blockquote>
<ul>
<li>사용자 세션 정보</li>
<li>장바구니 객체</li>
<li>사용자 인증 정보</li>
<li>사용자별 설정 정보</li>
<li>임시 데이터 저장소</li>
</ul>
</li>
</ul>
</li>
</ul>
<ul>
<li>application<ul>
<li>서블릿 컨텍스트 내에서 Bean이 단일 인스턴스로 존재한다.</li>
<li>애플리케이션이 구동되는 동안 동일한 객체가 유지된다.<blockquote>
<p>생성되는 객체들</p>
</blockquote>
<ul>
<li>애플리케이션 전역 설정</li>
<li>캐시 매니저</li>
<li>전역 카운터</li>
<li>공유 리소스 관리자</li>
<li>애플리케이션 이벤트 리스너</li>
</ul>
</li>
</ul>
</li>
</ul>
<h3 id="스코프-설정-예제">스코프 설정 예제</h3>
<pre><code class="language-java">// 자동 등록
@Scope(&quot;singleton&quot;) // 생략 가능(기본 값)
@Component // @Service 사용 가능
public class MemberServiceImpl implements MemberService { ... }

// 수동 등록
@Configuration
public class AppConfig {

    @Scope(&quot;singleton&quot;) // 생략 가능
    @Bean
    public MemberService memberService() {
            return new MemberServiceImpl();
    }

}</code></pre>
<h3 id="스코플-별-생명주기-관리">스코플 별 생명주기 관리</h3>
<ol>
<li><p><strong>Singleton</strong></p>
<p> 생성: 컨테이너 시작 시
 소멸: 컨테이너 종료 시</p>
</li>
</ol>
<ol start="2">
<li><p><strong>Prototype</strong></p>
<p> 생성: getBean() 호출 시
 소멸: GC에 의해 관리</p>
</li>
</ol>
<ol start="3">
<li><p><strong>Request</strong></p>
<p> 생성: HTTP 요청 시작 시
 소멸: HTTP 요청 종료 시</p>
</li>
</ol>
<ol start="4">
<li><p><strong>Session</strong></p>
<p> 생성: 사용자 세션 시작 시
 소멸: 세션 종료 또는 만료 시</p>
</li>
</ol>
<ol start="5">
<li><strong>Application Scope</strong>
 생성: 애플리케이션 시작 시
 소멸: 애플리케이션 종료 시</li>
</ol>
<ol start="6">
<li><strong>WebSocket Scope</strong>
 생성: 애플리케이션 시작 시
 소멸: 웹소켓 연결 종료 시</li>
</ol>
<ul>
<li>스코프 코드</li>
</ul>
<pre><code class="language-java">// 1. Singleton Scope (Default)
@Component
public class SingletonService {
    private int counter = 0;

    public void increment() {
        counter++;
    }

    public int getCount() {
        return counter;
    }
}

// 2. Prototype Scope
@Component
@Scope(&quot;prototype&quot;)
public class PrototypeService {
    private int value;

    public PrototypeService() {
        this.value = new Random().nextInt(100);
    }
}

// 3. Request Scope
@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class RequestScopedBean {
    private final String requestId = UUID.randomUUID().toString();

    public String getRequestId() {
        return requestId;
    }
}

// 4. Session Scope
@Component
@Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class UserSessionBean {
    private String username;
    private List&lt;String&gt; permissions = new ArrayList&lt;&gt;();

    public void login(String username) {
        this.username = username;
    }
}

// 5. Application Scope
@Component
@Scope(WebApplicationContext.SCOPE_APPLICATION)
public class ApplicationScopedBean {
    private final Map&lt;String, String&gt; globalConfig = new ConcurrentHashMap&lt;&gt;();

    public void setConfig(String key, String value) {
        globalConfig.put(key, value);
    }
}

// 6. Custom Scope Example
@Component
@Scope(value = &quot;tenant&quot;, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class TenantScopedBean {
    private String tenantId;
    private DataSource tenantSpecificDataSource;

    public void setTenantId(String tenantId) {
        this.tenantId = tenantId;
        // Initialize tenant-specific resources
    }
}

// 7. Scope Configuration
@Configuration
public class ScopeConfig {
    @Bean
    public CustomScopeConfigurer customScopes() {
        CustomScopeConfigurer configurer = new CustomScopeConfigurer();
        Map&lt;String, Object&gt; scopes = new HashMap&lt;&gt;();
        scopes.put(&quot;tenant&quot;, new TenantScope());
        configurer.setScopes(scopes);
        return configurer;
    }
}
</code></pre>
<h1 id="4-빈의-라이프사이클">4. 빈의 라이프사이클</h1>
<p>스프링은 빈의 생명주기를 다음 순서로 관리합니다:</p>
<ol>
<li>빈 생성 = 생성자 호출</li>
<li>의존성 주입</li>
<li>초기화 콜백 호출</li>
<li>빈 사용</li>
<li>소멸 콜백 호출</li>
</ol>
<h3 id="라이프사이클-예제">라이프사이클 예제</h3>
<pre><code class="language-java">@Component
public class LifecycleBean {

    @PostConstruct
    public void init() {
        System.out.println(&quot;Bean is initialized&quot;);
    }

    @PreDestroy
    public void destroy() {
        System.out.println(&quot;Bean is about to be destroyed&quot;);
    }
}</code></pre>
<h1 id="5-콜백-메서드">5. 콜백 메서드</h1>
<h2 id="1-콜백-종류">1) 콜백 종류</h2>
<h3 id="1-초기화-콜백">1. 초기화 콜백</h3>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/72cfb4ab-5a0a-41a2-84f6-d2fee4db07a6/image.png" /></p>
<ul>
<li><code>@PostConstruct</code> 또는 <code>InitializingBean</code>의 <code>afterPropertiesSet()</code> 메서드 호출</li>
<li>주로 데이터베이스 연결, 리소스 준비, 설정 작업과 같은 준비를 수행한다.</li>
</ul>
<h3 id="2-소멸전-콜백">2. 소멸전 콜백</h3>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/82f07e7c-fa62-4e7f-949f-3e42dfa54699/image.png" /></p>
<ul>
<li><code>@PreDestroy</code> 또는 <code>DisposableBean</code>의 <code>destroy()</code> 메서드 호출</li>
<li>Application이 종료되거나 컨테이너가 종료되기 직전에 Spring은 Bean의 소멸 메서드를 호출하여 리소스를 정리한다.<ul>
<li>주로 파일 닫기, 데이터베이스 연결 해제 등 리소스 정리를 위해 사용된다.</li>
</ul>
</li>
</ul>
<h2 id="콜백-설정-방법">콜백 설정 방법</h2>
<h3 id="콜백-메서드-설정-방법-1---initializingbean-disposablebean">콜백 메서드 설정 방법 1 - InitializingBean, DisposableBean</h3>
<blockquote>
<p>Bean이 생성되고 모든 의존성이 주입된 후 InitializingBean의 afterPropertiesSet() 메서드가 호출되고 초기화 작업을 수행할 수 있다. 컨테이너가 종료될 때는 DisposableBean의 destroy() 메서드가 호출되며, 리소스 해제나 정리 작업을 처리할 수 있다.
인터페이스를 사용하여 콜백 메서드를 사용하는 방법은 최근에는 잘 사용하지 않는 방법이지만 전체적인 동작 흐름을 코드로 이해하기 위해 만든 예시입니다.</p>
</blockquote>
<ul>
<li><p>인터페이스 설정</p>
<pre><code class="language-java">public class MyBean implements InitializingBean, DisposableBean {

  private String data;

  public MyBean() {
      System.out.println(&quot;Bean 생성자 호출&quot;);
      System.out.println(&quot;data = &quot; + data);
  }

  public void setData(String data) {
      this.data = data;
  }

  public String getData() {
      return data;
  }

  // InitializingBean 인터페이스의 초기화 메서드
  @Override
  public void afterPropertiesSet() throws Exception {
      System.out.println(&quot;MyBean 초기화 - afterPropertiesSet() 호출됨&quot;);
      System.out.println(&quot;data = &quot; + data);
  }

  // DisposableBean 인터페이스의 종료 메서드
  @Override
  public void destroy() throws Exception {
      System.out.println(&quot;MyBean 종료 - destroy() 호출됨&quot;);
      data = null;
  }

  public void doSomething() {
      System.out.println(&quot;MyBean 작업 중...&quot;);
  }
</code></pre>
</li>
</ul>
<p>}</p>
<pre><code>```java
@Configuration
public class AppConfig {

    @Bean
    public MyBean myBean() {
        MyBean myBean = new MyBean();
        // 의존관계 설정
        myBean.setData(&quot;Example&quot;);
        return myBean;
    }

}</code></pre><pre><code class="language-java">public class InterfaceCallback {

    public static void main(String[] args) {
        // Spring 컨테이너 생성 및 설정 클래스 등록
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        // Bean 사용
        MyBean myBean = context.getBean(MyBean.class);
        myBean.doSomething();

                // 종료 되기전 호출
        System.out.println(myBean.getData());
        // 컨테이너 종료 (DisposableBean 호출됨)
        context.close();
        System.out.println(myBean.getData());
    }

}</code></pre>
<ul>
<li><p>출력 결과</p>
<pre><code>// 1. Bean 생성자 호출
Bean 생성자 호출
data = null
// 2. 의존관계 설정 setData()
// 3. 초기화
MyBean 초기화 - afterPropertiesSet() 호출됨
// 4. Bean 사용
data = Example
MyBean 작업 중...
// 5. Spring 종료 전 Data 출력
Example
// 6. destroy() -&gt; data = null
MyBean 종료 - destroy() 호출됨
// 7. close 이후 Data 출력
null</code></pre></li>
<li><p><strong>인터페이스 사용의 단점</strong></p>
<ol>
<li>Spring에 의존적이다.<ul>
<li>외부 라이브러리 등의 코드에 적용할 수 없다.</li>
</ul>
</li>
<li>Method의 이름을 바꿀 수 없다.(오버라이딩)</li>
</ol>
</li>
</ul>
<h3 id="콜백-메서드-설정-방법-2---bean-속성">콜백 메서드 설정 방법 2 - @Bean 속성</h3>
<blockquote>
</blockquote>
<p>Spring에서 생명주기 콜백을 지원하는 방법으로 설정 정보에 초기화 메서드와 종료 메서드를 지정하는 방법을 사용할 수 있다. 이 방법을 선택하면 외부 라이브러리에도 콜백 메서드를 사용할 수 있다.</p>
<ul>
<li><strong>설정 정보에 초기화 메서드, 종료 메서드 지정</strong><ul>
<li><code>*@Bean*(initMethod = &quot;초기화 메서드명&quot;, destroyMethod = &quot;소멸 메서드명&quot;)</code></li>
</ul>
</li>
</ul>
<pre><code class="language-java">public class MyBeanV2 {

    private String data;

    public MyBeanV2() {
        System.out.println(&quot;Bean 생성자 호출&quot;);
        System.out.println(&quot;data = &quot; + data);
    }

    public void setData(String data) {
        this.data = data;
    }

    public String getData() {
        return data;
    }

    public void init() {
        System.out.println(&quot;MyBean 초기화 - init() 호출됨&quot;);
        System.out.println(&quot;data = &quot; + data);
    }

    public void close() {
        System.out.println(&quot;MyBean 종료 - close() 호출됨&quot;);
        data = null;
    }

    public void doSomething() {
        System.out.println(&quot;MyBean 작업 중...&quot;);
    }

}</code></pre>
<pre><code class="language-java">@Configuration
public class AppConfigV2 {

    @Bean(initMethod = &quot;init&quot;, destroyMethod = &quot;close&quot;)
    public MyBeanV2 myBeanV2() {
        MyBeanV2 myBeanV2 = new MyBeanV2();
        // 의존관계 설정
        myBeanV2.setData(&quot;Example&quot;);
        return myBeanV2;
    }

}</code></pre>
<pre><code class="language-java">public class InterfaceCallbackV2 {

    public static void main(String[] args) {
        // Spring 컨테이너 생성 및 설정 클래스 등록
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfigV2.class);

        // Bean 사용
        MyBeanV2 myBeanV2 = context.getBean(MyBeanV2.class);
        myBeanV2.doSomething();

        // Bean 소멸 전 필드
        System.out.println(myBeanV2.getData());

        // 컨테이너 종료 (destroy 호출)
        context.close();

        // Bean 소멸 후 필드
        System.out.println(myBeanV2.getData());
    }

}</code></pre>
<ul>
<li>실행결과
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/1d1e465f-9e9d-48b1-a705-24fd2b0881fb/image.png" /></li>
</ul>
<ul>
<li>설정정보 사용<ol>
<li>Bean이 Spring 내부적으로 구현된 코드에 의존하지 않는다.</li>
<li>메서드 이름을 자유롭게 설정할 수 있다.</li>
<li>외부 라이브러리에도 초기화, 종료 메서드를 적용할 수 있다.</li>
</ol>
</li>
</ul>
<blockquote>
</blockquote>
<p>@Bean의 destroyMethod 속성은 종료 메서드 추론 기능으로 대부분의 라이브러리 종료 메서드 이름인 close, shutdown이 기본값으로 등록되어 이름이 둘 중 하나라면 명시하지 않아도 동작하도록 구성되어 있다.</p>
<h3 id="콜백-메서드-설정-방법-3---postconstruct-predestroy">콜백 메서드 설정 방법 3 - @PostConstruct, @PreDestroy</h3>
<blockquote>
<p>Bean의 생명주기 동안 특정 작업을 실행하기 위해 사용하는 Annotation으로 각각 Spring Bean의 초기화와 소멸 시점에 호출되는 메서드를 지정할 때 사용된다.</p>
</blockquote>
<ul>
<li><p><strong>Java 표준 Annotation</strong></p>
<ul>
<li><p>Spring이 아니여도 Annotation이 동작한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/239bf59f-474c-47a2-9748-ab0a58f23c9f/image.png" /></p>
</li>
</ul>
</li>
</ul>
<pre><code>- 최신 Spring에서 권장하는 방법이다.
1. **`@PostConstruct`**
    - Bean이 생성되고 의존성 주입이 완료된 후에 호출되는 메서드를 지정한다.
    - 주로 **초기화 작업**을 할 때 사용된다.
2. **`@PreDestroy`**
    - Bean이 소멸되기 직전에 호출되는 메서드를 지정한다.
    - 주로 **리소스 정리** 작업을 할 때 사용된다.</code></pre><pre><code class="language-java">public class MyBeanV3 {

    private String data;

    public MyBeanV3() {
        System.out.println(&quot;Bean 생성자 호출&quot;);
        System.out.println(&quot;data = &quot; + data);
    }

    public void setData(String data) {
        this.data = data;
    }

    public String getData() {
        return data;
    }

    // 초기화 메서드
    @PostConstruct
    public void init() {
        System.out.println(&quot;MyBean 초기화 - init() 호출됨&quot;);
        System.out.println(&quot;data = &quot; + data);
    }

    // 소멸 메서드
    @PreDestroy
    public void destroy() throws Exception {
        System.out.println(&quot;MyBean 종료 - destroy() 호출됨&quot;);
        data = null;
    }

    public void doSomething() {
        System.out.println(&quot;MyBean 작업 중...&quot;);
    }

}</code></pre>
<pre><code class="language-java">@Configuration
public class AppConfigV3 {

    @Bean
    public MyBeanV3 myBeanV3() {
        MyBeanV3 myBeanV3 = new MyBeanV3();
        // 의존관계 설정
        myBeanV3.setData(&quot;Example&quot;);
        return myBeanV3;
    }

}</code></pre>
<pre><code class="language-java">public class InterfaceCallbackV3 {

    public static void main(String[] args) {
        // Spring 컨테이너 생성 및 설정 클래스 등록
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfigV3.class);

        // Bean 사용
        MyBeanV3 myBeanV3 = context.getBean(MyBeanV3.class);
        myBeanV3.doSomething();

        // Bean 소멸 전 필드
        System.out.println(myBeanV3.getData());

        // 컨테이너 종료 (destroy 호출)
        context.close();

        // Bean 소멸 후 필드
        System.out.println(myBeanV3.getData());
    }

}</code></pre>
<ul>
<li>실행결과</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/b2676d2e-c417-46fd-9551-6fa69d6d3bbc/image.png" /></p>
<ul>
<li><strong>표준 Annotation 방식의 단점</strong><ul>
<li>외부 라이브러리에 적용이 불가능하다.</li>
</ul>
</li>
</ul>
<blockquote>
</blockquote>
<p>외부 라이브러리에 초기화, 종료 메서드를 사용해야 한다면 @Bean의 <code>initMethod</code>, <code>destroyMethod</code> 를 사용하면 된다.</p>