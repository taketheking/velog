<p>쿠키와 세션은 웹 개발에서 사용자 경험을 개선하고, 데이터를 효율적으로 관리하는 중요한 도구입니다. 이 둘은 웹 애플리케이션과 사용자 간의 상태를 관리하기 위해 사용되지만, 각기 다른 방식과 목적을 가집니다.</p>
<h2 id="1-쿠키-cookie">1. 쿠키 (Cookie)</h2>
<p>쿠키는 클라이언트(웹 브라우저)에 저장되는 작은 데이터 조각입니다. 웹 사이트는 쿠키를 사용하여 사용자의 세션 상태, 사용자 설정, 방문 기록 등을 저장할 수 있습니다. 쿠키는 서버에서 클라이언트로 전송되며, 사용자가 해당 웹 사이트를 다시 방문할 때마다 클라이언트가 쿠키를 서버로 전송합니다.</p>
<h3 id="1-쿠키가-필요한-이유">1) 쿠키가 필요한 이유</h3>
<ol>
<li>HTTP는 Stateless, Connectionless 특성을 가지고 있다.</li>
<li>Client가 재요청시 Server는 이전 요청에 대한 정보를 기억하지 못한다.</li>
<li>로그인과 같이 상태를 유지해야 하는 경우가 발생한다.</li>
<li>Request에 사용자 정보를 포함하면 해결이 된다.<ul>
<li>로그인 후에는 사용자 정보와 관련된 값이 저장되어 있어야한다.</li>
</ul>
</li>
<li>브라우저를 완전히 종료한 뒤 다시 열어도 사용자 정보가 유지되어야 한다.</li>
</ol>
<h3 id="2-사용-예시">2) 사용 예시</h3>
<ul>
<li>자동 로그인 기능</li>
<li>사용자 설정 (언어, 테마 등)</li>
<li>장바구니 상태 유지</li>
</ul>
<h3 id="3-쿠키-동작-순서">3) 쿠키 동작 순서</h3>
<ol>
<li><p>클라이언트 요청: 사용자가 웹 브라우저에서 웹 사이트에 처음 접속할 때, 요청은 서버로 전송됩니다.</p>
</li>
<li><p>서버 응답 및 쿠키 전송: 서버는 클라이언트의 요청을 처리한 후, 쿠키를 생성하고 Set-Cookie HTTP 헤더에 담아 클라이언트에 쿠키를 전송합니다.</p>
<pre><code class="language-http">// 세션 쿠키 전송
HTTP/1.1 200 OK
Set-Cookie: sessionId=abc123; Path=/; Expires=Wed, 25 Dec 2024 23:59:59 GMT</code></pre>
<p>위의 쿠키는 서블릿 내부에서 자동으로 생성하는 세션쿠키이다.</p>
</li>
<li><p>클라이언트의 쿠키 저장: 브라우저는 서버로부터 받은 쿠키를 로컬에 저장합니다.</p>
</li>
<li><p>후속 요청 시 쿠키 전송: 클라이언트가 동일한 웹 사이트에 재접속할 때마다 브라우저는 저장된 쿠키를 자동으로 포함하여 서버로 전송합니다.</p>
<pre><code class="language-http">GET / HTTP/1.1
Cookie: sessionId=abc123</code></pre>
</li>
<li><p>서버의 쿠키 확인: 서버는 요청 시 클라이언트가 보낸 쿠키를 사용해 사용자 식별이나 사용자 상태 정보를 확인하고 적절한 응답을 반환합니다.</p>
</li>
</ol>
<h3 id="4-코드-예제-java-servlet">4) 코드 예제 (Java Servlet)</h3>
<p>다음은 Java Servlet에서 쿠키를 설정하고 읽는 예제입니다.</p>
<pre><code class="language-java">// 쿠키 읽기
Cookie[] cookies = request.getCookies();
if (cookies != null) {
    for (Cookie cookie : cookies) {
        if (&quot;username&quot;.equals(cookie.getName())) {
            System.out.println(&quot;Username: &quot; + cookie.getValue());
        }
    }
}</code></pre>
<p>httpRequest의 getCookies 메서드를 통해 쿠키들을 가지고 오는데, 이 쿠키들은 클라이언트가 보낸 요청의 header의 Cookie 속성에서 가지고 오는 것이다.</p>
<p>클라이언트가 맨 처음 요청을 보내는 상황이나, 서버에서 받은 쿠키가 없으면 쿠키 없이 요청을 보내면 cookie는 null값이 있다.</p>
<pre><code class="language-java">// 쿠키 생성 및 설정
Cookie userCookie = new Cookie(&quot;username&quot;, &quot;JohnDoe&quot;);
userCookie.setMaxAge(60 * 60 * 24); // 1일 동안 유지
response.addCookie(userCookie);</code></pre>
<p>httpResponse의 addCookie 메서드에 새로 생성한 Cookie를 입력하면 
클라이언트에게 응답에 header의 set-Cookie 속성에 해당 쿠키값으로 전달된다.</p>
<blockquote>
</blockquote>
<ul>
<li>Response 알아보기<pre><code>set-cookie: 
sessionId=abcd; 
expires=Sat, 11-Dec-2024 00:00:00 GMT;
path=/; 
domain=spartacodingclub.kr;
Secure</code></pre></li>
</ul>
<h3 id="5-cookie의-생명주기">5) Cookie의 생명주기</h3>
<ol>
<li><p><strong>세션 Cookie</strong> </p>
<ul>
<li>만료 날짜를 생략하면 브라우저 완전 종료시 까지만 유지된다.(<code>Default</code>)<ul>
<li><code>expires, max-age</code> 가 미설정한 경우</li>
<li><code>setMaxAge(-1)</code> 인 경우</li>
</ul>
</li>
<li>브라우저를 완전 종료 후 다시 페이지를 방문했을 때 다시 로그인을 해야한다.</li>
</ul>
</li>
<li><p><strong>영속 Cookie</strong></p>
<ul>
<li><p>만료 날짜를 입력하면 해당 날짜까지 유지한다.</p>
<ul>
<li><p><code>expires=Sat, 11-Dec-2024 00:00:00 GMT;</code></p>
<ul>
<li>해당 만료일이 도래하면 쿠키가 삭제된다.</li>
</ul>
</li>
<li><p><code>max-age=3600</code> (second, 3600초는 한시간. 60 * 60)</p>
<ul>
<li>0이 되거나 음수를 지정하면 쿠키가 삭제된다.</li>
</ul>
</li>
<li><p>영속 Cookie는 파일로 로컬 저장소에 저장된다.</p>
</li>
</ul>
</li>
</ul>
</li>
</ol>
<h3 id="6-cookie의-도메인">6) Cookie의 도메인</h3>
<ul>
<li>쿠키가 아무 사이트에서나 생기고 동작하면 안된다!<ul>
<li>필요없는 값 전송, 트래픽 문제 등이 발생한다.</li>
</ul>
</li>
<li><code>domain=spartacodingclub.kr</code><ul>
<li><code>domain=spartacodingclub.kr</code>를 지정하여 쿠키를 저장한다.</li>
<li><code>dev.spartacodingclub.kr</code>와 같은 서브 도메인에서도 쿠키에 접근한다.</li>
</ul>
</li>
<li>domain을 생략하면 현재 문서 기준 도메인만 적용한다.</li>
</ul>
<h3 id="7-cookie의-경로">7) Cookie의 경로</h3>
<ul>
<li>1차적으로 도메인으로 필터링 후 Path가 적용된다.</li>
<li>일반적으로 <code>path=/</code> 루트(전체)로 지정한다.</li>
<li>위 경로를 포함한 하위 경로 페이지만 쿠키에 접근한다.<ul>
<li><code>path=/api</code> 지정<ul>
<li><code>path=/api/example</code> 가능</li>
<li><code>path=/example</code> 불가능</li>
</ul>
</li>
</ul>
</li>
</ul>
<h3 id="8-cookie-보안">8) Cookie 보안</h3>
<ol>
<li><code>Secure</code><ul>
<li>기본적으로 Cookie는 http, https 구분하지 않고 전송한다.</li>
<li>Secure를 적용하면 https인 경우에만 전송한다. s = Secure</li>
</ul>
</li>
<li><code>HttpOnly</code><ul>
<li>XSS(Cross-site Scripting) 공격을 방지한다.<ul>
<li>악성 스크립트를 웹 페이지에 삽입하여 다른 사용자의 브라우저에서 실행되도록 하는 공격</li>
</ul>
</li>
<li>자바스크립트에서 Cookie 접근을 못하도록 막는다.</li>
<li>HTTP 요청시 사용한다.</li>
</ul>
</li>
<li><code>SameSite</code><ul>
<li>비교적 최신 기능이라 브라우저 지원여부를 확인 해야한다.</li>
<li>CSRF(Cross-Site Request Forgery) 공격을 방지한다.<ul>
<li>사용자가 의도하지 않은 상태에서 특정 요청을 서버에 전송하게 하여 사용자 계정에서 원치 않는 행동을 하게 만든다.</li>
</ul>
</li>
<li>요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송</li>
</ul>
</li>
</ol>
<pre><code class="language-java">// 보안을 강화한 쿠키 설정
Cookie secureCookie = new Cookie(&quot;userToken&quot;, &quot;encryptedValue&quot;);
secureCookie.setHttpOnly(true); // 자바스크립트를 통한 접근 방지
secureCookie.setSecure(true); // HTTPS를 통해서만 전송
response.addCookie(secureCookie);
</code></pre>
<h3 id="9-장점">9) 장점</h3>
<p>클라이언트 측에서 관리되므로 서버 리소스를 사용하지 않음.
사용자 설정 정보를 쉽게 저장 및 유지할 수 있음.</p>
<h3 id="10-단점">10) 단점</h3>
<p>보안 이슈: 쿠키는 쉽게 탈취 및 조작될 수 있으므로 민감한 정보는 저장하면 안 됨.
용량 제한: 쿠키는 각 도메인당 최대 4KB의 데이터를 저장할 수 있음.</p>
<hr />
<h2 id="2-세션-session">2. 세션 (Session)</h2>
<p>세션은 <strong>서버 측에서 관리되는 사용자 상태 정보를 저장</strong>하는 방식입니다. 서버는 각 사용자마다 고유한 세션 ID를 생성하고, 이 세션 ID를 통해 사용자의 상태 정보를 관리합니다. 클라이언트는 세션 ID를 쿠키에 저장하거나 URL 파라미터로 전달하여 서버에 접근할 때마다 세션 정보를 식별합니다.</p>
<h3 id="1-사용-예시">1) 사용 예시</h3>
<ul>
<li>로그인 상태 유지</li>
<li>쇼핑몰의 장바구니 정보 저장</li>
<li>사용자 맞춤형 페이지 렌더링</li>
</ul>
<h3 id="2-세션-동작-순서">2) 세션 동작 순서</h3>
<h4 id="1-클라이언트-요청-및-세션-생성">1. 클라이언트 요청 및 세션 생성</h4>
<p>클라이언트가 웹 애플리케이션에 처음 접속하면 서버는 새로운 세션을 생성하여 고유한 세션 ID를 클라이언트에 할당합니다. 이 과정에서 브라우저는 서버로부터 세션 ID를 쿠키 형태로 수신하여 클라이언트에 저장합니다.</p>
<pre><code class="language-java">// 클라이언트 요청 처리
HttpSession session = request.getSession(); // 세션이 없으면 새로 생성, 있으면 기존 세션 반환

