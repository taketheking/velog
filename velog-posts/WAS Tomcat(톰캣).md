<p>WAS는 클라이언트의 요청을 받아 동적 컨텐츠를 생성하거나 비지니스 로직을 수행한다.</p>
<p>톰캣은 Jakarta EE 표준 스펙을 구현해 서블릿, JSP로 작성된 애플리케이션을  실행하는 서버이다. but Jakarta EE 표준 스펙을 완벽하게 구현하고 있지 않기 때문에 완전한 WAS는 아니다.</p>
<p>톰캣은 서블릿 컨테이너로써 서블릿 메서드를 생성 및 관리하고, 관련 서비스를 제공합니다. 그러나 Jakarta EE 표준 스펙 중 서블릿과 JSP 사양을 구현하고 있지만 WAS의 EJB 등과 같은 엔터프라이즈 기능을 제공하지 않기 때문이다.</p>
<h2 id="1-배경-지식">1. 배경 지식</h2>
<blockquote>
</blockquote>
<p><strong>Jakarta EE</strong>
Jakarta EE(Java EE)는 기업용 애플리케이션에 필요한 기능들의 사양을 정의해둔 명세서이다. 즉, 대규모 애플리케이션을 개발하는데 필요한 표준화된 Java API의 모음이라고 할 수 있다. Java API의 특징은 API를 제공하는 구현 벤더와 분리되어 있기 때문에 API를 준수한다면 플러그 형태로 구현 벤더를 교체할 수 있다는 장점이 있다. 구체적으로는 JSP, Servlet, EJB, JMS, JMX, JTA 등 기업용 애플리케이션을 개발하고 실행하는 데 필요한 사양들을 명시하고 있다. 여기서 Jakarta EE(Java EE) API의 전체 목록을 확인할 수 있다. 다양한 써드파티에서 Jakarta EE(Java EE) 스펙을 준수한 구현을 제공하는데 대표적으로 아래와 같은 벤더가 있다. Jakarta EE(Java EE) 스펙을 완전히 구현한 것을 WAS(Web Application Server)라고 한다.</p>
<ul>
<li>JBoss(<a href="http://www.jboss.org">www.jboss.org</a>)</li>
<li>JOnAS(jonas.objectweb.org)</li>
<li>Geronimo(geronimo.apache.org)</li>
</ul>
<blockquote>
</blockquote>
<p><strong>Servlet</strong>
서블릿은 Jakarta EE API 중 하나로, Java 기술을 기반으로 한 웹 컴포넌트이다. Servlet은 서버 측에서 실행되는 Java 클래스로, 클라이언트의 요청을 처리하고 동적인 웹 페이지를 생성한다. Jakarta EE(Java EE)의 Servlet 주요 스펙을 요약하면 다음과 같다. (Servlet 3.1 Spec docs)</p>
<blockquote>
</blockquote>
<ul>
<li>요청/응답 모델: Servlet은 HTTP 요청을 받아 처리한 후, 응답을 반환하는 요청/응답 모델을 기반으로 한다.</li>
<li>생명주기 관리: Servlet은 특정 생명주기를 가진다. 주요 메소드로는 init(), service(), doGet(), doPost(), destroy() 등이 있다.</li>
<li>세션 관리: Servlet 스펙은 클라이언트 세션을 관리하기 위한 API를 제공한다.</li>
<li>컨텍스트 공유: ServletContext를 통해 애플리케이션 전체에 걸쳐 정보를 공유할 수 있다.</li>
<li>필터링: 필터를 사용하여 요청 및 응답에 대한 사전 및 사후 처리를 수행할 수 있다.</li>
<li>이벤트 리스너: 웹 애플리케이션의 생명주기 이벤트나 세션 생성/소멸 등의 이벤트에 대한 리스너를 정의할 수 있다.</li>
<li>멀티 스레딩: Servlet은 멀티 스레딩 환경에서 실행된다.
포워딩 및 리다이렉션: 요청을 다른 리소스로 포워딩하거나, 클라이언트에게 다른 URL로 리다이렉션 요청을 보낼 수 있다.</li>
<li>보안: Servlet 스펙은 선언적인 보안을 제공하여, 웹 리소스에 대한 접근 제어를 쉽게 구성할 수 있다.</li>
</ul>
<blockquote>
</blockquote>
<p><strong>Servlet Containner</strong>
웹 서버나 애플리케이션 서버의 일부로서, 네트워크 서비스를 제공한다. 웹 애플리케이션의 생명 주기를 관리하며, 요청을 받아 처리하고, 웹 페이지의 동적인 내용을 생성하는 역할을 한다. 클라이언트가 웹서버에 HTTP 요청을 보내면, 서블릿 컨테이너가 이 요청을 받아서 요청 URL에 해당하는 서블릿에 전달하며, 서블릿 쓰레드가 사용자의 요청을 처리한 후 서블릿 컨테이너를 통해 응답을 반환한다.</p>
<h2 id="2-톰캣tomcat">2. 톰캣(Tomcat)</h2>
<p>Tomcat은 Servlet 및 JSP 사양을 구현한 서블릿 컨테이너이다. JSP와 서블릿 처리, 서블릿의 수명 주기 관리, 요청 URL을 서블릿 코드로 매핑, HTTP 요청 수신 및 응답, 필터 체인 관리 등을 처리해준다. 톰캣은 Jakarta EE를 완전히 구현한 WAS는 아니고, JSP 와 Servlet 사양만 구현했기 때문에 Servlet Containner 또는 Web Container 라고 부른다.</p>
<h3 id="1-톰캣의-구조">1) 톰캣의 구조</h3>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/f96570d9-6de9-4c35-8aad-c4798ca86f5f/image.PNG" /></p>
<pre><code>    &lt;출처: https://www.youtube.com/watch?v=UlF6o3Wbi2k&gt;</code></pre><p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/6000ee07-1e56-4196-a789-4082cc3eae2b/image.png" /></p>
<pre><code>    &lt;출처: https://kbss27.github.io/2017/11/16/tomcatarchitecture/&gt;</code></pre><p>톰캣은 5개의 메인 구성요소와 부속요소로 이루어져 있다.</p>
<blockquote>
</blockquote>
<ol>
<li>Server</li>
<li>Service</li>
<li>Engine</li>
<li>Host</li>
<li>Connector</li>
<li>부속 요소</li>
</ol>
<h3 id="2-톰캣의-구성요소component">2) 톰캣의 구성요소(Component)</h3>
<pre><code>            톰캣 구성 관계도

