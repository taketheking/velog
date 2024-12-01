<p>필터와 인터셉터는 웹과 관련된 공통 관심사 (로그인, 사용자 권한)를 처리할 때 주로 사용된다.</p>
<p>물론 공통 관심사는 AOP로도 해결할 수 있지만, 웹과 관련된 공통 관심사를 처리할 때에는 HTTP header나 URL 정보 등이 필요한데, 필터와 인터셉터의 경우 HttpServletRequest를 제공하기 때문이다.</p>
<h2 id="필터의-흐름">필터의 흐름</h2>
<blockquote>
<p>HTTP 요청 -&gt; WAS -&gt; 필터1 -&gt; 필터2 -&gt; 필터3 -&gt; Dispatcher Servlet -&gt; 컨트롤러</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/0ddcf637-98ae-4da7-b399-8766a4496ee9/image.png" /></p>
<ul>
<li>기본적으로 필터는 dispatcherServlet 이전에 호출된다.</li>
<li>필터는 체인(chain)으로 구성되어 여러 개의 필터를 자유롭게 추가할 수 있다.</li>
</ul>
<p>이때, 필터에서 적절하지 않은 요청이라고 판단하면 dispatcherServlet을 호출하지 않고 요청을 끝낼 수도 있다.</p>
<blockquote>
<p>ex) 로그인 정보가 일치하지 않을 경우</p>
</blockquote>
<h2 id="filter-인터페이스">Filter 인터페이스</h2>
<ul>
<li><p><code>jakarta.servlet.Filter</code></p>
<p>  <img alt="" src="https://velog.velcdn.com/images/take_the_king/post/346a8681-92a1-4093-a055-906644177d68/image.png" /></p>
</li>
</ul>
<pre><code>- Filter Interface를 Implements하여 구현하고 Bean으로 등록하여 사용한다.
    - `Servlet Container`가 Filter를 Singleton 객체로 생성 및 관리한다.</code></pre><h2 id="filter-구현체">Filter 구현체</h2>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/76138a01-1efd-4f50-b713-306fa5e8cbef/image.png" /></p>
<pre><code class="language-java">@Slf4j
public class CustomFilter implements Filter {
    @Override
    public void doFilter(
            ServletRequest request,
            ServletResponse response,
            FilterChain chain
    ) throws IOException, ServletException {

        // Filter에서 수행할 Logic
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String requestURI = httpRequest.getRequestURI();

        log.info(&quot;request URI={}&quot;, requestURI);
        // chain 이 없으면 Servlet을 바로 호출
        chain.doFilter(request, response);
    }
}</code></pre>
<p>ServletRequest와 ServletResponse를 제공하는 것을 확인할 수 있으며, HttpServletRequest , HttpServletResponse로 다운캐스팅하여 사용할 수 있다.</p>
<blockquote>
<p>필터 인터페이스를 구현하고 등록하면 서블릿 컨테이너가 필터를 싱글톤 객체로 생성하고 관리한다.</p>
</blockquote>
<ul>
<li><p><strong>주요 메서드</strong></p>
<ol>
<li><p><code>init()</code></p>
<ul>
<li>Filter를 초기화하는 메서드이다.</li>
<li>Servlet Container가 생성될 때 호출된다.</li>
<li><code>default</code> ****method이기 때문에 implements 후 구현하지 않아도 된다.</li>
</ul>
</li>
<li><p><code>doFilter()</code></p>
<ul>
<li>Client에서 요청이 올 때 마다 <code>doFilter()</code> 메서드가 호출된다.<ul>
<li><code>doFilter()</code> 내부에 필터 로직(공통 관심사 로직)을 구현하면 된다.</li>
</ul>
</li>
<li>WAS에서 <code>doFilter()</code> 를 호출해주고 하나의 필터의 <code>doFilter()</code>가 통과된다면</li>
<li>Filter  Chain에 따라서 순서대로 <code>doFilter()</code> 를 호출한다.</li>
<li>더이상 <code>doFilter()</code> 를 호출할 Filter가 없으면 Servlet이 호출된다.</li>
</ul>
</li>
<li><p><code>destroy()</code></p>
<ul>
<li>필터를 종료하는 메서드이다.</li>
<li>Servlet Container가 종료될 때 호출된다.</li>
<li><code>default</code> method이기 때문에 implements 후 구현하지 않아도 된다.</li>
</ul>
</li>
</ol>
</li>
</ul>
<h2 id="필터-등록">필터 등록</h2>
<blockquote>
<p>필터 등록 방법에는 여러가지가 있지만, 스프링에서 제공하는 FilterRegistrationBean 을 사용해서 등록하면 된다.</p>
</blockquote>
<pre><code class="language-java">@Configuration
public class WebConfig implements WebMvcConfigurer {

