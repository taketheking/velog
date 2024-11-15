<h2 id="1-디렉토리-구조">1. 디렉토리 구조</h2>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/56935db4-1863-422c-a017-7b621dc21e71/image.PNG" /></p>
<p>이번에는 프로젝트를 계층 구조가 아닌 도메인 구조로 구성해봤다.
domain에는 Schedule, User가 있고 base는 공통으로 필요한 클래스를 모아두는 패키지로 사용했다.
global는 MVC 외에 사용되는 보조적으로 Filter나 Exception 클래스 등을 모아놨다.</p>
<p>어떤 프로젝트 구조가 좋을지는 더 고민해봐야겠다.</p>
<h2 id="2-로그인-기능---session-사용">2. 로그인 기능 - Session 사용</h2>
<ul>
<li><p>UserController 내의 login</p>
<pre><code class="language-java">@PostMapping(&quot;/login&quot;)
 public ResponseEntity&lt;Void&gt; login(@Valid @RequestBody LoginRequestDto requestDto, BindingResult bindingResult, HttpServletRequest request) {
     if (bindingResult.hasErrors()) {
         throw new BadValueException(String.valueOf(bindingResult.getFieldError().getDefaultMessage()));
     }

     User user = userService.login(requestDto.getEmail(), requestDto.getPassword());

     HttpSession session = request.getSession();
     session.setAttribute(Const.LOGIN_USER, user);

     return new ResponseEntity&lt;&gt;(HttpStatus.OK);
 }</code></pre>
</li>
</ul>
<p>여기서 email과 비밀번호가 일치하는 유저 정보가 있으면 session를 생성하고 로그인한다.</p>
<p>이 부분에서 이해가 안되었던 것은 session을 response에 담거나 클라이언트에게 보낸 적이 없는데 session의 id가 클라이언트에게 response의 쿠키에 담겨 날아간다는 것이다.</p>
<p>이것은 서블릿 컨테이너가 알아서 담아서 보내주는 것이라고 하는데 나중에 더 공부해야겠다.</p>
<ul>
<li><p>LoginFilter 내의 doFilter</p>
<pre><code class="language-java">@Override
  public void doFilter(
          ServletRequest request,
          ServletResponse response,
          FilterChain chain
  ) throws IOException, ServletException, ResponseStatusException {

      // 다양한 기능을 사용하기 위해 다운 캐스팅
      HttpServletRequest httpRequest = (HttpServletRequest) request;
      String requestURI = httpRequest.getRequestURI();

      // 로그인을 체크 해야하는 URL인지 검사
      // whiteListURL에 포함된 경우 true 반환 -&gt; !true = false
      if (!isWhiteList(requestURI)) {

          // 로그인 확인 -&gt; 로그인하면 session에 값이 저장되어 있다는 가정.
          // 세션이 존재하면 가져온다. 세션이 없으면 session = null
          HttpSession session = httpRequest.getSession(false);
          // 로그인하지 않은 사용자인 경우
          if (session == null || session.getAttribute(Const.LOGIN_USER) == null) {
              throw new ResponseStatusException(HttpStatus.UNAUTHORIZED, &quot;로그인을 해주세요.&quot;);
          }
      }
      // 1번경우 : whiteListURL에 등록된 URL 요청이면 바로 chain.doFilter()
      // 2번경우 : 필터 로직 통과 후 다음 필터 호출 chain.doFilter()
      // 다음 필터 없으면 Servlet -&gt; Controller 호출
      chain.doFilter(request, response);
  }</code></pre>
</li>
</ul>
<p>이 코드에서도 보면 session의 id값을 비교하는 곳이 나와있지 않다.
클라이언트가 보내온 session의 id와 서버가 가지고 있는 session의 id를 비교해야 그 세션이 맞는지 아닌지 확인을 할 수 있을텐데 말이다.</p>
<p>그래서 테스트를 해봤다. </p>
<pre><code class="language-java">if (session == null || session.getAttribute(Const.LOGIN_USER) == null)</code></pre>
<p>이 코드를</p>
<pre><code class="language-java">if (session == null)</code></pre>
<p>이렇게 변경하고</p>
<p>로그인을 하여 세션이 존재할 때 세션 id를 변경해서 요청을 해봤다. 
그런데 에러가 발생했다. session이 null값이라는 의미이다.
아마도 이것도 서블릿 컨테이너가 session의 id를 비교해서 같은 id가 없으면 null을 반환해주는 것이라 이해했다.</p>
<h2 id="3-필터-내의-예외처리">3. 필터 내의 예외처리</h2>
<p>WAS 서버의 필터에서는 ControllerAdice가 예외를 잡지 않는다. 왜냐하면 ControllerAdice는 스프링 컨테이너에서 동작하기 때문이다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/a14f2be5-a472-40fe-a049-0fac36f75182/image.png" /></p>
<p>필터는 스프링 컨테이너에 도달하기 전 단계이기 때문에 예외가 발생해도 모른다.
필터는 Spring Context 이전의 Servlet Context에서 관리되는 영역이다.</p>
<ul>
<li><p>ExceptionHandlerFilter의 doFilterInternal</p>
<pre><code class="language-java">@Override
  protected void doFilterInternal(@NonNull HttpServletRequest request, @NonNull HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
      try{
          filterChain.doFilter(request, response);
      }catch (ResponseStatusException e){

          response.setStatus(HttpStatus.UNAUTHORIZED.value());
          response.setContentType(MediaType.APPLICATION_JSON_VALUE);
          response.setCharacterEncoding(&quot;UTF-8&quot;); // UTF-8 인코딩 설정

          response.getWriter().write(e.getMessage());

      }
  }</code></pre>
