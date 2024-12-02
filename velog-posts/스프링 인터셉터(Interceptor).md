<h2 id="스프링-인터셉터란">스프링 인터셉터란?</h2>
<p>스프링 프레임워크의 <strong>인터셉터(Interceptor)</strong>는 HTTP 요청과 응답 흐름을 가로채서 전처리(pre-processing) 및 후처리(post-processing)를 수행할 수 있는 기능을 제공합니다. 이는 스프링 MVC에서 제공하는 핵심 컴포넌트 중 하나로, 주로 인증, 로깅, 권한 확인, 데이터 변환 등의 작업을 수행하는 데 사용됩니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/0312a4f6-4076-4bd5-b148-22ade5e52c9e/image.png" /></p>
<blockquote>
<p>웹 컨테이너에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작한다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/a95d2636-6183-4b2e-8320-28a6d5433821/image.png" /></p>
<h2 id="스프링-인터셉터-인터페이스">스프링 인터셉터 인터페이스</h2>
<p>스프링의 인터셉터를 사용하려면 HandlerInterceptor 인터페이스를 구현해서 사용하면 됩니다.</p>
<pre><code class="language-java">public interface HandlerInterceptor {

   default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
         throws Exception {

      return true;
   }

   default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
         @Nullable ModelAndView modelAndView) throws Exception {
   }


   default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
         @Nullable Exception ex) throws Exception {
   }

}</code></pre>
<h2 id="스프링-인터셉터-요청-흐름">스프링 인터셉터 요청 흐름</h2>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/021e4566-dad3-4d7e-ac2b-3400ae7bfeda/image.png" /></p>
<ul>
<li><p>preHandle()</p>
<ul>
<li>컨트롤러(핸들러) 호출 전에 호출됩니다. (엄밀히 말하면 핸들러 어댑터 호출 전)</li>
<li>preHandle()의 리턴값이 true이면 다음으로 진행, false면 더 이상 진행하지 않습니다.(false면 controller 호출 x -&gt; 종료)</li>
</ul>
</li>
<li><p>postHandle()</p>
<ul>
<li>컨트롤러(핸들러) 호출 후에 호출됩니다. ( 엄밀히 말하면 핸들러 어댑터 호출 후)</li>
<li>컨트롤러에서 예외 발생 시에는 실행되지 않고 afterCompletion으로 넘어갑니다.</li>
<li>파라미터로 ModelAndView가 넘어오기 때문에 모델이나 뷰에 추가작업을 할 수 있습니다.</li>
<li>주로 응답 데이터 가공이나 추가 작업에 사용됩니다.</li>
</ul>
</li>
<li><p>afterCompletion()</p>
<ul>
<li>요청의 전체 라이프사이클이 끝난 후 실행됩니다.(=뷰가 렌더링 된 이후 호출됩니다.)</li>
<li>항상 호출되기 때문에 Exception을 파라미터로 받아 컨트롤러에서 어떤 Exception이 발생했는지 로그로 출력할 수도 있고, Exception을 처리하는데 사용됩니다.</li>
<li>주로 리소스 정리나 예외 로깅 등에 사용됩니다.</li>
</ul>
</li>
</ul>
<h3 id="스프링-인터셉터-적용하는-방법">스프링 인터셉터 적용하는 방법</h3>
<ul>
<li>WebMvcConfigurer 인터페이스를 구현한 클래스 내부에서 addInterceptors(InterceptorRegistry registry)를 Override합니다.</li>
<li>이 때 registry의 메소드를 통해 인터셉트를 추가, 적용순서, 적용 URL 패턴, 예외 URL 패턴을 지정할 수 있습니다.</li>
</ul>
<pre><code class="language-java">registry.addInteceptor(추가할 인터셉터)
        .order(적용 순서)
        .addPatterns(&quot;적용할 URL 패턴&quot;)
        .excludePathPatterns(&quot;적용하지 않을 예외 URL 패턴&quot;)
