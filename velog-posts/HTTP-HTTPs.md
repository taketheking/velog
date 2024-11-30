<h1 id="http">HTTP</h1>
<hr />
<p>HyperText Transfer Protocol</p>
<ul>
<li>프로토콜</li>
</ul>
<p>직역하자면 Hyper Text 전송 프로토콜 입니다. Hyper Text를 전달하기 위한 약속 이라고 생각하시면 됩니다.  여기서 Hyper Text는 HTML이라 생각하시면 됩니다. 
맞습니다. 결국 http는 HTML 전송을 위한 프로토콜 입니다.</p>
<p>HTTP는 웹 페이지, 이미지, 동영상 등의 데이터를 클라이언트와 서버 간에 전송하기 위한 기본적인 통신 프로토콜입니다.</p>
<blockquote>
<p>HTTP의 역사</p>
</blockquote>
<p>사실 요즘 HTTP는 HTML 뿐만 아니라 XML, JSON, 이미지, 비디오, 등등의 문서를 전송할 수 있습니다.  1991년 당시에 웹은 지금과 전혀 다르게 HTML만 존재했습니다. 이 HTML을 전송하기 위한 프로토콜이라서 Hyper Text Transfer Protocol 이란 이름을 가지게 됩니다. HTML이 Hyper Text Markup Language의 약자거든요.</p>
<blockquote>
</blockquote>
<p>  <img alt="" src="https://velog.velcdn.com/images/taketheking/post/3bf5009b-9237-474e-b971-0ac7e6464c79/image.png" /></p>
<blockquote>
</blockquote>
<h2 id="http-동작-방식">HTTP 동작 방식</h2>
<p>클라이언트(웹 브라우저)가 서버에 요청(Request)을 보내고, 
서버가 해당 요청에 대한 응답(Response)을 클라이언트에 반환하는 방식입니다. </p>
<p>예를 들어, 웹 브라우저의 주소창에 <a href="http://example.com%EC%9D%84">http://example.com을</a> 입력하면 브라우저가 서버에 HTML 페이지를 요청하고, 서버는 해당 페이지를 응답으로 반환합니다.</p>
<h2 id="http-특징">HTTP 특징</h2>
<ul>
<li>비연결 (Connectionless)<ul>
<li>연결을 유지하지 않습니다. 인터넷이 종료 되어도 페이지가 유지되죠. 반대의 예는 게임 서버입니다. 게임은 연결이 끊기면 서버에서 튕깁니다.</li>
</ul>
</li>
<li>무상태 (Stateless)<ul>
<li>서버가 이전 요청의 상태를 기억하지 않습니다. 즉, 각각의 요청이 독립적으로 처리되며, 서버는 클라이언트의 이전 요청 정보를 알지 못한 채 매번 새로운 요청으로 처리합니다.</li>
</ul>
</li>
<li>클라이언트 서버 구조
클라이언트가 서버에 요청을 보내면, 서버가 요청에 대한 응답을 보내는 클라이언트-서버 구조로 이루어져 있다.</li>
</ul>
<h3 id="http-method">HTTP Method</h3>
<ul>
<li>GET : 조회</li>
<li>POST : 등록</li>
<li>PUT : 전체 수정</li>
<li>PATCH : 일부 수정</li>
<li>DELETE : 삭제</li>
</ul>
<h2 id="http-포트">HTTP 포트</h2>
<p>HTTP는 기본적으로 포트 80을 사용합니다.</p>
<h2 id="http-보안">HTTP 보안</h2>
<p>HTTP는 데이터를 암호화하지 않고 전송하기 때문에, 전송 중인 데이터가 중간에서 도청되거나 변조될 위험이 있습니다. 이를 통해 민감한 정보를 전송하는 것은 매우 위험할 수 있습니다.</p>
<h1 id="https">HTTPS</h1>
<hr />
<p>HTTPS는 HTTP에 SSL/TLS(Secure Sockets Layer/Transport Layer Security) 암호화 계층을 추가한 프로토콜입니다. 이를 통해 데이터가 암호화되어 전송되므로, 중간에 누군가가 데이터를 가로채더라도 해독할 수 없습니다.</p>
<h2 id="https-동작-방식">HTTPS 동작 방식</h2>
<p>HTTPS는 클라이언트와 서버가 서로 통신하기 전에 SSL/TLS 인증서를 사용해 안전한 연결을 설정합니다. 이 연결을 통해 데이터가 암호화되어 전송되며, 도청, 변조, 피싱 등의 공격으로부터 보호받을 수 있습니다.</p>
<h2 id="https-포트">HTTPS 포트</h2>
<p>HTTPS는 기본적으로 포트 443을 사용합니다.</p>
<h2 id="https-보안">HTTPS 보안</h2>
<p>HTTPS는 데이터 암호화, 데이터 무결성, 서버 인증을 제공합니다.</p>
<blockquote>
</blockquote>
<ol>
<li>암호화: 클라이언트와 서버 간의 모든 데이터가 암호화되어 전송되므로, 중간에서 데이터를 가로채더라도 내용을 해독할 수 없습니다.</li>
<li>무결성: 전송 중 데이터가 변조되지 않도록 보호합니다. 만약 데이터가 중간에서 변경되면 클라이언트와 서버는 이를 감지할 수 있습니다.</li>
<li>서버 인증: SSL/TLS 인증서를 통해 클라이언트가 통신하려는 서버가 실제로 신뢰할 수 있는 서버인지 확인할 수 있습니다.</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/088330f9-8b1f-461c-9d10-bba9f97d65e6/image.png" /></p>