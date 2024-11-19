<p>스프링 부트는 웹서버 어플리케이션 구조에 대해 잘 몰라도, 뚝딱 웹서버 어플리케이션을 만들도록 도와준다. 하지만 한번씩 궁금증이 들 때가 있다.</p>
<blockquote>
</blockquote>
<ul>
<li>Controller는 한 개인가?
스프링은 기본적으로 singleton bean을 생성하는데 contoller도 분명 singleton일거 같은데...</li>
<li>그럼 singleton으로 생성된 contoller를 여러 쓰레드가 공유한다면 동기화 문제는??</li>
<li>Contoller가 한 개라면 10만 개의 Request가 있다면 Controller 1개가 전부다 처리하는건가?</li>
</ul>
<p>그래서 위 질문을 해결하기 위해  내장 톰캣, 스레드 풀, JVM 메모리 관점에서 파악해보고자 한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/5d6a3e4a-10ac-438a-add3-a8522a2e3c58/image.png" /></p>
<p>위 그림은 스프링부트의 flow를 찾아보면 흔히 나오는 MVC 흐름도 그림이다.</p>
<p>&quot;한 유저의 요청이 어떻게 처리되는지&quot; 에 대한 내용은 많지만 &quot;여러 유저의 요청이 어떻게 처리되는지&quot; 에 대한 내용은 찾아보기 힘들었던거 같다.</p>
<p>추측컨데, MVC는 스프링부트를 사용하는 개발자가 직접 구현해야하는 부분이고, 다중요청처리는 스프링부트가 알아서 해주고 있는 부분이니 상대적으로 관심도가 덜한 것이라 여겨진다.</p>
<p>사실 스프링부트가 다중요청을 처리하는 것이 아니라, 스프링부트에 내장되어있는 서블릿 컨테이너(Tomcat)에서 다중요청을 처리해준다. 핵심 키워드는 Tomcat Thread Pool, NIO Connector, Embeded Tomcat 이다.</p>
<p><strong>결론부터 보자면 다음과 같다.</strong></p>
<blockquote>
</blockquote>
<p>Bean들은 기본적으로 Singleton으로 생성되고 관리된다
스프링부트는 내장 서블릿 컨테이너인 Tomcat을 이용한다.
Tomcat은 다중 요청을 처리하기 위해서, 부팅할 때 스레드의 컬렉션인 Thread Pool을 생성한다.
유저 요청(HttpServletRequest)가 들어오면 Thread Pool에서 하나씩 Thread를 할당한다. 해당 Thread에서 스프링부트에서 작성한 Dispatcher Servlet을 거쳐 유저 요청을 처리한다.
작업을 모두 수행하고 나면 스레드는 스레드풀로 반환된다.</p>
<h2 id="controller-1개는-어떻게-여러-개의-요청을-처리하지">Controller 1개는 어떻게 여러 개의 요청을 처리하지?</h2>
<hr />
<p>Request 별로 Thread가 따로 생성되고, 이에 따라 각각의 ServletContext를 갖는데 어떻게 Controller가 1개만 생성되는 것일까 ??</p>
<h3 id="singleton인-controller">Singleton인 Controller</h3>
<p>각각의 Thread는 singleton으로 생성된 Bean( ⇒ Controller 포함 ) 들을 참고하여 일을 한다.
스프링에서 Bean들은 기본적으로 Singleton으로 생성되고 관리된다.
사실상 이 Thread들은 단 1개의 Singleton Controller 객체를 공유하기에 최종적으로 1개의 Controller만 사용하는 것이다.</p>
<p>즉, 하나의 Singleton Controller가 수많은 request를 처리한다기 보다는, 각각의 Thread가 singleton으로 생성된 Controller객체의 정보들를 참고하여 실행한다고 보면 된다.</p>
<p>이는 JAVA 의 메모리 구조와 관련이 있다.</p>
<h3 id="controller가-저장되는-곳">Controller가 저장되는 곳</h3>
<p>우선 Controller 객체 하나를 생성하면 객체 자체는 Heap에 생성되지만, 해당 Class의 정보(메소드 처리 로직, 명령들)는 Method Area(메서드 영역)에 저장된다.
메소드 영역으로는 모든 Thread가 접근 가능하기 때문에 객체의 Binary Code 정보를 공유할 수 있다.</p>
<p>자바의 메모리 구조
 <img alt="" src="https://velog.velcdn.com/images/take_the_king/post/4848e333-18c5-4d9a-b607-5bf35de70000/image.png" /></p>