</li>
</ul>
<p>그래서 이 프로젝트에서 login 필터에서 발생하는 에러를 처리하기 위해 login 필터 앞에 새로운 ExceptionHandlerFilter를 만들어서 예외를 잡아서 직접 response하는 방법으로 예외를 처리했다.</p>
<blockquote>
</blockquote>
<ul>
<li>OncePerRequestFilter는 왜 썻는가?
참고 : <a href="https://junyharang.tistory.com/378">https://junyharang.tistory.com/378</a></li>
</ul>
<h2 id="4-dto에-필드가-한-개일-때-생기는-오류">4. dto에 필드가 한 개일 때 생기는 오류</h2>
<blockquote>
</blockquote>
<p>DefaultHandlerExceptionResolver : Resolved [org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: Cannot construct instance of <code>com.example.schedulewithjpa.domain.User.dto.DeleteRequestDto</code> (although at least one Creator exists): cannot deserialize from Object value (no delegate- or property-based Creator)]</p>
<p>jackson library가 빈 생성자(기본 생성자)가 없는 모델을 생성하는 방법을 몰라서 발생하는 에러이다.
JSON 데이터를 Java Object로 변환하기 위해서 빈 생성자가 필요하다. </p>
<p>Jackson은 Java에서 객체를 JSON 문자열을 변환(역직렬화)하거나 JSON 문자열을 객체로 변환(직렬화)하는 기능을 제공하는 라이브러리이다.</p>
<p>만약 자바 객체에서 생성자가 없는 경우 Jackson에서 InvalidDefinitionException 예외를 발생한다.</p>
<p>final 변수를 가지고 있는 클래스의 인스턴스를 생성할 때는 final 변수들의 값을 생성자에서 초기화 줘야 한다. 그래서 기본 생성자를 사용할 수 없다.</p>
<p>하지만 Jackson에서는 기본 생성자를 통해 JSON 데이터를 역직렬화한다. 그래서 에러가 발생했던 것이다.</p>
<p>방법 1은 final을 제거하고 기본 생성자를 생성하는 것이다.</p>
<p>방법 2는 엄격하게 객체를 불변으로 두기 위해서 final을 유지하고 싶다면 @JsonCreator 와 @JsonProperty 애너테이션을 사용하여 final 변수에 값을 주입하는 것이다.</p>
<p>@JsonCreator 애너테이션은 Jackson이 역직렬화할 생성자를 지정해 준다.</p>
<p>@JsonProperty 애너테이션은 Jackson이 역직렬화할 JSON 데이터의 이름을 지정해 준다.</p>
<p>참고 : <a href="https://green-bin.tistory.com/80">https://green-bin.tistory.com/80</a>
참고 : <a href="https://kong-dev.tistory.com/236">https://kong-dev.tistory.com/236</a></p>
<h2 id="5-objectmapper에서-localdatetime이-변환되지않는-오류">5. ObjectMapper에서 LocalDateTime이 변환되지않는 오류</h2>
<p>객체를 직접 매핑해서 response로 전달하는 과정에서 localdatetime에 의한 오류가 발생했다.</p>
<blockquote>
</blockquote>
<p>com.fasterxml.jackson.databind.exc.InvalidDefinitionException: Java 8 date/time type <code>java.time.LocalDateTime</code> not supported by default: add Module &quot;com.fasterxml.jackson.datatype:jackson-datatype-jsr310&quot; to enable handling at [Source: ...중략 ]</p>
<p>오류 메시지를 해석하면 다음과 같습니다. Java 8의 날짜/시간 유형은 기본적으로 지원되지 않습니다. 이를 처리하도록 사용하려면 com.fastxml.jackson.datatype:jsr310 모듈을 추가해야 합니다.
라고 되어있다.</p>
<p>참고 : <a href="https://woo-chang.tistory.com/75">https://woo-chang.tistory.com/75</a></p>
<p>하지만 오류 메세지에 시간을 표시하는게 당장 중요한 것이 아니기 때문에 빼고 
나중에 직렬화를 공부할 때 다시 찾아보려고 한다.</p>
<h2 id="6-영속성-전이">6. 영속성 전이</h2>
<p>앞으로 공부......</p>