Server
├── Listener (여러 개 존재 가능)
├── GlobalNamingResources
│   └── Resource (UserDatabase)
└── Service (Catalina)
    ├── Connector (port=8080, protocol=HTTP/1.1)
    └── Engine (Catalina)
        ├── Realm (LockOutRealm)
        │   └── Realm (UserDatabaseRealm)
        └── Host (localhost)
            └── Valve (AccessLogValve)</code></pre><blockquote>
</blockquote>
<ul>
<li>Server: 최상위 컴포넌트로 서버의 전반적인 설정을 관리한다.</li>
<li>Listener: 서버 라이프사이클 이벤트를 처리한다.</li>
<li>GlobalNamingResources: 글로벌 네이밍 리소스, UserDatabase 같은 리소스 설정이 있다.</li>
<li>Service: 하나 이상의 커넥터(Connector)와 하나의 엔진(Engine)을 가진다.</li>
<li>Connector: HTTP 요청을 처리한다.</li>
<li>Engine: 실제로 웹 애플리케이션을 처리하는 컴포넌트이다.</li>
<li>Realm: 보안 관련 설정을 담당한다.</li>
<li>Host: 가상 호스트 설정을 관리한다.</li>
<li>Valve: 로깅이나 보안 등 추가적인 처리를 담당한다.</li>
</ul>
<h4 id="1-server">1. Server</h4>
<blockquote>
</blockquote>
<p>톰캣 서버의 최상위 요소로 Tomcat 웹 애플리케이션 서버 자체를 의미한다. 톰캣 서버의 구성과 관리를 담당한다.</p>
<p>하나의 JVM 내에서 하나의 Server 인터턴스를 실행할 수 있다. 여러 개의 JVM 인스턴스를 실행한다면 여러 대의 Tomcat Server 인스턴스를 생성할 수 있는데 각각의 Server 인서턴스는 다른 네트워크 포트를 사용해야 충돌이 발생하지 않는다.</p>
<h4 id="2-service">2. Service</h4>
<blockquote>
</blockquote>
<p>Tomcat Service는 하나 이상의 Connector와 하나의 Engine을 그룹화하는 계층이다.
요청을처리하는 기능을 제공하는 톰캣 최상위 컴포넌트 중 하나로, connector로부터 요청을 수신하고 Engine(catalina)로 전달한다.</p>
<p>일반적으로 하나의 Engine 타입의 컨테이너 하나와 여러 개의 Connector로 구성된다. Service는 여러 웹 애플리케이션을 관리하고 클라이언트의 요청을 처리하는 단위이다. 클라이언트가 요청을 하면 Connector가 요청을 받아, Engine(container)에게 전달한다. Engine은 요청을 웹 애플리케이션으로 라우팅하고 결과를 반환한다. 각 Service는 이름이 부여되어 있어 로그 메시지를 통해 Service를 쉽게 식별할 수 있다.</p>
<p>service가 여러개 인 경우는 같은 ip에 여러개의 포트번호로 다중 서비스(웹 서비스)을 제공할 때이다. 즉, 하나의 톰캣 서버에 여러 개의 웹 어플리케이션을 포트로 아예 구분할 때 이런 구성을 가진다.
아래의 host인 주소만 변경하는게 아니다.</p>
<pre><code>&lt;Service name=&quot;Catalina&quot;&gt;
    &lt;Connector port=&quot;8082&quot; protocol=&quot;HTTP/1.1&quot;
               connectionTimeout=&quot;20000&quot;
               redirectPort=&quot;8443&quot; /&gt;

    &lt;Engine name=&quot;Catalina&quot; defaultHost=&quot;localhost&quot;&gt;

      &lt;Realm className=&quot;org.apache.catalina.realm.LockOutRealm&quot;&gt;
        &lt;Realm className=&quot;org.apache.catalina.realm.UserDatabaseRealm&quot;
               resourceName=&quot;UserDatabase&quot;/&gt;
      &lt;/Realm&gt;

      &lt;Host name=&quot;localhost&quot;  appBase=&quot;webapps&quot;
            unpackWARs=&quot;true&quot; autoDeploy=&quot;true&quot;&gt;

        &lt;Valve className=&quot;org.apache.catalina.valves.AccessLogValve&quot; directory=&quot;logs&quot;
               prefix=&quot;localhost_access_log&quot; suffix=&quot;.txt&quot;
               pattern=&quot;%h %l %u %t &amp;quot;%r&amp;quot; %s %b&quot; /&gt;

      &lt;/Host&gt;
    &lt;/Engine&gt;
  &lt;/Service&gt;



  &lt;Service name=&quot;Catalina2&quot;&gt;