<p><strong>&quot;공유된다&quot; 해서 동기화 될 필요가 없다.</strong></p>
<ul>
<li><p>공유되는 정보를 사용하기 위하여 굳이 Controller 객체를 사용하고 있는 Thread나 Controller 객체 자체가 동기화 될 필요가 없다.</p>
</li>
<li><p>원래 동기화를 해주는 이유는 프로세스(Thread)들간 알고있는 정보(상태)를 일치하기 위해서인데, Controller가 내부적으로 상태를 갖는 것이 없으니, 그냥 메소드 호출만 하면 된다. → 공유하는 데이터 즉, 클래스변수, 전역변수를 컨트롤러에서 사용하지 않기 때문에 상태를 갖는 것이 없음</p>
</li>
</ul>
<ul>
<li><p>그로 인해 굳이 동기화할 이유도 없고 그저 처리 로직만 ‘공유되어’ 사용되는 것이다.</p>
</li>
<li><p>따라서 스레드는 Controller의 메소드를 공유하고 제각각 호출할 수 있기 때문에 들어오는 요청이 1만 개의 요청이든 10만 개의 요청이든 상관없게 된다.</p>
</li>
</ul>
<h3 id="만약-상태-정보를-갖게-된다면">만약 상태 정보를 갖게 된다면?</h3>
<ul>
<li><p>결론적으로 싱글톤으로 관리되는 Bean의 경우 상태 정보를 갖지 않기 때문에 요청의 수와 상관없이 싱글턴 객체의 장점을 이용할 수 있었던 것이다.</p>
</li>
<li><p>만약 Bean이 상태 정보를 갖게된다면 스레드들간의 동기화가 필요하고 그렇게 된다면 오버헤드가 발생하게 된다. 이는 Controller 객체를 하나만 만들고 컨테이너에서 단순히 꺼내쓸 수 있었던 장점을 잃게 된다.</p>
</li>
</ul>
<h2 id="스프링부트와-내장-톰캣">스프링부트와 내장 톰캣</h2>
<p>스프링과 스프링부트의 주요한 차이점 중 하나는, 스프링 부트에선 내장 서블릿 컨테이너(Tomcat)을 지원한다는 것이다.</p>
<p>그럼으로써 application.yml 혹은 application.properties (Spring Environment)에 설정을 주는 것만으로 간편하게 Tomcat의 설정을 바꾸어줄 수 있다.</p>
<p>다음은 서버의 설정을 바꾸어주는 예시이다.</p>
<pre><code># application.yml (적어놓은 값은 default)
server:
  tomcat:
    threads:
      max: 200 # 생성할 수 있는 thread의 총 개수
      min-spare: 10 # 항상 활성화 되어있는(idle) thread의 개수
    max-connections: 8192 # 수립가능한 connection의 총 개수
    accept-count: 100 # 작업큐의 사이즈
    connection-timeout: 20000 # timeout 판단 기준 시간, 20초
  port: 8080 # 서버를 띄울 포트번호</code></pre><p>이 밖에도 많은 옵션을 과거 xml방식의 복잡한 설정에서 벗어나 간단하게 옵션을 변경할 수 있다. 만약 설정을 주지 않는다면, SpringBoot AutoConfiguration에서 정의한 디폴트값을 주입하게 된다.</p>