        @Bean
        public FilterRegistrationBean customFilter() {
                FilterRegistrationBean&lt;Filter&gt; filterRegistrationBean = new FilterRegistrationBean&lt;&gt;();
                // Filter 등록
                filterRegistrationBean.setFilter(new CustomFilter());
                // Filter 순서 설정
                filterRegistrationBean.setOrder(1);
                // 전체 URL에 Filter 적용
                filterRegistrationBean.addUrlPatterns(&quot;/*&quot;);

                return filterRegistrationBean;
        }
}</code></pre>
<ul>
<li><code>setFilter()</code><ul>
<li>등록할 필터를 파라미터로 전달하면 된다.</li>
</ul>
</li>
<li><code>setOrder()</code><ul>
<li>Filter는 Chain 형태로 동작한다.</li>
<li>즉, 실행될 Filter들의 순서가 필요하다.</li>
<li>파라미터로 전달될 숫자에 따라 우선순위가 정해진다.</li>
<li>숫자가 낮을수록 우선순위가 높다.</li>
</ul>
</li>
<li><code>addUrlPatterns()</code><ul>
<li>필터를 적용할 URL 패턴을 지정한다.</li>
<li>여러개 URL 패턴을 한번에 지정할 수 있다.</li>
<li>규칙은 Servlet URL Pattern과 같다.</li>
</ul>
</li>
<li><code>filterRegistrationBean.addUrlPatterns(&quot;/*&quot;)</code><ul>
<li>모든 Request는 Custom Filter를 항상 지나간다.</li>
</ul>
</li>
</ul>
<h2 id="간단한-로그인-인증-필터-구현-예시-1">간단한 로그인 인증 필터 구현 예시 1</h2>
<h4 id="whitelist-지정">whitelist 지정</h4>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/8f0e29dc-597f-4fe7-bd7d-8c9dab79c25c/image.png" /></p>
<ul>
<li><p>String[] whitelist : 요청을 허용할 URI 경로를 지정한다.</p>
<p>doFilter() 메서드 내에서 요청 URI가 whitelist에 포함될 경우 바로 다음 필터를 호출하도록 했다.</p>
</li>
</ul>
<h4 id="로그인-필터">로그인 필터</h4>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/fdd35f67-507c-462a-9066-812fb96478e0/image.png" /></p>
<ul>
<li>isLoginCheckPath() : 해당 요청이 whitelist에 있을 경우 바로 다음 필터를 호출하며, 그렇지 않을 경우 사용자를 확인하는 로직을 수행한다.</li>
</ul>
<p>해당 메서드 내에서는 세션이 존재하는지 확인 후, 존재하지 않으면 로그인 화면으로 redirect 한다.</p>
<ul>
<li>sendRedirect(&quot;/login?redirectURL=&quot; + requestURI) : 이때, 로그인 후 사용자가 기존에 요청했던 화면으로 바로 이동시키기 위해 파라미터로 기존 요청 URI를 넘긴다.</li>
</ul>
<p>이후 컨트롤러에서는 해당 요청의 ID/Password를 확인하고, 일치할 경우 기존의 요청 경로로 redirect 해주면 된다.</p>
<h2 id="간단한-로그인-인증-필터-구현-예시-2">간단한 로그인 인증 필터 구현 예시 2</h2>
<h4 id="login-filter-만들기">Login Filter 만들기</h4>
<pre><code class="language-java">@Slf4j
public class LoginFilter implements Filter {
    // 인증을 하지 않아도될 URL Path 배열
    private static final String[] WHITE_LIST = {&quot;/&quot;, &quot;/user/signup&quot;, &quot;/login&quot;, &quot;/logout&quot;};

    @Override
    public void doFilter(
            ServletRequest request,
            ServletResponse response,
            FilterChain chain
    ) throws IOException, ServletException {

        // 다양한 기능을 사용하기 위해 다운 캐스팅
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String requestURI = httpRequest.getRequestURI();

        // 다양한 기능을 사용하기 위해 다운 캐스팅
        HttpServletResponse httpResponse = (HttpServletResponse) response;

        log.info(&quot;로그인 필터 로직 실행&quot;);

        // 로그인을 체크 해야하는 URL인지 검사
        // whiteListURL에 포함된 경우 true 반환 -&gt; !true = false
        if (!isWhiteList(requestURI)) {

            // 로그인 확인 -&gt; 로그인하면 session에 값이 저장되어 있다는 가정.
            // 세션이 존재하면 가져온다. 세션이 없으면 session = null
            HttpSession session = httpRequest.getSession(false);

            // 로그인하지 않은 사용자인 경우
            if (session == null || session.getAttribute(&quot;sessionKey값&quot;) == null) {
                throw new RuntimeException(&quot;로그인 해주세요.&quot;);
            }

            // 로그인 성공 로직

        }

        // 1번경우 : whiteListURL에 등록된 URL 요청이면 바로 chain.doFilter()
        // 2번경우 : 필터 로직 통과 후 다음 필터 호출 chain.doFilter()
        // 다음 필터 없으면 Servlet -&gt; Controller 호출
        chain.doFilter(request, response);


    }

    // 로그인 여부를 확인하는 URL인지 체크하는 메서드
    private boolean isWhiteList(String requestURI) {
        // request URI가 whiteListURL에 포함되는지 확인
        // 포함되면 true 반환
        // 포함되지 않으면 false 반환
        return PatternMatchUtils.simpleMatch(WHITE_LIST, requestURI);
    }
}</code></pre>
<h4 id="filter-등록하기">Filter 등록하기</h4>
<pre><code class="language-java">@Configuration
public class WebConfig {

        @Bean
        public FilterRegistrationBean customFilter() {
                FilterRegistrationBean&lt;Filter&gt; filterRegistrationBean = new FilterRegistrationBean&lt;&gt;();
                filterRegistrationBean.setFilter(new CustomFilter()); // Filter 등록
                filterRegistrationBean.setOrder(1); // Filter 순서 1 설정
                filterRegistrationBean.addUrlPatterns(&quot;/*&quot;); // 전체 URL에 Filter 적용

                return filterRegistrationBean;
        }

        @Bean
        public FilterRegistrationBean loginFilter() {
                FilterRegistrationBean&lt;Filter&gt; filterRegistrationBean = new FilterRegistrationBean&lt;&gt;();
                filterRegistrationBean.setFilter(new LoginFilter()); // Filter 등록
                filterRegistrationBean.setOrder(2); // Filter 순서 2 설정
                filterRegistrationBean.addUrlPatterns(&quot;/*&quot;); // 전체 URL에 Filter 적용

                return filterRegistrationBean;
        }
}</code></pre>
<ul>
<li><p><code>loginFilter()</code> 메서드 내부의 <code>addUrlPatterns()</code> 를 활용하여 URL 패턴을 지정할 수 있다.</p>
<ul>
<li><p>코드 내부에 <code>WHITE_LIST</code> 가 존재하기 때문에 전체 허용</p>
</li>
<li><p><code>addUrlPatterns()</code> 에 URL 패턴을 직접 지정해두면 추후 수정할 가능성이 높다.</p>
<ul>
<li><p>새로운 URL Mapping이 있는 Controller에 인증 확인을 해야할 경우</p>
<p>ex) <code>“/post/*”</code></p>
</li>
</ul>
</li>
</ul>
</li>
</ul>