// 바뀐 곳
    &lt;Connector port=&quot;8083&quot; protocol=&quot;HTTP/1.1&quot;
               connectionTimeout=&quot;20000&quot;
               redirectPort=&quot;8443&quot; /&gt;

    &lt;Engine name=&quot;Catalina&quot; defaultHost=&quot;localhost&quot;&gt;

      &lt;Realm className=&quot;org.apache.catalina.realm.LockOutRealm&quot;&gt;
        &lt;Realm className=&quot;org.apache.catalina.realm.UserDatabaseRealm&quot;
               resourceName=&quot;UserDatabase&quot;/&gt;
      &lt;/Realm&gt;
// 바뀐 곳
      &lt;Host name=&quot;localhost&quot;  appBase=&quot;webapps2&quot;
            unpackWARs=&quot;true&quot; autoDeploy=&quot;true&quot;&gt;

        &lt;Valve className=&quot;org.apache.catalina.valves.AccessLogValve&quot; directory=&quot;logs&quot;
               prefix=&quot;localhost_access_log&quot; suffix=&quot;.txt&quot;
               pattern=&quot;%h %l %u %t &amp;quot;%r&amp;quot; %s %b&quot; /&gt;

      &lt;/Host&gt;
    &lt;/Engine&gt;
  &lt;/Service&gt;</code></pre><h4 id="3-connectors">3. Connectors</h4>