// 세션이 새로 생성된 경우 확인
if (session.isNew()) {
    System.out.println(&quot;새로운 세션이 생성되었습니다.&quot;);
    session.setAttribute(&quot;username&quot;, &quot;JohnDoe&quot;); // 세션에 데이터 저장
} else {
    System.out.println(&quot;기존 세션을 사용 중입니다.&quot;);
}</code></pre>
<p>request의 getSession 메서드를 실행하면 세션이 생성되면서 session id도 생성이 된다.
그리고 서블릿에서 request의 getSession 메서드를 실행하면 내부적으로 response의 set-Cookie 속성에 session의 id를 <code>JSESSIONID=abc123xyz</code> 형태로 담아서 응답할 때 보내준다.</p>
<h4 id="2-세션-id-전송-및-저장">2. 세션 ID 전송 및 저장</h4>
<p>서버는 세션 ID를 Set-Cookie 헤더에 포함하여 클라이언트로 전송합니다. 클라이언트의 브라우저는 이 세션 ID를 쿠키로 저장하고, 이후 모든 요청에 해당 세션 ID를 자동으로 서버에 전송합니다.</p>
<pre><code class="language-http">HTTP/1.1 200 OK
Set-Cookie: JSESSIONID=abc123xyz; Path=/; HttpOnly</code></pre>
<p>이 JSESSIONID 쿠키는 클라이언트의 브라우저에 저장되며, 이후 요청마다 자동으로 전송되어 사용자의 세션을 식별합니다.</p>
<h4 id="3-클라이언트의-후속-요청">3. 클라이언트의 후속 요청</h4>
<p>클라이언트가 같은 웹 애플리케이션에 재접속할 때, 브라우저는 JSESSIONID 쿠키를 서버로 전송하여 현재 세션을 참조합니다.</p>
<pre><code class="language-java">// 후속 요청에서 세션 확인 및 데이터 읽기
HttpSession session = request.getSession(false); // 세션이 없으면 null 반환
if (session != null) {
    String username = (String) session.getAttribute(&quot;username&quot;);
    System.out.println(&quot;사용자 이름: &quot; + username);
} else {
    System.out.println(&quot;세션이 없습니다. 새로 생성이 필요합니다.&quot;);
}</code></pre>
<p>위 코드에서 request.getSession(false)는 기존 세션이 없으면 null을 반환하므로, 기존 세션의 존재 여부를 확인할 수 있습니다.</p>
<h4 id="4-세션-만료와-관리">4. 세션 만료와 관리</h4>
<p>서버 측에서 세션은 일정 시간 동안 유지되며, 사용자가 지정된 시간 동안 활동하지 않으면 만료됩니다. 세션의 만료 시간은 setMaxInactiveInterval 메서드를 사용해 설정할 수 있습니다.</p>
<pre><code class="language-java">// 세션 만료 시간 설정 (초 단위)
HttpSession session = request.getSession();
session.setMaxInactiveInterval(30 * 60); // 30분 동안 유지