<blockquote>
<p>해당 디폴트 값은 org.springframework.boot.autoconfigure.web.ServerProperties 클래스에서 확인할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/e58e52a1-07e4-49f3-802a-868a0ba2e86e/image.png" /></p>
</blockquote>
<h2 id="스레드풀thread-pool-설정">스레드풀(Thread Pool) 설정</h2>
<p>위에 예시로 들어둔 yml파일에 적어놓은 값들이, Tomcat ThreadPoolExcutor와 Connector에 줄 수 있는 옵션들이다.</p>
<p>우선 Thread Pool이 무엇인지 알아보도록 하자.</p>
<h3 id="스레드풀이란">스레드풀이란</h3>
<p>Thread Pool은 프로그램 실행에 필요한 Thread들을 미리 생성해놓는다는 개념이다.(Thread는 cpu의 자원을 이용하여 코드를 실행하는 하나의 단위 이다.)</p>
<p>Tomcat 3.2 이전 버전에서는, 유저의 요청이 들어올 때 마다 Servlet을 실행할 Thread를 하나씩 생성했고 요청이 끝나면 destory했다. 자세히 알지 못하더라도 이 방침은 문제를 야기시킬 것이라고 예상할 수 있을 것이다.</p>
<p>모든 요청에 대해 스레드를 생성하고 소멸하는 것은 OS와 JVM에 대해 많은 부담을 안겨준다. 동시에 일정 이상의 다수 요청이 들어올 경우 리소스(CPU와 메모리 자원) 소모에 대한 억제가 어렵다. 즉 순간적으로 서버가 다운되거나 동시다발적인 요청을 처리하지 못해서 생기는 문제가 야기될 수 있다.</p>
<p>해당 문제를 해결하기 위해, 톰캣은 스레드풀을 활용하기 시작한다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/5d94b95a-9bde-4fc7-8df2-71ca40233a1c/image.png" /></p>
<p>스레드풀의 기본 플로우는 다음과 같다.</p>
<ol>
<li>첫 작업이 들어오면, core size만큼의 스레드를 생성한다.</li>
<li>유저 요청(Connection, Server socket에서 accept한 소캣 객체)이 들어올 때마다 작업 큐(queue)에 담아둔다.</li>
<li>core size의 스레드 중, 유휴상태(idle)인 스레드가 있다면 작업 큐에서 작업을 꺼내 스레드에 작업을 할당하여 작업을 처리한다.
1) 만약 유휴상태인 스레드가 없다면, 작업은 작업 큐에서 대기한다.
2) 그 상태가 지속되어 작업 큐가 꽉 찬다면, 스레드를 새로 생성한다.
3) 3번과정을 반복하다 스레드 최대 사이즈 에 도달하고 작업큐도 꽉 차게 되면, 추가 요청에 대해선 connection-refused 오류를 반환한다.</li>
<li>태스크가 완료되면 스레드는 다시 유휴상태로 돌아간다.
1) 작업큐가 비어있고 core size이상의 스레드가 생성되어있다면 스레드를 destory한다.</li>
</ol>
<p><strong>즉, 스레드를 미리 만들어놓고 필요한 작업에 할당했다가 돌려 받는다.</strong></p>
<blockquote>
</blockquote>
<p>※ 참고
Connection, Server socket에서 accept한 소캣 객체가 이해 안 된다면 자바 소켓 프로그래밍 로 검색.
구체적인 구현 보다 개념적으로 서버와 클라이언트가 어떻게 연결되는지에 대해 살펴보면 된다.</p>
<p>그러므로 스레드는 많으면 너무 많은 스레드가 cpu의 자원을 두고 경합하게 되므로 처리속도가 느려질 수 있고, 적으면 cpu자원을 최적으로 활용하지 못하여 마찬가지로 처리속도가 느려질 수 있다.</p>
<p>스레드는 적절한 수로 유지되는 것이 가장 좋다. 또한 스레드 풀은 최대한 core size를 유지하려고 한다.</p>
<blockquote>
</blockquote>
<p>※ 참고
이와 관련해 어떤 전략이 있는지는, 스레드풀 전략 으로 검색. 적절한 스레드의 개수로는 적정 스레드 개수로 검색</p>
<h3 id="스레드풀-생성-threadpoolexecutor">스레드풀 생성 (ThreadPoolExecutor)</h3>
<p>위에 설명한 스레드풀을 자바에서 구현한 구현체가 ThreadPoolExecutor이다. 앞서 application.yml에서 주었던 설정 중 일부를 살펴보자.</p>
<pre><code>server:
  tomcat:
    threads:
      max: 200 # 생성할 수 있는 thread의 총 개수
      min-spare: 10 # 항상 활성화 되어있는(idle) thread의 개수
    accept-count: 100 # 작업 큐의 사이즈</code></pre><p>이 두가지 설정은 스레드 최대 사이즈 및 core size 를 변경할 수 있도록 해준다. 톰캣 9.0의 디폴트 옵션은 각각 200개, 25개 인데 스프링부트(ServerProperties)에선 200개, 10개를 디폴트 값으로 잡았다.</p>