<blockquote>
</blockquote>
<p>Connectors는 클라이언트와 통신을 담당한다. 예를들어, coyote로 부터 받은 Request 객체를 catalina(서블릿 컨테이너)에 전달하고 catalina(서블릿 컨테이너)가 처리한 응답을 다시 coyote로 전달하는 역할을 한다.</p>
<p>Connectors는 클라이언트의 웹 요청을 받아서 적절한 애플리케이션으로 전달하는 역할을 하는 컴포넌트이다. 이를 통해 HTTP, HTTPS 등 다양한 프로토콜과 포트를 사용하여 웹 애플리케이션에 접근할 수 있게 한다. 하나의 Engine은 여러 개의 Connectors를 설정할 수 있는데, 각 Connectors의 포트는 유일해야 한다. 기본 Connector는 Coyote이며 HTTP 1.1을 구현한다. 그 외에도 AJP, SSL Connector, HTTP 2.0 Connector 등이 있다.</p>
<blockquote>
<p><strong>Coyote란</strong>
톰캣의 Coyote는 HTTP 1.1과 HTTP 2.0 프로토콜을 지원하는 Connector 컴포넌트이다.</p>
</blockquote>
<p>Connector 컴포넌트이기 때문에 당연히 서버로 들어오는 연결에 대해 요청을 수신하고 Engine에게 요청을 전달한다.</p>
<blockquote>
</blockquote>
<p>Coyote는 또한 정적 콘텐츠를 직접 처리할 수도 있는데, Coyote 덕분에 Tomcat은 Apache HTTP Server와 같은 별도의 웹 서버 없이도 웹 요청을 직접 처리할 수 있다.</p>
<blockquote>
</blockquote>
<p>즉, Catalina가 처리하는 서블릿/JSP 동적 콘텐츠뿐만 아니라 Coyote의 처리 덕분에 정적 콘텐츠도 직접 제공할 수 있어 톰캣이 완전한 웹 서버 역할을 수행할 수 있다.</p>
<h4 id="4-engine">4. Engine</h4>
<p>Engine은 Tomcat 서버에서 가장 상위에 위치한 컨테이너이다. Catalina 서블릿 엔진을 대표하고, HTTP 헤더를 분석하여 요청을 어느 가상 호스트나 컨텍스트로 전달할지 결정한다. 하나의 Engine 내에는 여러 Hosts가 포함될 수 있고, 각 Host는 여러 웹 애플리케이션을 나타낼 수 있다. 또한 Context는 단일 웹 애플리케이션을 나타낸다.
기본 Tomcat 구성에는 localhost 호스트를 포함하는 Catalina 엔진이 포함됩니다</p>
<h4 id="5-host">5. Host</h4>
<blockquote>
</blockquote>
<p>Host는 Tomcat에서 특정 도메인 이름에 대한 요청을 처리하는 가상 호스트를 나타낸다.</p>
<p>Host는 아파치 웹 서버의 가상 호스트와 유사하다. 하나의 물리적 서버에 여러 개의 웹 사이트를 호스팅할 수 있다. Host는 보통 Engine 내에 위치해 Engine의 요청을 받아 해당 요청이 어느 Host로 전달될지 판단한다.
서로 다른 도메인을 같은 서버에서 호스팅하면서도, 각각을 별도의 웹사이트처럼 운영할 수 있다는 장점이 있다.</p>
<pre><code>- 가상 호스트가 뭐지?

가상호스트 (Virtual Host)는 한 컴퓨터에서 여러 웹사이트를 (예를 들어, www.company1.com과 www.company2.com) 서비스함을 뜻한다.