// 세션 만료 후 처리
if (session == null || session.getAttribute(&quot;username&quot;) == null) {
    response.sendRedirect(&quot;login.jsp&quot;); // 세션이 만료된 경우 로그인 페이지로 리다이렉트
}</code></pre>
<h4 id="5-세션-삭제-및-무효화">5. 세션 삭제 및 무효화</h4>
<p>로그아웃 시에는 세션을 명시적으로 무효화하여 사용자 정보를 삭제할 수 있습니다.</p>
<pre><code class="language-java">// 세션 무효화 (로그아웃 시 호출)
HttpSession session = request.getSession(false);
if (session != null) {
    session.invalidate(); // 세션 삭제
    System.out.println(&quot;세션이 무효화되었습니다.&quot;);
}</code></pre>
<h3 id="2-코드-예제-java-servlet">2) 코드 예제 (Java Servlet)</h3>
<p>다음은 Java Servlet에서 세션을 생성하고 값을 읽고 쓰는 예제입니다.</p>
<pre><code class="language-java">// 세션 생성 및 값 저장
HttpSession session = request.getSession();
session.setAttribute(&quot;user&quot;, &quot;JohnDoe&quot;);

// 세션 만료 설정
session.setMaxInactiveInterval(30 * 60); // 30분 동안 유지