<p>accept-count는 작업큐의 사이즈이다. 스프링 부트에선 아무 옵션을 안주면 Integer.MAX, 즉 대략 21억를 할당한다. 이는 무한 대기열 전략 으로, 아무리 요청이 많이 들어와도 core size를 늘리지 않는다는 정책이다. 무한 대기열 전략에선 작업큐가 꽉 찰 일이 없으므로, 스레드풀의 max사이즈가 의미가 없다.</p>
<p>그럼 설정을 주어 max값을 변경하는 건 아무 의미 없을까? 뒤에서 다시 살펴보도록 하자.</p>
<p>아래는 ThreadPoolExecutor의 생성자에 브레이크 포인트를 찍어놓고 디버그 모드로 실행한 결과다. SpringBoot 실행 시, 톰캣을 initalize하고, ThreadPoolExecutor을 상속한 ScheduledThreadPoolExecutor를 생성함을 볼 수 있다. 이 때 Environment에 설정한 값이 있다면 해당 값이 주입되고, 없으면 기본값이 주입된다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/64e64255-fea9-4c9f-b149-1ee4daa982c8/image.png" /></p>
<h3 id="스레드풀-테스트">스레드풀 테스트</h3>
<p>지금까지 알아본 바에 의하면, 유저 요청이 들어올 때(Connection)마다 스레드가 하나씩 할당될 것이고, 작업큐가 가득차면 스레드가 늘어날 것이고, 스레드도 가득 차면 유저 요청이 거절될 것이다. </p>
<p>정말 그런지 간단한 실험을 하나 해보자.</p>
<p>스프링 프로젝트를 하나 만들고, application.yml에 다음과 같이 옵션을 주었다.</p>
<pre><code>server:
  tomcat:
    threads:
      max: 2
      min-spare: 2
    accept-count: 1
  port: 5000</code></pre><p>그 후 3초를 대기하는 api를 하나 만들었다.</p>
<pre><code>@Slf4j
@Controller
public class HelloController {

    @RequestMapping(&quot;/hello&quot;)
    public ResponseEntity&lt;Void&gt; hello() throws InterruptedException {
        log.info(&quot;start&quot;);
        Thread.sleep(3000);
        log.info(&quot;end&quot;);
        return ResponseEntity.ok().build();
    }
}</code></pre><p>이 프로젝트가 감당할 수 있는 요청은 동원할 수 있는 스레드 2개, 그리고 작업큐 1개에서 대기할 요청까지 최대 3개다. 이를 확인하기 위해 또 다른 스프링 프로젝트를 만들어, 요청을 5번 보내보자.</p>
<pre><code>@Slf4j
@Controller
public class PowerController {