가상호스트에는 &quot;IP기반 (IP-based)&quot; 방식과 &quot;이름기반 (name-based)&quot; 방식이 있다. 가상호스트 때문에 여러 사이트들이 같은 서버에서 돌고있다는 사실을 웹사용자는 눈치채지 못한다.</code></pre><p>Host가 여러개인 경우는 위의 서비스를 분리하는 것처럼 여러 개의 서비스를 제공하기 위해서와 동일하지만 ip와 포트번호가 같고 도메인 주소만 다르게 해서 각 다른 서비스를 제공하기 위해 사용된다.</p>
<pre><code>&lt;Service name=&quot;Catalina&quot;&gt;
   &lt;Connector port=&quot;8080&quot; protocol=&quot;HTTP/1.1&quot;
               connectionTimeout=&quot;20000&quot;
               URIEncoding=&quot;UTF-8&quot;
               redirectPort=&quot;8443&quot; /&gt;

   &lt;Connector port=&quot;8009&quot; protocol=&quot;AJP/1.3&quot; redirectPort=&quot;8443&quot; /&gt;

     &lt;Engine name=&quot;Catalina&quot; defaultHost=&quot;localhost&quot;&gt;

     &lt;Realm className=&quot;org.apache.catalina.realm.LockOutRealm&quot;&gt;
       &lt;Realm className=&quot;org.apache.catalina.realm.UserDatabaseRealm&quot;
              resourceName=&quot;UserDatabase&quot;/&gt;
     &lt;/Realm&gt;

     &lt;Host name=&quot;test.domain.com&quot;  appBase=&quot;webapps&quot;
           unpackWARs=&quot;true&quot; autoDeploy=&quot;true&quot;&gt;
       &lt;Valve className=&quot;org.apache.catalina.valves.AccessLogValve&quot; directory=&quot;logs&quot;
              prefix=&quot;localhost_access_log.&quot; suffix=&quot;.txt&quot;
              pattern=&quot;%h %l %u %t &amp;quot;%r&amp;quot; %s %b&quot; /&gt;
     &lt;/Host&gt;

// 바뀐 곳 - host 주소 추가 , appBase가 다르다.
     &lt;Host name=&quot;test2.domain.com&quot;  appBase=&quot;webapps2&quot;
           unpackWARs=&quot;true&quot; autoDeploy=&quot;true&quot;&gt;
       &lt;Valve className=&quot;org.apache.catalina.valves.AccessLogValve&quot; directory=&quot;logs&quot;
              prefix=&quot;localhost_access_log.&quot; suffix=&quot;.txt&quot;
              pattern=&quot;%h %l %u %t &amp;quot;%r&amp;quot; %s %b&quot; /&gt;
     &lt;/Host&gt;

   &lt;/Engine&gt;
 &lt;/Service&gt;</code></pre><h4 id="6-context">6. Context</h4>