// 세션에서 값 읽기
String username = (String) session.getAttribute(&quot;user&quot;);
System.out.println(&quot;Username from session: &quot; + username);</code></pre>
<h3 id="3-서블릿-컨텍스트와-세션-저장소">3) 서블릿 컨텍스트와 세션 저장소</h3>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/c31dc605-181a-4574-aae0-c635cb12c177/image.png" /></p>
<h4 id="서블릿-컨텍스트servlet-context">서블릿 컨텍스트(Servlet Context)</h4>
<ul>
<li>하나의 톰캣은 여러 개의 웹 애플리케이션(웹 프로젝트)을 실행할 수 있음</li>
<li>실제 운영의 경우 웹 애플리케이션마다 별도의 도메인으로 분리해서 운영</li>
<li>프로젝트 실행 경로를 ‘/’ 외에 다른 이름으로 각각 지정해서 실행하면 하나의 톰캣 내에 여러 웹 애플리케이션 실행 가능</li>
<li>각각의 웹 애플리케이션은 자신만이 사용하는 고유의 메모리 영역을 하나 생성해서 이 공간에 서블릿이나 JSP 등을 인스턴스로 만들어 서비스를 제공<ul>
<li>⇒ 이 영역을 서블릿 API에서는 서블릿 컨텍스트라고 함</li>
</ul>
</li>
</ul>
<h4 id="세션-저장소session-repository">세션 저장소(Session Repository)</h4>
<ul>
<li>각각의 웹 애플리케이션을 생성할 때는 톰캣이 발행하는 쿠키(개발자가 생성하는 쿠키와 구분하기 위해서 세션 쿠키라고 함)들을 관리하기 위한 메모리 영역이 하나 더 생성 ⇒ 세션 저장소</li>
<li>기본적으로 키(key)와 값(value)을 보관하는 구조<ul>
<li>이때 키가 되는 역할을 하는 것이 톰캣에서 JSESSIONID라는 쿠키의 값이 됨</li>
</ul>
</li>
<li>톰캣 내부의 세션 저장소는 발행된 쿠키들의 정보를 보관하는 역할</li>
<li>새로운 JSESSIONID 쿠키가 만들어 질때마다 메모리 공간을 차지</li>
<li>위 문제를 해결하기 위해 톰캣은 주기적으로 세션 저장소를 조사하면서 더 이상 사용하지 않는 값들을 정리</li>
</ul>
<h3 id="4-세션을-통한-상태-유지-메커니즘">4) 세션을 통한 상태 유지 메커니즘</h3>
<p>코드 상에서 HttpServletRequest의 getSession()이라는 메서드를 실행하면 톰캣에서는 JSESSIONID 이름의 쿠키가 요청(Request)할 때 있었는지 확인</p>
<p>→ 없으면 새로운 값을 만들어 세션 저장소에 보관
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/066907f2-8afb-4374-aabd-98660625feab/image.png" /></p>
<p>3개의 브라우저가 처음으로 세션이 필요한 경로를 요청했다고 가정하고, JSESSIONID 값이 각각 ‘A1234’, ‘B1111’, ‘C3333’과 같았다고 생각하면 다음과 같은 구조가 됨
세션 저장소에서는 JSESSIONID의 값마다 고유한 공간(세션 컨텍스트)을 가지게 되는데, 이 공간(세션 컨텍스트)은 다시 키(key)와 값(value)으로 데이터를 보관할 수 있음</p>
<p>이 공간들을 이용해서 서블릿/JSP 등은 원하는 객체들을 보관할 수 있는데, 다른 객체들은 다음과 같은 형태로 보관할 수 있게 됨
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/dbf69953-c511-4f56-b4dc-aa2c2d4a07ca/image.png" /></p>
<p>‘A1234’와 ‘B1111’은 자신이 사용하는 공간에 ‘login 정보’가 존재
서버에서 프로그램을 작성할 때는 이를 이용해 해당 사용자가 로그인했다는 것을 인정하는 방식
서블릿 API에서는 HttpServletRequest를 통해 getSession()이라는 메서드로 각 JSESSIONID의 공간에 접근할 수 있음</p>
<h3 id="5-장점">5) 장점</h3>
<p>서버 측에서 데이터를 관리하므로 클라이언트에 민감한 데이터를 저장하지 않음.
대용량 데이터 저장 가능.</p>
<h3 id="6-단점">6) 단점</h3>
<p>서버 리소스 소모: 사용자가 많아질수록 서버 메모리를 더 많이 사용.
상태 유지: 서버가 재시작되면 세션 정보가 손실될 수 있음.</p>
<hr />
<h2 id="3-사용자-정의-쿠키와-세션-쿠키의-차이">3. 사용자 정의 쿠키와 세션 쿠키의 차이</h2>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/b61a5bc5-f23a-47d3-8bbb-d7b8312e00fa/image.PNG" /></p>
<h2 id="4-쿠키와-세션의-차이점">4. 쿠키와 세션의 차이점</h2>
<ul>
<li>쿠키와 세션은 비슷한 역할을 하며, 동작 원리도 비슷하다. 그 이유는 세션도 결국 쿠키를 사용하기 때문이다.</li>
</ul>
<ul>
<li><p>큰 차이점은 사용자의 정보가 저장되는 위치이다. 쿠키는 서버의 자원을 전혀 사용하지 않으며, 세션은 서버의 자원을 사용한다.</p>
</li>
<li><p>보안 면에서 세션이 더 우수하며, 쿠키는 클라이언트 로컬에 저장되기 때문에 변질되거나 request에서 스니핑 당할 우려가 있어서 보안에 취약하지만 세션은 쿠키를 이용해서 session-id만 저장하고 그것으로 구분하여 서버에서 처리하기 때문에 비교적 보안성이 높다.</p>
</li>
<li><p>라이프 사이클은 쿠키도 만료기간이 있지만 파일로 저장되기 때문에 브라우저를 종료해도 정보가 유지될 수 있다. 또한 만료기간을 따로 지정 해 쿠키를 삭제할 때까지 유지할 수도 있다.</p>
</li>
<li><p>반면에 세션도 만료기간을 정할 수 있지만, 브라우저가 종료되면 만료기간에 상관없이 삭제된다.
속도 면에서 쿠키가 더 우수하며, 쿠키는 쿠키에 정보가 있기 때문에 서버에 요청 시 속도가 빠르고 세션은 정보가 서버에 있기 때문에 처리가 요구되어 비교적 느린 속도를 낸다.</p>
</li>
</ul>
<blockquote>
</blockquote>
<p>보통 쿠키와 세션의 차이에 대해서 저장 위치와 보안에 대해서는 잘 알고 있지만, 사실 가장 중요한 것은 라이프사이클이다.</p>
<h2 id="5-쿠키와-세션의-결합-사용">5. 쿠키와 세션의 결합 사용</h2>
<p>일반적으로 쿠키는 세션 ID를 클라이언트 측에 저장하여, 세션을 식별하는 데 사용됩니다. 이를 통해 서버는 각 사용자에 대한 세션을 관리할 수 있으며, 클라이언트는 세션 정보를 별도로 유지할 필요가 없습니다.</p>
<pre><code class="language-java">// 세션 ID를 쿠키에 저장하는 예제
Cookie sessionCookie = new Cookie(&quot;JSESSIONID&quot;, session.getId());
sessionCookie.setHttpOnly(true); // XSS 방지
response.addCookie(sessionCookie);</code></pre>
<blockquote>
</blockquote>
<p>일반적으로는 requset.getSession으로 세션 생성시에 response에 서블릿이 자동으로 set-Cookie에 세션ID를 쿠키로 저장한다.</p>
<hr />
<p>참고 및 이미지 출처 :
<a href="https://dev-coco.tistory.com/61">https://dev-coco.tistory.com/61</a></p>
<p><a href="https://catsbi.oopy.io/0c27061c-204c-4fbf-acfd-418bdc855fd8">https://catsbi.oopy.io/0c27061c-204c-4fbf-acfd-418bdc855fd8</a>
 <a href="https://velog.io/@reasonoflife39/%EC%84%B8%EC%85%98%EA%B3%BC-%ED%95%84%ED%84%B0-%EC%BF%A0%ED%82%A4">https://velog.io/@reasonoflife39/%EC%84%B8%EC%85%98%EA%B3%BC-%ED%95%84%ED%84%B0-%EC%BF%A0%ED%82%A4</a></p>