    @GetMapping(&quot;/&quot;)
    @ResponseBody
    public ResponseEntity&lt;Void&gt; request5Threads(){
        RestTemplate restTemplate = new RestTemplate();
        for (int i = 0; i &lt; 5; i++) {
            Thread thread = new Thread(() -&gt; {
                log.info(&quot;발사!&quot;);
                String result = restTemplate
                    .getForObject(&quot;http://localhost:5000/hello&quot;, String.class);
                log.info(result);
            });
            thread.start();
        }

        return ResponseEntity.ok().build();
    }
}</code></pre><p>해당 코드는 밀리세컨드 단위로 5번의 요청을 한번에 보내게 된다. 2개의 요청이 두개의 활성 스레드에서 각각 3초동안 block되고, 3번째 요청은 작업 큐에서 대기할 것이므로 4, 5번째 요청은 거절되어야 한다.</p>
<p>즉, 예상을 해보자면 4, 5번째 요청은 거절당하고 에러가 발생해야 할것이다.</p>
<p><strong>결과</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/98c479cb-46d5-4907-9dfc-88d6d8493015/image.png" /></p>
<p>요청은 동시에 갔지만, 응답은 3초단위로 텀을 두고 순차적으로 처리되는 모습을 볼 수 있다. 서버측도 확인해보자.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/858c9960-fbeb-4d07-8cea-24b3b3217106/image.png" /></p>
<p>2개의 활성 스레드가 차근차근, 3초 간격으로 작업을 처리한 걸 볼 수 있다. 작업큐는 1칸 이므로 두 개의 4,5번 째 요청은 받을 수 없었을텐데 정상 처리되고 있다?? 어떻게 이게 가능한 것일까??</p>
<h2 id="bio-connector와-nio-connector">BIO Connector와 NIO Connector</h2>
<hr />
<p>비밀은 Connector에 있다. 위의 이야기는 BIO(Blocking I/O) Connector일 때 유효한 이야기다. 그러나 톰캣 8.5부터 NIO(NonBlocking I/O) Connector이 기본으로 채택되고, 9.0부터는 BIO Connector이 deprecate 됨 으로써 위의 설명과는 다른 방식으로 진행하게 된다. 하나씩 알아보자.</p>
<h3 id="connector">Connector</h3>
<p>Connector는 소켓 연결을 수입하고 데이터 패킷을 획득하여 HttpServletRequest 객체로 변환하고, Servlet 객체에 전달하는 역할을 한다.</p>
<p>Acceptor에서 while문으로 대기하며 port listen을 통해 Socket Connection을 얻게 된다.
Socket Connection으로부터 데이터를 획득한다. 데이터 패킷을 파싱해서 HttpServletRequest 객체를 생성한다.
Servlet Container 에 해당 요청객체를 전달한다. ServletContainer는 알맞은 서블릿을 찾아 요청을 처리한다.</p>
<h3 id="bio-connector">BIO Connector</h3>
<p>BIO Connector는 Socket Connection을 처리할 때 Java의 기본적인 I/O 기술을 사용한다. thread pool에 의해 관리되는 thread는 소켓 연결을 받고 요청을 처리하고 요청에 대해 응답한 후 소켓 연결이 종료되면 pool에 다시 돌아오게 된다.</p>
<p>즉, conneciton이 닫힐 때까지 하나의 thread는 특정 connection에 계속 할당되어 있는 것이다.</p>
<p>이러한 방식으로 Thread 를 할당하여 사용할 경우, 동시에 사용되는 thread 수가 동시 접속할 수 있는 사용자의 수가 될 것이다. 그리고 이러한 방식을 채택해서 사용할 경우 thread들이 충분히 사용되지 않고 idle(아무것도하지않는) 상태로 낭비되는 시간이 많이 발생한다. 이러한 문제점을 해결하고 리소스(thread)를 효율적으로 사용하기 위해 NIO Connector가 등장했다.</p>
<h3 id="nio-connector">NIO Connector</h3>
<p>NIO Connector는 I/O가 아니라 Http11NioProtocol을 사용하는데, 해당 내용에 대해 이해하기 위해선 NIO에 대해 이해해야 한다. (해당 내용에 대해선 자바 NIO 키워드로 검색)</p>
<p>NIO Connector에선 Poller라고 하는 별도의 스레드가 커넥션을 처리한다. Poller는 Socket들을 캐시로 들고 있다가 해당 Socket에서 data에 대한 처리가 가능한 순간에만 thread를 할당하는 방식을 사용해서 thread이 idle 상태로 낭비되는 시간을 줄여준다</p>
<p>NIO Connector의 흐름은 다음 그림과 같다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/41452605-1bc8-40df-90f6-032a928e7abc/image.png" /></p>
<p>Acceptor는 이름 그대로 Socket Connection을 accept한다. serverSocket.accept() 방식을 사용하고 있다. 소켓에서 Socket Channel 객체를 얻어서 톰캣의 NioChannel 객체로 변환한다. 그리고 추가로 NioChannel 객체를 PollerEvent라는 객체로 한번 더 캡슐화해서 event queue에 넣게 된다. Acceptor는 event Queue의 공급자, Poller thread는 event Queue의 사용자라고 할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/ac8b13ba-9cbd-4254-85c2-950b025d59d5/image.png" /></p>
<p>Poller는 NIO의 Selector를 가지고 있다. Selector에는 다수의 채널이 등록되어 있고, select 동작을 수행하여 데이터를 읽을 수 있는 소켓을 얻는다. 그리고 Worker Thread Pool에서 이용할 수 있는 Woker Thread를 얻어서 해당 소켓을 worker thread에게 넘기게 된다.</p>
<p>Java Nio Selector를 사용해서 data 처리가 가능할 때만 Thread를 사용하기 때문에 idle 상태로 낭비되는 Thread가 줄어들게 된다. Poller에선 Max Connection까지 연결을 수락하고, Selector를 통해 채널을 관리하므로 작업큐 사이즈와 관계 없이 추가로 커넥션을 refuse하지 않고 받아놓을 수 있다.</p>
<p>스레드 또한 모자라다면 max사이즈 까지 스레드를 추가하는 것을 볼 수 있었는데, 해당 이유에 대해선 확인 후 보충하겠다</p>
<p>아래는 tomcat 8.0 버전의 Connector 구현체들이다.</p>
<p><a href="https://tomcat.apache.org/tomcat-8.0-doc/config/http.html#Connector_Comparison">https://tomcat.apache.org/tomcat-8.0-doc/config/http.html#Connector_Comparison</a></p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/b5f07fb4-7d22-4e80-82ba-4288f386d383/image.png" /></p>
<p>요약
NIO 기반의 Connector는 하나의 Connection이 하나의 스레드를 할당받는 BIO Connector에 비해, Selector를 활용해 Socket을 관리하므로 더 적은 스레드를 사용한다. 또한 max-connections값까지 접속을 유지하고, 스레드가 모자라다면 max 사이즈까지 스레드를 추가한다. time-wait시간 안에 처리가 가능하다면 처리할 수 있다.</p>
<p>출처 : <a href="https://s-y-130.tistory.com/211">https://s-y-130.tistory.com/211</a></p>