<p>Context는 웹 애플리케이션을 나타낸다. Engine이나 Host에게 애플리케이션의 루트 폴더 위치를 알려주는 등의 설정을 포함한다. 여러 Context가 하나의 호스트를 공유할 수 있다.
예시로 스프링으로 개발한 프로젝트가 여기에 해당한다고 생각하면 된다.
스프링에서는 Servlet으로 웹 요청을 처리하는데 위의 과정을 거쳐서 요청을 받고 응답을 주는 것이라 생각하면 된다.
Servlet은 Engine 계층인 catalina(서블릿 컨테이너)에 의해 생성되고 관리된다. 
즉, Context는 서블릿과 JSP 파일 등을 포함한다.</p>
<h4 id="7-부속-요소">7. 부속 요소</h4>
<p><strong>1) Realm</strong>
Realm은 사용자 인증과 권한을 부여하는 컴포넌트이다. Realm은 전체 엔진에서 공통적으로 적용되므로 엔진 내의 애플리케이션은 인증을 위한 Realm을 공유하게 된다.</p>
<p><strong>2) Valves</strong>
Valves 요청과 응답을 가로채서 사전 처리를 할 수 있다. 이는 Servlet의 필터와 비슷하지만, Valves 톰캣 고유의 컴포넌트이다. Hosts, contexts, Engine에 Valves를 포함시킬 수 있다. Valves는 엔진과 컨텍스트 사이, 호스트와 컨텍스트 사이, 컨텍스트와 웹 리로스 사이에서 요청을 가로챌 수 있다. Request 헤더 및 쿠키 저장, Response 헤더 및 쿠키 등을 로깅 등에 사용된다.</p>
<h3 id="3-톰캣의-동작-과정">3) 톰캣의 동작 과정</h3>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/dfb72a20-277a-43e4-b522-f1973a58f8e3/image.png" /></p>
<ol>
<li>클라이언트 요청: 브라우저에서 HTTP 요청을 보냅니다.</li>
<li>Coyote 커넥터: 요청을 수신하여 Catalina 엔진에 전달합니다.</li>
<li>Catalina 엔진: 요청을 분석하고 적절한 Host 및 Context를 찾습니다.</li>
<li>Context: 서블릿 또는 JSP 파일로 요청을 전달하여 처리합니다.</li>
<li>Jasper (JSP의 경우): JSP 파일을 서블릿 코드로 변환 및 컴파일 후 실행합니다.</li>
<li>응답: 서블릿이 생성한 응답이 Coyote를 통해 클라이언트에 반환됩니다.</li>
</ol>
<blockquote>
</blockquote>
<p>즉, 클라이언트 HTTP request &gt; Catalina &gt; Context &gt; Servlet &gt; 클라이언트 response 순으로 처리됩니다.</p>
<h3 id="4-톰캣의-환경설정">4) 톰캣의 환경설정</h3>
<p>아래는 톰캣 server.xml 파일의 간단한 구성 예제입니다.</p>
<pre><code>&lt;Server port=&quot;8005&quot; shutdown=&quot;SHUTDOWN&quot;&gt;
    &lt;Service name=&quot;Catalina&quot;&gt;
        &lt;Connector port=&quot;8080&quot; protocol=&quot;HTTP/1.1&quot; 
                   connectionTimeout=&quot;20000&quot; 
                   redirectPort=&quot;8443&quot; /&gt;

        &lt;Engine name=&quot;Catalina&quot; defaultHost=&quot;localhost&quot;&gt;
            &lt;Host name=&quot;localhost&quot; appBase=&quot;webapps&quot; 
                  unpackWARs=&quot;true&quot; autoDeploy=&quot;true&quot;&gt;
                &lt;!-- Context 설정 --&gt;
                &lt;Context path=&quot;/myApp&quot; docBase=&quot;myApp&quot; reloadable=&quot;true&quot;/&gt;
            &lt;/Host&gt;
        &lt;/Engine&gt;
    &lt;/Service&gt;
