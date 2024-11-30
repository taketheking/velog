<h1 id="tcp-vs-udp">TCP vs UDP</h1>
<hr />
<p>TCP = Transmission Control Protocol</p>
<p>UDP = User Datagram Protocol</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/bfa33182-6fe1-498c-9b0c-f61ec3ab1385/image.png" /></p>
<p>TCP와 UDP 모두 프로토콜 입니다. 데이터를 어떻게 전달할지 써놓은 규약같은 겁니다.
일반적으로 신뢰성이 요구되는 애플리케이션에서는 TCP를 사용하고 간단한 데이터를 빠른 속도로 전송하고자 하는 애플리케이션에서는 UDP를 사용한다.</p>
<h2 id="tcp">TCP</h2>
<hr />
<blockquote>
</blockquote>
<p>TCP(Transmission Control Protocol, 전송 제어 프로토콜)는 인터넷을 통해 디바이스에서 웹 서버로 데이터를 전송하는 네트워크 프로토콜입니다. TCP/IP 프로토콜이라고 불리기도 합니다. 메신저에서 친구와 채팅을 하거나, 이메일을 보내거나, 온라인 동영상을 보거나, 웹을 검색할 때마다 TCP 프로토콜을 사용합니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/cc525d6d-daa3-4201-8db2-ad993d1d105d/image.png" /></p>
<p>TCP는 연결 기반이므로 데이터를 전송하는 동안 수신자와 발신자 사이에 연결을 설정하고 이를 유지합니다. 데이터가 완전히 온전하게 도착하도록 보장합니다. 이러한 신뢰성 때문에 TCP는 가장 널리 사용되는 네트워크 프로토콜입니다.</p>
<p><strong>- 연결하는 방법</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/f48d5776-4be9-4761-9b51-b739e0cdd512/image.png" /></p>
<p><strong>3-way handshake</strong>로 연결하여 신뢰성 보장</p>
<h3 id="tcp의-장점">TCP의 장점</h3>
<p>•   운영체제와 독립적으로 작동하여 여러 장치에서 가능 (호환성)</p>
<p>•    <strong>3-way handshake</strong>로 연결 수립</p>
<p>•    데이터의 오류 검사로 <strong>순서 보장</strong>과 <strong>손실 복구</strong> (재전송)</p>
<p>•    <strong>흐름(속도) 제어</strong></p>
<blockquote>
<p>데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지</p>
</blockquote>
<ul>
<li>송신하는 곳에서 감당이 안되게 많은 데이터를 빠르게 보내 수신하는 곳에서 문제가 일어나는 것을 막는다.</li>
<li>수신자가 윈도우크기(Window Size) 값을 통해 수신량을 정할 수 있다.</li>
</ul>
<p>•   <strong>혼잡 제어</strong></p>
<blockquote>
</blockquote>
<p>네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지</p>
<blockquote>
</blockquote>
<ul>
<li>정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는 것을 막는다.</li>
</ul>
<p>•   4-way handshaking 을 통해 연결을 해제.</p>
<p>• 전이중 (Full-Duplex) : 전송이 양방향으로 동시에 일어날 수 있다.</p>
<h3 id="tcp의-단점">TCP의 단점</h3>
<p>• 상당히 많은 대역폭을 사용하고 속도가 느림(검사 때문에)</p>
<p>• 소량의 데이터 손실이 다른 데이터의 손실로 이어짐</p>
<p>• 근거리 통신망이나 개인 영역 네트워크에서 작동하지 않음</p>
<p>• 점대점 (Point to Point) : 각 연결이 정확히 2개의 종단점을 가지고 있다.
=&gt; 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다.</p>
<h3 id="tcp-동작-원리">TCP 동작 원리</h3>
<ol>
<li>TCP는 각 데이터 패킷에 고유 식별자와 시퀀스 번호를 할당합니다. 이를 통해 수신자는 어떤 패킷이 수신되었고 다음에 어떤 패킷이 도착하는지 식별할 수 있습니다.</li>
</ol>
<p>2.데이터 패킷이 수신되고 올바른 순서로 도착하면 수신자는 발신자에게 수신 확인을 보냅니다.</p>
<ol start="3">
<li><p>이제 발신자는 다른 패킷을 보낼 수 있습니다.</p>
</li>
<li><p>패킷이 분실되거나 잘못된 순서로 전송된 경우, 수신자는 동일한 데이터 패킷을 다시 보내야 함을 알리는 침묵 상태를 유지합니다.</p>
</li>
</ol>
<blockquote>
</blockquote>
<p>데이터가 순차적으로 전송되기 때문에 데이터 혼잡과 흐름 제어에 도움이 되며 오류를 쉽게 발견하고 수정할 수 있습니다. 또한 TCP를 통해 전송된 데이터가 목적지에 온전히 도달할 가능성이 더 높다는 의미이기도 합니다. 하지만 단점도 있습니다. 두 당사자 간에 많은 양의 통신이 오가기 때문에 연결을 설정하고 데이터를 교환하는 데 시간이 오래 걸립니다.</p>
<h3 id="tcp-헤더-정보">TCP 헤더 정보</h3>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/9cd625fd-bea8-42da-a2fa-d2201eab2add/image.PNG" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/c68835db-500b-4259-a1c7-a79f3a68e856/image.PNG" /></p>
<h3 id="tcp의-연결-및-해제-과정">TCP의 연결 및 해제 과정</h3>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/cae0e7be-2be8-4903-be8f-08074c7bbfae/image.png" /></p>
<h4 id="tcp-connection-3-way-handshake">TCP Connection (3-way handshake)</h4>
<p>먼저 open()을 실행한 클라이언트가 SYN을 보내고 SYN_SENT 상태로 대기한다.
서버는 SYN_RCVD 상태로 바꾸고 SYN과 응답 ACK를 보낸다.
SYN과 응답 ACK을 받은 클라이언트는 ESTABLISHED 상태로 변경하고 서버에게 응답 ACK를 보낸다.
응답 ACK를 받은 서버는 ESTABLISHED 상태로 변경한다.</p>
<h4 id="tcp-disconnection-4-way-handshake">TCP Disconnection (4-way handshake)</h4>
<p>먼저 close()를 실행한 클라이언트가 FIN을 보내고 FIN_WAIT1 상태로 대기한다.
서버는 CLOSE_WAIT으로 바꾸고 응답 ACK를 전달한다. 동시에 해당 포트에 연결되어 있는 어플리케이션에게 close()를 요청한다.
ACK를 받은 클라이언트는 상태를 FIN_WAIT2로 변경한다.
close() 요청을 받은 서버 어플리케이션은 종료 프로세스를 진행하고 FIN을 클라이언트에 보내 LAST_ACK 상태로 바꾼다.
FIN을 받은 클라이언트는 ACK를 서버에 다시 전송하고 TIME_WAIT으로 상태를 바꾼다. TIME_WAIT에서 일정 시간이 지나면 CLOSED된다. ACK를 받은 서버도 포트를 CLOSED로 닫는다.</p>
<blockquote>
<p><strong>주의</strong>
반드시 서버만 CLOSE_WAIT 상태를 갖는 것은 아니다.
서버가 먼저 종료하겠다고 FIN을 보낼 수 있고, 이런 경우 서버가 FIN_WAIT1 상태가 됩니다.
누가 먼저 close를 요청하느냐에 따라 상태가 달라질 수 있다.</p>
</blockquote>
<h2 id="udp">UDP</h2>
<hr />
<p>UDP는 ‘User Datagram Protocol,’ 즉 ‘사용자 데이터그램 프로토콜’의 약자입니다. UDP 네트워크 프로토콜은 TCP에 비해 안정성(신뢰도)은 떨어지지만 더 빠르고 간단합니다. 그래서 스트리밍이나 게임과 같이 빠른 속도가 중요한 상황에서 자주 사용됩니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/85c82fb0-21de-4ee1-83d4-cf5f5e1d31f4/image.png" /></p>
<h3 id="udp-장점">UDP 장점</h3>
<p>• UDP는 더 작은 패킷을 더 적은 오버헤드로 전송하여 엔드투엔드 지연을 줄임(빠른 속도)</p>
<p>• UDP는 일부 패킷이 누락되더라도 데이터를 전송하므로 패킷 손실로 인해 전체 전송이 중단되지 않음</p>
<p>• 브로드캐스트 및 멀티캐스트 기능을 통해 하나의 UDP 전송을 여러 수신자에게 한 번에 전송할 수 있음</p>
<p>• UDP 전송은 TCP와 같은 다른 옵션보다 더 빠르고 효율적인 방법</p>
<p>• 데이터 오류 검사를 위한 체크섬 존재</p>
<h3 id="udp-단점">UDP 단점</h3>
<p>• UDP는 데이터 패킷이 목적지에 성공적으로 도달했는지 여부를 확인 x</p>
<p>• UDP는 전송이 온전하게 도착한다고 보장할 수 없음. 일부 패킷이 손실되었을 수 있지만 발신자 측에서 이를 확인할 수 있는 방법은 없음.</p>
<p>• 라우터가 데이터 패킷의 우선순위를 정해야 하는 경우, UDP 패킷보다 TCP 패킷을 먼저 전송할 가능성이 높음</p>
<p>• UDP는 특정 순서로 데이터를 전송하지 않으므로 패킷은 어떤 순서로든 도착할 수 있음(순서 보장 x)</p>
<blockquote>
</blockquote>
<h4 id="그럼-왜써요">그럼 왜써요?</h4>
<blockquote>
</blockquote>
<p>우리가 통신을 할 때 일부분 유실이 되어도 괜찮은 경우가 있습니다.
예를 들면, 실시간 스트리밍 서비스, 온라인 게임, 인터넷 전화 등이 있죠.
또한 TCP는 신뢰성이 있지만 연결과정, 유실 체크 등으로 인해 상대적으로 느립니다.
하지만 UDP는 상대적으로 빠른 속도를 보여 줍니다.</p>
<h4 id="데이터가-유실-된다면-ip랑-다른게-뭐죠">데이터가 유실 된다면 IP랑 다른게 뭐죠?</h4>
<p>IP는 단순 호스트간 통신만 지원합니다.</p>
<p>UDP는 PORT(포트)를 지원하고, 데이터 무결성 검사도 가능합니다.</p>
<p>HTTP/3에는 구글이 제안하고 표준화 시킨 QUIC이란 프로토콜을 사용합니다.</p>
<p>이것은 내부적으로 UDP를 사용하여 빠른 속도로 통신을 합니다.</p>
<h3 id="udp-동작-원리">UDP 동작 원리</h3>
<p>UDP는 고유 식별자나 시퀀스 번호 없이도 TCP와 동일한 작업을 수행하는 방식으로 작동합니다. 스트림으로 데이터를 전송하며 데이터가 손상되지 않고 도착했는지 확인하기 위한 체크섬만 있습니다. UDP는 오류 수정 기능이 거의 없으며 패킷 손실에 대해서도 신경 쓰지 않습니다. 오류가 발생하기 쉽지만 TCP보다 훨씬 빠르게 데이터를 전송합니다.</p>
<h3 id="udp-헤더-정보">UDP 헤더 정보</h3>
<p>응용 계층으로부터 데이터 받은 UDP도 UDP 헤더를 추가한 후에 이를 IP로 보낸다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/ede7d5d6-5ec0-4d0b-9553-753bacedf6a6/image.png" />
&lt;출처 : <a href="https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&amp;blogId=minki0127&amp;logNo=220804490550&gt;">https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&amp;blogId=minki0127&amp;logNo=220804490550&gt;</a>
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/0d6dd451-59db-4fff-984c-63ca8db8e6ed/image.PNG" /></p>
<blockquote>
</blockquote>
<p>🚨</p>
<blockquote>
</blockquote>
<p>UDP를 쓰는 QUIC 방식은 어떻게 데이터 유실을 방지할까요?</p>
<blockquote>
</blockquote>
<p>관련 내용 한번 스스로 찾아보시기 바랍니다.</p>
<h1 id="ip">IP</h1>
<blockquote>
</blockquote>
<p>인터넷 프로토콜(IP)은 데이터 패킷이 네트워크를 통해 이동하고 올바른 대상에 도착할 수 있도록 데이터 패킷을 라우팅하고 주소를 지정하기 위한 프로토콜 또는 규칙의 집합입니다. 인터넷을 통과하는 데이터는 패킷이라고 하는 더 작은 조각으로 나뉩니다. IP 정보는 각 패킷에 첨부되며, 이 정보는 라우터가 패킷을 올바른 위치로 보내는 데 도움이 됩니다. 인터넷에 연결하는 모든 장치나 도메인에는 IP 주소가 할당되며, 패킷이 연결된 IP 주소로 전달되면 데이터가 필요한 곳에 도착합니다.</p>
<blockquote>
</blockquote>
<p>패킷이 목적지에 도착하면 IP와 함께 어떤 전송 프로토콜이 사용되는지에 따라 다르게 처리됩니다. 가장 일반적인 전송 프로토콜은 TCP와 UDP입니다.</p>
<blockquote>
</blockquote>
<p>네트워킹에서 프로토콜은 둘 이상의 장치에서 서로 통신하고 이해할 수 있도록 특정 작업을 수행하고 데이터 형식을 지정하는 표준화된 방법입니다.</p>
<blockquote>
</blockquote>
<p>프로토콜이 필요한 이유를 이해하려면 편지를 부치는 과정을 생각해보면 됩니다. 봉투에는 주소가 도, 시, 주소, 이름, 우편 번호의 순서로 기록됩니다. 우편 번호를 먼저 쓴 다음 도로명 주소, 시 등을 차례로 쓴 봉투를 우편함에 넣으면 우체국에서 배달하지 않습니다. 우편 시스템이 작동하려면 합의된 프로토콜에 따라 주소를 작성해야 합니다. 같은 방식으로 모든 IP 데이터 패킷은 특정 정보를 특정 순서로 표시해야 하며 모든 IP 주소는 표준화된 형식을 따릅니다.</p>
<hr />
<blockquote>
</blockquote>
<p>OSI 7 계층에서의 Network Layer 이면서, 인터넷 프로토콜 스위트의 Internet Layer 에
해당하는 IP는 호스트간의 통신만을 담당 한다는 점이 특징입니다</p>
<blockquote>
</blockquote>
<p>이는 택배로 비유하자면, 내용물의 상태나, 그 주소에 수취인이 있는지 등은 고려하지 않고
일단은 배송 요청이 오면 내용물을 받기로 되어있는 주소로 보내는 것 이라고 이해 하였습니다.</p>
<blockquote>
</blockquote>
<p>내용물의 상태를 보장한다는 부분은 신뢰성(Reliability) 을 의미하고,
수취인이 있는지 등을 고려한다는 점은 연결 (Connection) 에 관한 것입니다</p>
<blockquote>
</blockquote>
<p>IP는 이 두가지 다 보장하지 않습니다.
따라서, IP의 특징은 비 신뢰성(Unreliability)과, 비 연결성(Connectionlessness) 입니다.</p>
<blockquote>
</blockquote>
<p>&quot;아니 그럼 내 택배의 상태는 잘 도착했는지, 멀쩡한지는 어떻게 보장할 수 있어요?&quot;
→ TCP 에서 그러한 것을 담당하고 있습니다.</p>
<blockquote>
</blockquote>
<p>IP 와 동일한 Layer 의 다른 프로토콜은 IP, ICMP, ARP, RARP 가 있습니다.</p>
<hr />
<h2 id="ipinternet-protocol-주소">IP(Internet Protocol) 주소</h2>
<p>IP(Internet Protocol) 주소란 인터넷에 연결되어 있는 모든 장치들(컴퓨터, 서버 장비, 스마트폰 등)을 식별할 수 있도록 각각의 장비에게 부여되는 고유 주소이다.</p>
<h3 id="1-ipv4-ipv6">1. IPv4, IPv6</h3>
<p>IP주소는 IPv4, IPv6 2가지 종류가 있으며, 일반적으로 IP 주소라 하면 IPv4 주소를 말한다.</p>
<h4 id="ipv4">IPv4</h4>
<p>IPv4는 IP version 4의 약자로 전 세계적으로 사용된 첫 번째 인터넷 프로토콜이다.</p>
<p>주소는 32비트 방식으로, 8비트씩 4자리로 되어 있으며 각 자리는 온점으로 구분한다.</p>
<p>ex) 115.68.24.88</p>
<p>IPv4는 0 ~ 2^32 (약 42억 9천)개의 주소를 가질 수 있는데, </p>
<p>전 세계적으로 인터넷 사용자 수가 급증하면서 IPv4 주소가 고갈될 위기에 처해있다.</p>
<p>이러한 고갈 문제를 해결하기 위해 등장한 주소가 바로 IPv6이다.</p>
<h4 id="ipv6">IPv6</h4>
<p>IPv6는 IP version 6의 약자로,</p>
<p>IPv4의 주소체계를 128비트 크기로 확장한 차세대 인터넷 프로토콜 주소이다.</p>
<p>16비트씩 8자리로 각 자리는 콜론으로 구분한다.</p>
<p>ex) 2001:0DB8:1000:0000:0000:0000:1111:2222</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/b281c360-68f2-475a-8701-1567bf3f2a2b/image.png" /></p>
<p>네트워크 속도, 보안적인 부분뿐만 아니라 여러 면에서 뛰어나지만</p>
<p>기존의 주소체계를 변경하는데 비용이 많이 들어서 아직 완전히 상용화가 되지 않았다.</p>
<ol start="2">
<li>고정 IP, 유동 IP
IP는 나누는 방식에 따라 고정 IP, 유동 IP와 공인 IP, 사설 IP로 나눌 수 있다.</li>
</ol>
<p>고정 IP는 말 그대로 변하지 않고 컴퓨터에 고정적으로 부여된 IP이다.</p>
<p>한번 부여되면 IP 반납을 하기 전까지는 다른 장비에 부여할 수 없는 고유의 IP로</p>
<p>보안성이 우수하기 때문에 보안이 필요한 업체나 기관에서 사용한다.</p>
<p>유동 IP 역시 말그대로 변하는 IP이다.</p>
<p>인터넷 사용자 모두에게 고정 IP를 부여해 주기는 힘들기 때문에,</p>
<p>일정한 주기 또는 사용자들이 인터넷에 접속하는 매 순간마다 사용하고 있지 않은 IP 주소를 임시로 발급해 주는 IP이다.</p>
<p>대부분의 사용자는 유동 IP를 사용한다.</p>
<ol start="3">
<li>공인 IP, 사설 IP
IP 주소는 임의로 우리가 부여하는 것이 아니라 전 세계적으로 ICANN이라는 기관이</li>
</ol>
<p>국가별로 사용할 IP 대역을 관리하고, 우리나라는 한국인터넷진흥원(KISA)에서 국내 IP 주소들을 관리하고 있다.</p>
<p>이것을 ISP(Internet Service Provider의 약자로 KT, LG, SKT와 같이 인터넷을 제공하는 통신업체)가 부여받고,</p>
<p>우리는 위 회사에 가입을 통해 IP를 제공받아 인터넷을 사용하게 되는 것이다.</p>
<p>이렇게 발급받은 IP를 공인 IP라고 한다.</p>
<p>공유기를 사용한 인터넷 접속 환경일 경우 공유기까지는 공인 IP 할당을 하지만,</p>
<p>공유기에 연결되어 있는 가정이나 회사의 각 네트워크 기기에는 사설 IP를 할당한다.</p>
<p>사설 IP는 어떤 네트워크 안에서 내부적으로 사용되는 고유한 주소이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/dd186e75-21e5-4503-b3c3-0709919e9b64/image.png" /></p>
<p>즉, 공인 IP는 전 세계에서 유일하지만, 사설 IP는 하나의 네트워크 안에서 유일하다.</p>
<p>공인 IP는 외부, 내부 상관없이 해당 IP에 접속할 수 있으나, 사설 IP는 내부에서만 접근이 가능하다.</p>
<h1 id="port">PORT</h1>
<hr />
<p>이전에 IP를 통해 패킷을 전달하여 통신을 한다고 했습니다.</p>
<p>IP는 장치 하나가 부여 받는 주소 입니다.</p>
<p>우리는 컴퓨터나 스마트폰을 쓸 때 여러 프로그램을 띄워 사용합니다.</p>
<p>당장 지금 사용 중인 컴퓨터만 보아도 인터넷 브라우저, 카톡, 슬랙, ZOOM 등이 띄워져 있습니다.</p>
<p>IP는 특정 장치까지는 특정할 수 있으나,</p>
<p>그 장치 안에 누가 데이터를 받을지 특정할 수 없습니다.</p>
<p>이때 필요한 것이 PORT(포트) 입니다.</p>
<p>항구할 때 그 포트와 동일 어원을 가집니다.</p>
<p>물론 USB 포트 할 때 포트도 같습니다.</p>
<p>일반적으로 이 개념은 아파트로 많이 비유합니다.</p>
<ul>
<li>아파트 주소 (동까지) = IP</li>
<li>호 수 = 포트</li>
</ul>
<p>ex)</p>
<p>IP = 서울시 스파구 르타동 99번지 101동
PORT = 707호</p>
<h3 id="포트의-범위">포트의 범위</h3>
<p>포트는 0 ~ 65,535까지 할당 가능합니다.</p>
<p>하나의 장치에서 서로 다른 프로그램이 동일한 포트를 쓸 수 없다</p>
<h3 id="잘-알려진-포트">잘 알려진 포트</h3>
<p>0 ~ 1023 범위를 가지고 국제 도메인 관리기구에 의해 관리 됩니다.</p>
<p>본인이 만든 프로그램에서는 이 범위의 포트를 사용하지 않는 것이 좋습니다. (보안의 구멍이 되기도 함)</p>
<ul>
<li><code>FTP</code> - 20, 21 (TCP)</li>
<li><code>SSH</code> - 22 (TCP)</li>
<li><code>텔넷</code> - 23 (TCP)</li>
<li><code>SMTP</code> - 25 (TCP)</li>
<li><code>DNS</code> - 53 (TCP/UDP)</li>
<li><code>DHCP</code> - 67 (UDP)</li>
<li><code>HTTP</code> - 80 (TCP)</li>
<li><code>HTTPS</code> - 443 (TCP)</li>
<li><code>Tomcat</code> - 8080 (TCP)</li>
<li><code>RDP</code> - 3389 (TCP/UDP, 주로 윈도우 원격 데스크탑)</li>
</ul>
<p>여기까지 우리는 각 장치끼리 통신에 대한 용어들을 알아보았습니다.</p>
<p>즉, IP를 이용하여 우리는 통신을 주고 받습니다.</p>
<p>물론 앞서 알아본 내용들은 IP를 사용한 그 이후 단계에 내부에서 일어나는 일들입니다.</p>
<p>표면적으로는 IP를 알아야 우리는 통신을 할 수 있습니다.</p>
<p>거기 추가로 포트를 통해 특정 컴퓨터의 특정 프로그램과 통신을 할 수 있습니다.</p>
<h1 id="packet">Packet</h1>
<hr />
<p>패킷은 전달하는 작은 데이터 조각을 의미합니다. Package(패키지)와 비슷한 의미로 이해하면 편합니다. 사실상 두 영어단어의 어원도 동일합니다.</p>
<blockquote>
<p><em>라틴어 “pacare” (묶다, 정리하다)에서 유래</em></p>
</blockquote>
<p>여러가지 패킷이 있지만 그 중 하나만 살펴보면 아래와 같습니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/423dbccd-d82d-4f13-98b1-6755ecb0cc83/image.png" /></p>
<hr />
<h3 id="1-버전version">1. 버전(Version)</h3>
<p>-IP프로토콜의 버전을 의미. IPv4인지?IPv6인지?</p>
<p>-4, 6으로 표시</p>
<h3 id="2-헤더길이header-length">2. 헤더길이(Header Length)</h3>
<p>-옵션 필드를 포함한 헤더의 총 길이</p>
<p>-옵션이 가변적이기 때문에 최소20byte ~ 최대60byte이다.</p>
<h3 id="3-서비스타입type-of-service">3. 서비스타입(Type-Of-Service)</h3>
<p>-IP패킷의 우선순위를 결정.</p>
<h3 id="4-전체길이total-length">4. 전체길이(Total Length)</h3>
<p>-헤더+데이터를 포함한IP패킷의 전체 길이.</p>
<p>-바이트단위로 나타낸다.</p>
<h3 id="5-식별자identification">5. 식별자(Identification)</h3>
<p>-패킷을 조립하거나 분해할때 식별하는 번호</p>
<p>-IP에서는 링크의 최대 전달 유닛(MTU : Maximum Transmission Unit)보다 더 큰 데이터를 보내야 하는 경우 여러 개의 작은 패킷으로 분할해서 전송할 수 있다. 분할된 패킷들은 목적지에서 재조립되는데, 동일한 데이터로부터 분할되었음을 인지하고 재조립할 수 있도록 분할된 패킷들을 같은 식별자 필드 값을 갖는다.</p>
<h3 id="6-플래그flag">6. 플래그(Flag)</h3>
<p>-패킷이 단편화되었는지 아닌지 단서를 제공하는 역할.</p>
<p>-플래그 필드는 세 비트로 구성..</p>
<p>-첫번째 비트는 예약된 필드로 사용되지 않는다.</p>
<p>-두번째 비트는 단편화금지(Don't fragment) 비트로 '1'이면 패킷을 분할할 수 없다.</p>
<p>만약 패킷을 분할해야 하는 경우에 이 비트가 '1'이라면 패킷을 폐기시키고, 송신측으로 ICMP오류 메시지를 보낸다.</p>
<p>-세번째 비트는 추가단편화(More Fragment)비트로써 이 비트가 '0'이라면 해당 패킷이 분할된 패킷 중 마지막 패킷임을 나타낸다.</p>
<h3 id="7-단편화-옵셋fragment-offset">7. 단편화 옵셋(Fragment Offset)</h3>
<p>-식별자필드와 플래그필드를 이용하여 패킷을 재조립하기 위해 분할된 패킷간의 순서에 대한 정보를 제공.</p>
<p>-식별자에서 쪼개진 패킷에 대한 순서정보를 표시</p>
<h3 id="8-time-to-livettl">8. Time-To-Live(TTL)</h3>
<p>-패킷이 경유할 수 있는 최대 홉 수.</p>
<p>-패킷이 라우터를 통과할 때마다 TTL값은 1씩 감소되는데 TTL 값이 0이 되면 패킷은 폐기되고 송신측으로 ICMP메세지가 전달된다.</p>
<p>-if)TTL이 없다면 패킷이 노드상에 계속돌아다니게 된다. 무한loop. 이것을 방지하기 위해 TTL이 있다.</p>
<h3 id="9-프로토콜protocol">9. 프로토콜(Protocol)</h3>
<p>-IP패킷이 어떤 상위 프로토콜과 관련되는지를 나타내기 위해 사용.</p>
<p>-ICMP : 1, TCP : 6, UDP : 17을 의미.</p>
<p>-if)번호가 6번이면 IP헤더를 떼고 트랜스포트계층으로 올려준다.</p>
<h3 id="10-헤더-체크섬header-checksum">10. 헤더 체크섬(Header Checksum)</h3>
<p>-오류 발생을 검사하기 위한 필드.</p>
<h3 id="11-송신자-ip주소source-address">11. 송신자 IP주소(Source Address)</h3>
<p>-출발지 주소를 나타낸다.</p>
<h3 id="12-수신자-ip주소destination-address">12. 수신자 IP주소(Destination Address)</h3>
<p>-목적지 주소를 나타낸다.</p>
<hr />
<blockquote>
</blockquote>
<p>IPv4 패킷</p>
<blockquote>
</blockquote>
<p>하지만 IP를 통해 통신을 하면 문제가 있습니다.</p>
<blockquote>
</blockquote>
<ol>
<li><strong>비연결성</strong> → 수신 대상의 현재 상태에 상관없이 전송한다.</li>
<li><strong>비신뢰성</strong> → 많은 과정을 거치다가 데이터가 소실된다.
용량이 크면 여러 패킷으로 나뉘어 전송되지만 도착하는 순서가 뒤바뀐다.<blockquote>
</blockquote>
이 문제를 해결하기 위해 TCP 프로토콜이 만들어졌습니다.</li>
</ol>
<blockquote>
</blockquote>
<p>🚨</p>
<blockquote>
</blockquote>
<p>물론 TCP가 IP 통신만을 대체하기 위해 나온 것은 아닙니다.</p>
<blockquote>
</blockquote>
<p>여러 복잡적인 이유로 TCP가 만들어졌습니다.</p>
<blockquote>
</blockquote>
<p>IP 통신의 단점을 해결은 TCP의 여러 장점 중 하나일 뿐입니다.</p>
<blockquote>
</blockquote>
<hr />
<p>출처 및 참고</p>
<p><a href="https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4">https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4</a></p>
<p><a href="https://nordvpn.com/ko/blog/tcp-udp-comparison/">https://nordvpn.com/ko/blog/tcp-udp-comparison/</a></p>
<p><a href="https://baebalja.tistory.com/475">https://baebalja.tistory.com/475</a></p>
<p><a href="https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&amp;blogId=jyj9372&amp;logNo=50170030307">https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&amp;blogId=jyj9372&amp;logNo=50170030307</a></p>