</code></pre>
<h2 id="구현-예제">구현 예제</h2>
<h3 id="1-인터셉터-클래스-구현">1. 인터셉터 클래스 구현</h3>
<pre><code class="language-java">import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class MyInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println(&quot;PreHandle: 요청 처리 전 실행&quot;);
        // 예: 인증 확인
        return true; // true일 경우 요청을 계속 처리
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println(&quot;PostHandle: 컨트롤러 처리 후 실행&quot;);
        // 예: 응답 데이터 가공
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println(&quot;AfterCompletion: 요청 완료 후 실행&quot;);
        // 예: 리소스 정리
    }
}
</code></pre>
<h3 id="2-인터셉터-등록-spring-configuration-클래스">2. 인터셉터 등록 (Spring Configuration 클래스)</h3>
<pre><code class="language-java">import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private MyInterceptor myInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor)
                .order(1)
                .addPathPatterns(&quot;/api/**&quot;) // 특정 경로에만 적용
                .excludePathPatterns(&quot;/api/auth/**&quot;); // 제외 경로 설정
    }
}
</code></pre>
<h2 id="필터filter-vs-인터셉터interceptor-차이-정리-및-요약">필터(Filter) vs 인터셉터(Interceptor) 차이 정리 및 요약</h2>
<p>[ 필터(Filter) vs 인터셉터(Interceptor) 차이 정리 및 요약 ]
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/0fd7fb1b-63eb-4ffa-a788-8bd1db04f878/image.png" /></p>
<p> </p>
<h3 id="관리되는-컨테이너">관리되는 컨테이너</h3>
<p>앞서 살펴본 그림에서 보이듯이 필터와 인터셉터는 관리되는 영역이 다르다. 필터는 스프링 이전의 서블릿 영역에서 관리되지만, 인터셉터는 스프링 영역에서 관리되는 영역이기 때문에 필터는 스프링이 처리해주는 내용들을 적용 받을 수 없다. 이로 인한 차이로 발생하는 대표적인 예시가 스프링에 의한 예외처리가 되지 않는다는 것이다.
참고로 일부 포스팅 또는 자료에서 필터(Filter)가 스프링 빈으로 등록되지 못하며, 빈을 주입 받을 수도 없다고 하는데, 이는 잘못된 설명이다. 이는 매우 옛날의 이야기이며, 필터는 현재 스프링 빈으로 등록이 가능하며, 다른 곳에 주입되거나 다른 빈을 주입받을 수도 있다. 이와 관련해서는 다음 포스팅에서 자세히 살펴보도록 하자.
 
 
 </p>
<h3 id="스프링의-예외-처리-여부">스프링의 예외 처리 여부</h3>
<p>일반적으로 스프링을 사용한다면 ControllerAdvice와 ExceptionHandler를 이용한 예외처리 기능을 주로 사용한다. 예를 들어 원하는 멤버를 찾지 못하여 로직에서 MemberNotFoundException을 던졌다면 404 Status로 응답을 반환하길 원할 것이다. 그리고 이를 위해 우리는 다음과 같은 예외 처리기를 구현하여 활용할 것이다. 이를 통해 예외가 서블릿까지 전달되지 않고 처리된다.</p>
<pre><code class="language-java">@RestControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(MemberNotFoundException.class)
    public ResponseEntity&lt;Object&gt; handleMyException(MemberNotFoundException e) {
        return ResponseEntity.notFound()
            .build();
    }

    ...
}</code></pre>
<p> 
 
하지만 앞서 설명하였듯 필터는 스프링 앞의 서블릿 영역에서 관리되기 때문에 스프링의 지원을 받을 수 없다. 그래서 만약 필터에서  MemberNotFoundException이 던져졌다면, 에러가 처리되지 않고 서블릿까지 전달된다. 서블릿은 예외가 핸들링 되기를 기대했지만, 예외가 그대로 올라와서 예상치 못한 Exception을 만난 상황이다. 따라서 내부에 문제가 있다고 판단하여 500 Status로 응답을 반환한다. 이를 해결하려면 필터에서 다음과 같이 응답(Response) 객체에 예외 처리가 필요하다.</p>
<p>자세한 예외 처리 관련 내용은 해당 포스팅을 참고하도록 하자.</p>
<pre><code class="language-java">public class MyFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletResponse servletResponse = (HttpServletResponse) response;
        servletResponse.setStatus(HttpServletResponse.SC_NOT_FOUND);
        servletResponse.getWriter().print(&quot;Member Not Found&quot;);
    }
}
 </code></pre>
<p> 
 
 </p>
<h3 id="requestresponse-객체-조작-가능-여부">Request/Response 객체 조작 가능 여부</h3>
<p>필터는 Request와 Response를 조작할 수 있지만 인터셉터는 조작할 수 없다. 여기서 조작한다는 것은 내부 상태를 변경한다는 것이 아니라 다른 객체로 바꿔친다는 의미이다. 이는 필터와 인터셉터의 코드를 보면 바로 알 수 있다.
필터가 다음 필터를 호출하기 위해서는 필터 체이닝(다음 필터 호출)을 해주어야 한다. 그리고 이때 Request/Response 객체를 넘겨주므로 우리가 원하는 Request/Response 객체를 넣어줄 수 있다. NPE가 나겠지만 null로도 넣어줄 수 있는 것이다.</p>
<pre><code class="language-java">public MyFilter implements Filter {

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
        // 개발자가 다른 request와 response를 넣어줄 수 있음
        chain.doFilter(new MockHttpServletRequest(), new MockHttpServletResponse());       
    }

}</code></pre>
<p> 
 
하지만 인터셉터는 처리 과정이 필터와 다르다. 디스패처 서블릿이 여러 인터셉터 목록을 가지고 있고, for문으로 순차적으로 실행시킨다. 그리고 true를 반환하면 다음 인터셉터가 실행되거나 컨트롤러로 요청이 전달되며, false가 반환되면 요청이 중단된다. 그러므로 우리가 다른 Request/Response 객체를 넘겨줄 수 없다. 그리고 이러한 부분이 필터와 확실히 다른 점이다.</p>
<pre><code class="language-java">public class MyInterceptor implements HandlerInterceptor {

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        // Request/Response를 교체할 수 없고 boolean 값만 반환할 수 있다.
        return true;
    }

}</code></pre>
<p> 
 
 </p>
<h3 id="-필터filter와-인터셉터interceptor의-용도-및-예시-">[ 필터(Filter)와 인터셉터(Interceptor)의 용도 및 예시 ]</h3>
<h4 id="필터filter의-용도-및-예시">필터(Filter)의 용도 및 예시</h4>
<blockquote>
</blockquote>
<ul>
<li>공통된 보안 및 인증/인가 관련 작업</li>
<li>모든 요청에 대한 로깅 또는 감사</li>
<li>이미지/데이터 압축 및 문자열 인코딩</li>
<li>Spring과 분리되어야 하는 기능</li>
</ul>
<p>필터에서는 기본적으로 스프링과 무관하게 전역적으로 처리해야 하는 작업들을 처리할 수 있다.
대표적으로 보안 공통 작업이 있다. 필터는 인터셉터보다 앞단에서 동작하므로 전역적으로 해야하는 보안 검사(XSS 방어 등)를 하여 올바른 요청이 아닐 경우 차단을 할 수 있다. 그러면 스프링 컨테이너까지 요청이 전달되지 못하고 차단되므로 안정성을 더욱 높일 수 있다.
또한 필터는 이미지나 데이터의 압축이나 문자열 인코딩과 같이 웹 애플리케이션에 전반적으로 사용되는 기능을 구현하기에 적당하다. Filter는 다음 체인으로 넘기는 ServletRequest/ServletResponse 객체를 조작할 수 있다는 점에서 Interceptor보다 훨씬 강력한 기술이다.
 
 
 </p>
<h4 id="인터셉터interceptor의-용도-및-예시">인터셉터(Interceptor)의 용도 및 예시</h4>
<blockquote>
</blockquote>
<ul>
<li>세부적인 보안 및 인증/인가 공통 작업</li>
<li>API 호출에 대한 로깅 또는 감사</li>
<li>Controller로 넘겨주는 정보(데이터)의 가공</li>
</ul>
<p>인터셉터에서는 클라이언트의 요청과 관련되어 전역적으로 처리해야 하는 작업들을 처리할 수 있다.
대표적으로 세부적으로 적용해야 하는 인증이나 인가와 같이 클라이언트 요청과 관련된 작업 등이 있다. 예를 들어 특정 그룹의 사용자는 어떤 기능을 사용하지 못하는 경우가 있는데, 이러한 작업들은 컨트롤러로 넘어가기 전에 검사해야 하므로 인터셉터가 처리하기에 적합하다.
또한 인터셉터는 필터와 다르게 HttpServletRequest나 HttpServletResponse 등과 같은 객체를 제공받으므로 객체 자체를 조작할 수는 없다. 대신 해당 객체가 내부적으로 갖는 값은 조작할 수 있으므로 컨트롤러로 넘겨주기 위한 정보를 가공하기에 용이하다. 예를 들어 사용자의 ID를 기반으로 조회한 사용자 정보를 HttpServletRequest에 넣어줄 수 있다.
그 외에도 우리는 다양한 목적으로 API 호출에 대한 정보들을 기록해야 할 수 있다. 이러한 경우에 HttpServletRequest나 HttpServletResponse를 제공해주는 인터셉터는 클라이언트의 IP나 요청 정보들을 포함해 기록하기에 용이하다.
 
 
 
 
대표적으로 필터(Filter)를 인증과 인가에 사용하는 도구로는 SpringSecurity가 있다. SpringSecurity의 특징 중 하나는 Spring MVC에 종속적이지 않다는 것인데, 이러한 이유로는 필터 기반으로 인증/인가 처리를 하기 때문이다.
위의 내용 중에서 필터는 스프링 빈으로 등록이 가능하며, 다른 곳에 주입되거나 다른 빈을 주입받을 수도 있다고 하였다. 사실 서블릿 컨테이너에 관리되는 필터가 스프링 빈에 주입되는 등의 작업이 가능하다는 것이 이상하기도 하며, 몇몇 블로그들은 이와 관련해서 잘못 설명하고 있기도 하다. 그래서 다음 포스팅에서는 왜 필터(Filter)가 스프링 빈으로 등록이 가능한지 자세히 살펴보도록 하자.</p>
<p>출처: <a href="https://mangkyu.tistory.com/173">https://mangkyu.tistory.com/173</a> [MangKyu's Diary:티스토리]</p>
<hr />
<hr />
<h3 id="a1-aop란">A1. AOP란?</h3>
<p>AOP는 결제 기능, 게시판 기능, 유저 기능 등 여러 기능 처리 앞, 뒤에 공통적으로 들어가는 코드(로깅이나 소요시간 측정 등)을 따로 빼서 관리하는걸 뜻합니다.</p>
<p>예를들어,</p>
<ol>
<li><p>결제+로깅,</p>
</li>
<li><p>게시판+로깅,</p>
</li>
<li><p>유저+로깅</p>
</li>
</ol>
<p>이렇게 코드가 섞여있던걸 있던걸, AOP로 처리하면,</p>
<p>===</p>
<p>로깅() 메서드를 따로 만들어 놓고, 어노테이션으로 등록한 후에,</p>
<p>결제(), 게시판(), 유저() 메서드 위에 '@로깅' 한줄만 붙이는 겁니다.</p>
<p>그럼 기존 코드에서 관심사가 분리되었으니 유지보수가 쉽겠죠?</p>
<h3 id="a2-필터-인터셉터와-aop의-차이">A2. 필터, 인터셉터와 AOP의 차이</h3>
<p>AOP는 공통로직 관심사의 분리라고 했잖아요? 로깅이나 성능 측정 같은?</p>
<p>AOP라는 컨셉에 한 종류가 필터이고, 인터셉터 입니다.</p>
<p>필터랑 인터셉터의 차이는 spring context 밖에서 놀면 필터고 안에서 놀면 인터셉터라고 이해하시면 쉽습니다.</p>
<h3 id="a3-spring-context-안과-밖">A3. spring context 안과 밖?</h3>
<p>client-&gt;server로 http request를 보내죠?</p>
<p>client-&gt;(web server) -&gt; web application server(WAS, tomcat) -&gt; spring</p>
<p>중간에 웹서버(nginx나 아파치나..) 찍고, WAS(톰캣이나) 찍고 스프링으로 보내지겠죠?</p>
<p>근데 저 톰캣에 WAS단계, spring 찍기 전에 servlet단에서 지지고 볶는 단계가 아래에 보시면
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/f6c31ae4-1c0c-4337-a3cd-597e1b7f6c47/image.png" /></p>
<p>web context 단계입니다.</p>
<p>저 web context안에 필터 보이시죠? 쟤가 필터구요</p>
<p>필터 다 거치고 http request가 Dispatcher Servlet에게 보내져서</p>
<p>url에 매핑된 controller에게 보내고, database연결해서 모델에 데이터 담아 뷰에 녹여 던지는 작업이 일어나잖아요?</p>
<p>이게 저 빨간색 부분인 spring context에서 일어납니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/01b12825-1d9f-44ec-9bbb-bb6f55c8f95f/image.png" /></p>
<p>spring context 부분에서 작동하는게 interceptor 구요.</p>
<p>근데 Filter랑 Interceptor 둘다 AOP인데 왜 굳이 둘로 나눠놨을까요?</p>
<h3 id="a4-filter">A4. Filter</h3>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/5214c4e9-a441-4654-8636-113890002c46/image.png" /></p>
<p>필터는 spring context 바깥이잖아요?</p>
<p>근데 spring context에서 Java객체를 Bean으로 등록해서 관리 하잖아요?</p>
<p>그래서 필터에서는 spring bean 못씁니다.</p>
<p>아니 그럼 필터를 왜쓰죠?</p>
<p>spring context에서 mvc로 지지고 볶고 하기 전에 처리해야 하는 것들 있잖아요?</p>
<p>예를들어 http request를 보낸 클라이언트가 해커는 아닌지, XSS 공격하는지, CORS 정책을 잘 따르면서 보냈는지 먼저 필터에서 거르고 통과한 애들만 spring context안에 들여보낸다던지,</p>
<p>유저가 로그인해서 인증 토큰으로 jwt 토큰을 받았는데, 이게 위조된게 아닌지 필터에서 먼저 확인한 후, 들여보냅니다.</p>
<p>utf-8 등록하는 것도 필터에서 등록할거에요.</p>
<h3 id="a5-interceptor">A5. Interceptor</h3>
<p>인터셉터는 뭐하는 놈일까요?
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/0eb1e540-15c5-44d1-8331-f0debb5b6cbd/image.png" /></p>
<p>필터 다 거르고 spring context안에 들어왔죠?</p>
<p>그럼 http request가 컨트롤러로 보내지고, 서비스로 보내지고 등등 해서 처리되겠죠?</p>
<p>이 때, 컨트롤러에서 회원가입 메서드에서 파라미터로 UserVO를 받는데, 필드값이 username, password인데,</p>
<p>client에서 POST요청 보낼 때</p>
<blockquote>
</blockquote>
<p>{
    usermame: xxx,
    pazzword: xxx
}</p>
<p>이런식으로 필드 이름을 오타내서 보냈다고 하면, 이걸 컨트롤러 단에서 파라미터로 받을 때</p>
<p>@Valid UserVO 로 확인한 후, 필드 값이 이상하면 Exception을 던지는데요,</p>
<p>여기서 @Valid나, 컨트롤러 안에서 터지는 Exception을 잡아서 Exception만 전용으로 던지는 @ControllerAdvice 이게 붙은 핸들러들을 인터셉터라고 보시면 됩니다.</p>
<p>이 외에도 서비스 단에서 비지니스 로직 처리에 걸리는 시간 앞 뒤로 해서 측정하는거나,</p>
<p>스프링에서 로깅 찍을 때,</p>
<blockquote>
</blockquote>
<p>import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;</p>
<blockquote>
</blockquote>
<p>private Logger logger = LogManager.getLogger(log.class);
logger.info(&quot;로깅!&quot;);</p>
<p>이러는 애들도 일종의 인터셉터라고 보시면 됩니다.</p>
<h3 id="a6-필터에서-빈을-못쓴다">A6. 필터에서 빈을 못쓴다?</h3>
<p>근데 사실 필터에서는 스프링 빈 못쓴다는거 뻥입니다ㅋㅋ</p>
<p>원랜 못썼는데요,</p>
<p>버전업 되면서 DelegationProxyFilter가 나오면서 빈 등록 가능합니다.</p>
<p>그래서 필터를 인터셉터처럼 쓰기가 가능은 한데요,</p>
<p>되도록이면 기존에 필터의 목적에 맞게 구현하는게 관례입니다.</p>
<p>(spring context안에서 놀다가 web context 밖에 나왔다가 다시 들어가는게 아마 성능상으로도 별로일 거라고 추측하고 있습니다..)</p>
<p>출처 : <a href="https://okky.kr/questions/1346808">https://okky.kr/questions/1346808</a></p>