&lt;/Server&gt;
</code></pre><p>이 구성은 톰캣 서버의 기본 포트를 8080으로 설정하고, myApp이라는 컨텍스트를 통해 애플리케이션을 제공합니다.</p>
<h2 id="3-톰캣-디렉토리-구조">3. 톰캣 디렉토리 구조</h2>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/09e6c282-53c2-4809-b10b-a4e68985b572/image.png" /></p>
<ul>
<li><p>bin: 톰캣 실행과 종료에 관한 스크립트 파일이 위치합니다.</p>
</li>
<li><p>conf: server.xml 와 같은 서버 전체 설정에 관한 설정 파일이 위치합니다. 이 중 server.xml은 서버 설정과 관련된 접근/접속에 관한 내용들이 담겨있습니다. 설정되어 있는 포트와 충돌 나는 일을 방지하기 위해 필요에 따라 포트를 변경할 수 있습니다. 더해서 webapps 폴더가 아닌 다른 경로로 변경하기 위해서는  부분에 프로젝트 경로를 입력해 주면 됩니다. web.xml은 톰캣의 환경설정 파일이며 서블릿, 필터, 인코딩을 설정할 수 있으며, 서버가 올라갈 때 가장 먼저 읽는 파일입니다.</p>
</li>
<li><p>lib: 톰캣과 다른 웹 서버를 연결해 주는 바이너리 모듈들과 톰캣 구동시 필요한 jar 라이브러리들이 위치합니다.</p>
</li>
<li><p>logs: 예외 발생 사항 등 톰캣 로그 파일들이 위치합니다.</p>
</li>
<li><p>temp: 톰캣이 실행되는 동안 임시파일들이 위치하는 임시 저장용 폴더입니다.</p>
</li>
<li><p>webapps: 웹 애플리케이션이 위치합니다. 스프링 프로젝트 같은 것을 넣는 곳입니다.</p>
</li>
<li><p>work: JSP파일을 서블릿 상태로 변환한 java파일과 class 파일들이 저장되는 위치입니다.</p>
</li>
</ul>
<h2 id="4-jasper">4. Jasper</h2>
<p>Jasper는 톰캣의 JSP 엔진으로, JSP 파일을 서블릿 코드로 변환하고 컴파일합니다. 클라이언트 요청 시, JSP 파일이 처음 호출되면 Jasper가 이 파일을 서블릿으로 변환하여 실행합니다. 이후에는 서블릿이 메모리에 남아 있다가 재사용됩니다.</p>
<hr />
<h2 id="5-스프링-부트에서-내장-톰캣embedded-tomcat이-실행되는-과정">5. 스프링 부트에서 내장 톰캣(Embedded Tomcat)이 실행되는 과정</h2>
<ol>
<li>맨 처음 SpringApplication.run이 실행
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/3137467f-172d-4f84-a687-ce6c7cbc1af3/image.PNG" /></li>
</ol>
<ol start="2">
<li><p>run메서드에서 createApplicationContext 메서드가 실행되어
내부 웹서버 설정하고 구동하는 ServletWebServerApplicationContext를 생성한다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/d014dd29-06a7-4526-acc8-c3b4c7f34279/image.PNG" /></p>
</li>
<li><p>ServletWebServerApplicationContext의 createWebServer 메서드를 실행하여 TomcatServletWebServerFactory를 생성한다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/6b6ce7a9-ebac-4af9-84fa-7ffd3651b5b8/image.PNG" /></p>
</li>
<li><p>TomcatServletWebServerFactory의 getWebServer 메서드를 실행하면 톰캣이 생성한다. 이 인스턴스는 LifecycleListener및 connector 역할을 한다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/62472cf0-ae31-4333-b3de-47003e988f88/image.PNG" /></p>
</li>
<li><p>위의 톰캣 인스턴스를 실행하고 종료하기 위해 TomcatWebServer를 생성하고 위의 톰캣인스턴스를 파라미터로 넣어준다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/5745aa47-9999-42a8-b8a5-16b7a9c3ec50/image.PNG" /></p>
</li>
<li><p>그러면 이제 톰캣이 실행된다.</p>
</li>
</ol>
<hr />
<blockquote>
</blockquote>
<p><strong>참고</strong></p>
<ol>
<li><a href="https://tomcat.apache.org/tomcat-10.1-doc/architecture/overview.html">https://tomcat.apache.org/tomcat-10.1-doc/architecture/overview.html</a></li>
<li><a href="https://mincanit.tistory.com/105">https://mincanit.tistory.com/105</a></li>
<li><a href="https://velog.io/@hyunjae-lee/Tomcat-2-%EA%B5%AC%EC%A1%B0">https://velog.io/@hyunjae-lee/Tomcat-2-%EA%B5%AC%EC%A1%B0</a></li>
<li><a href="https://www.youtube.com/watch?v=UlF6o3Wbi2k">https://www.youtube.com/watch?v=UlF6o3Wbi2k</a></li>
<li><a href="https://px201226.github.io/tomcat/">https://px201226.github.io/tomcat/</a></li>
<li><a href="https://medium.com/@potato013068/%ED%86%B0%EC%BA%A3%EC%9D%98-%EA%B5%AC%EC%A1%B0%EC%99%80-%EB%8F%99%EC%9E%91-%EB%A9%94%EC%BB%A4%EB%8B%88%EC%A6%98-91fbebf0eb67">https://medium.com/@potato013068/%ED%86%B0%EC%BA%A3%EC%9D%98-%EA%B5%AC%EC%A1%B0%EC%99%80-%EB%8F%99%EC%9E%91-%EB%A9%94%EC%BB%A4%EB%8B%88%EC%A6%98-91fbebf0eb67</a></li>
</ol>