<p>intro
URI와 URL 그리고 URN까지 개념이 명확하게 나누어지지 않아서 이해가 가지 않아 내용을 정리해 보았다.
단순하게 말하면, 모든 인터넷 주소는 URI이고 그 중에 도메인을 사용하면 거의 URL이고 특수한 규칙을 따르는 URN이 있다. URL과 URN은 모두 URI 이다. 
URL도 아니면서 URN도 아니면서 URI인 경우도 있으나 거의 보기 힘들다.</p>
<h1 id="uriurlurn">URI/URL/URN</h1>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/251f2ee8-10b3-4fd8-885f-9d94f1270054/image.png" /></p>
<h1 id="1-uri-uniform-resource-identifier">1. URI (Uniform Resource Identifier)</h1>
<h2 id="정의">정의</h2>
<p>URI은 네트워크 상의 리소스를 식별할 수 있는 식별자 전체를 말한다. 
URI는 인터넷에서 자원을 식별하는 데 사용되는 문자열이다. '자원'은 웹 페이지, 문서, 이미지, 파일, 그리고 백엔드에서 자주 사용하는 API Endpoint 를 포함하는 개념이다.</p>
<h2 id="기능">기능</h2>
<p>URI는 특정 자원을 식별하고, 해당 자원이 어떻게 접근될 수 있는지에 대한 정보를 제공한다.</p>
<h2 id="종류">종류</h2>
<p>URI에는 두 가지 주요 형태가 있다.
URL (Uniform Resource Locator)
URN (Uniform Resource Name)</p>
<blockquote>
</blockquote>
<p><strong>URI이지만 URL과 URN이 아닌 예시?</strong></p>
<blockquote>
</blockquote>
<p>URI 중에서 URL이나 URN에 해당하지 않는 예시는 드물고 사용되지 않는다.
대부분의 URI는 URL으로 사용된다. (웹 사이트의 주소 - 특정 자원을 주소로 나타내는 그 방식 말이다.)</p>
<blockquote>
</blockquote>
<p>그러나 이론적으로 가능하긴 하다. 이러한 URI는 자원을 식별하지만, 그 자원의 위치(URL)나 이름(URN)을 구체적으로 지정하지 않는 경우를 뜻한다.</p>
<blockquote>
</blockquote>
<p>예를 들어, 어떤 시스템에서 내부적으로 사용되는 식별자는 URI일 수 있지만,
외부적으로 접근 가능한 위치(URL)나 표준화된 이름(URN)을 갖지 않는 경우로, 이렇게 쓰는 일은 지양해야 한다.</p>
<h1 id="2-url-uniform-resource-locator">2. URL (Uniform Resource Locator)</h1>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/a6111687-6635-47c2-8ae5-d76a3a61e039/image.png" /></p>
<blockquote>
</blockquote>
<p>URL은 실제 주소와, 자원의 위치(API 엔드포인트 등)를 포함한 개념이다.</p>
<h2 id="정의-1">정의</h2>
<p>URL은 인터넷 상의 자원이 실제로 어디에 위치하고 있는지를 나타내는, URI의 가장 흔한 형태이다. 
우리가 일반적으로 사용하는 웹 사이트의 주소라고 할 수 있겠다. 이는 실제 자원의 위치와 쿼리 스트링 등 자세한 정보를 포함한다.
특징
따라서 URL은 자원의 위치를 나타내며, 해당 자원에 접근하기 위해 필요한 모든 정보(프로토콜, 도메인 이름, 경로 등)를 포함한다.
예시</p>
<pre><code>&quot;http://www.example.com/products?category=books&amp;name=john&quot;</code></pre><blockquote>
</blockquote>
<ol>
<li>프로토콜 : &quot;http://&quot;는 웹에서 자원을 어떻게 접근할지 정하는 프로토콜을 나타낸다.<blockquote>
</blockquote>
</li>
<li>도메인 이름: &quot;<a href="http://www.example.com&quot;%EC%9D%80">www.example.com&quot;은</a> 인터넷 상의 위치(도메인)를 가리킨다.<blockquote>
</blockquote>
</li>
<li>경로: &quot;/products&quot;는 서버 내에서 특정 자원이나 페이지를 가리키는 경로다.</li>
</ol>
<h2 id="url-형태">URL 형태</h2>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/c80076d6-4a99-4273-b092-bdb9b72f9749/image.png" />
&lt;mdn-url-all 출처 : 하단&gt;</p>
<ol>
<li>스키마(Scheme): 이 부분은 URL이 시작되는 부분으로, 사용되는 프로토콜을 지정하는 위치이다.</li>
</ol>
<p>예를 들면 'http', 'https', 'FTP', 'mailto' 등이 있다. 스키마의 경우 콜론(:)과 두 개의 슬래시//로 끝난다.</p>
<pre><code>ex) https://
</code></pre><ol start="2">
<li>권한(Authority):이 부분은 스키마 다음에 오며, 일반적으로 기업 또는 일반 사용자가 가지고 있는 도메인의 정보, 호스트, 그리고 포트로 구성된다.</li>
</ol>
<p>Domain Name의 경우 인터넷의 실제 IP 주소와 연결하기 위한 도메인 주소 이름을 많이 활용하며, 문자로 연결하는 이유는 기억하기 쉬운 이름의 도메인을 가지고 있기 위함이다. IP 주소로도 도메인 네임의 주소를 대체할 수 있다.</p>
<p>기업 또는 일반 사용자의 정보는 선택 사항이며 '@'기호 뒤에 호스트 이름이 나온다. 거의 대부분 도메인 네임을 통하여 IP 값을 문자 주소로 변경해서 서비스를 운영한다.</p>
<ol start="3">
<li>포트(Port): 리소스에 접근하기 위해 사용되는 포트 번호를 나타낸다. 일반적으로는 80 포트를 활용하며, 기본 포트 번호로 사용하는 경우 대부분 생략한다.</li>
</ol>
<ol start="4">
<li>리소스 경로(Path): 이 부분은 권한(Authority) 뒤에 오며, 서버 상의 리소스 위치를 나타낸다. 경로는 슬래시(/)로 구분된 여러 세그먼트로 구성될 수 있는데 경로의 경우. 상대 경로와 절대 경로로 나뉜다. 자세한 내용은 하단에서 정리할 예정이다.</li>
</ol>
<ol start="5">
<li>매개변수(Parameters): Path URL의 일부는 쿼리 문자열로, '?' 기호 다음에 오며, 키-값 쌍의 형태를 가지고 있다. 이 매개변수 값은 리소스에 대한 추가 정보를 제공하거나, 동적 리소스를 지정하는데 사용된다.</li>
</ol>
<ol start="6">
<li>쿼리 문자열(Query String): 추가적인 매개변수를 전달하기 위해 사용되는 문자열이다. 일반적으로 'key=value' 형태로 작성되며, 여러 개의 쿼리 문자열은 '&amp;'로 구분한다.</li>
</ol>
<ol start="7">
<li>앵커(Anchor): 이 부분은 URL의 끝 부분에 올 수 있으며, 매개변수가 끝난 뒤. '#' 기호 다음에 오는 식별자로 보면 된다. 앵커는 웹 페이지 내의 특정 부분으로 특정 섹션 또는 헤딩을 가리키는데 사용된다. 또한 앵커는 프래그먼트 식별자라고도 불린다.</li>
</ol>
<ol start="8">
<li>프래그먼트(Fragment): 문서나 리소스 내의 특정 부분을 조금 더 상세하게 가리키기 위해 사용되는 식별자이며, 주로 HTML 문서의 앵커 태그와 연결되어 사용된다.</li>
</ol>
<ul>
<li><p>URL을 사용하는 방법은 매우 간단하다. 웹 브라우저의 주소 표시줄에 URL을 입력하면, 크롬 브라우저 or 웹 브라우저는 해당 URL이 가리키는 자원을 가져오면서 요청/응답 과정을 진행하고 끝난다. 하이퍼링크를 클릭하면 URL 주소로 이동하게 되는 것을 생각하면 활용 방법은 매우 간단한 편이다.</p>
</li>
<li><p>URL은 파일을 다운로드 하는 경우.(흔하게 FTP 등) 웹 기반의 API를 사용할 때도 중요한 역할을 한다. 이들 모든 경우에서 URL은 웹 브라우저에서 원하는 시기에 원하는 존재하는 자료를 찾을 때 필요한 주소를 제공해주는 것이다.</p>
</li>
</ul>
<h2 id="절대-url와-상대-url">절대 URL와 상대 URL</h2>
<p>절대 URL(Absolute URL)은 전체 경로를 포함하여 특정 리소스의 정확한 위치를 나타내는 것을 말한다.</p>
<p>절대 URL은 프로토콜(ex: http, https)과 도메인(ex : <a href="http://www.google.com">www.google.com</a>  or <a href="http://www.naver.com">www.naver.com</a>  or <a href="http://www.tistory.com">www.tistory.com</a> )을 포함한다.</p>
<p><strong>활용 예시)</strong></p>
<pre><code>
https://도메인주소//images/img.jpg
</code></pre><p>상대 URL(Relative URL)은 현재 페이지를 기준으로 한 리소스의 위치를 상대적으로 지정하는 것을 말한다.</p>
<p>상대 URL의 경우 절대 URL과 도메인을 생략하고, 경로만 나타내는데 대부분 상대 경로의 경우. 웹 페이지 간의 내부 링크 또는 현재 웹 페이지에서 다른 리소스를 참조할 때 사용된다.</p>
<p><strong>활용 예시)</strong></p>
<pre><code>
/images/img.jpg
</code></pre><blockquote>
</blockquote>
<p>[여담]</p>
<blockquote>
</blockquote>
<p>절대 URL 경로와 상대 URL 경로를 개발자 분들은 검색하면서도 종종 볼 수 있을 것이다. 본인이 경험했던 사례는 벨로그(velog)에서 Github 아이콘에 주소를 거시는 분들이 상대 경로로 걸어야 하는데 절대 경로로 <a href="https://github~~//https~%EB%A5%BC">https://github~~//https~를</a> 걸어서 없는 주소로 나오는 경우가 있는 것을 봤다.</p>
<h3 id="절대-url경로와--상대-url-경로는-어디에-쓰이는가">절대 URL경로와  상대 URL 경로는 어디에 쓰이는가?</h3>
<blockquote>
</blockquote>
<p>절대 URL은 항상 특정 리소스의 정확한 위치를 지정하거나 변하지 않는 특정 리소스를 참조하는 경우 활용된다.</p>
<blockquote>
</blockquote>
<p>상대 URL은 절대 URL 경로보다 상대적으로 짧고, 유연하다.</p>
<blockquote>
</blockquote>
<p>상대 경로는 동일한 웹 페이지가 다른 도메인 또는 서브도메인에 배치되어도 링크가 올바르게 동작할 수 있도록 해주는데 내부 사이트내에서 내비게이션 구조를 구축하고, 파일 및 폴더의 관계를 유지하면서 효율적인 링크 관리하는 데 활용된다.</p>
<blockquote>
</blockquote>
<p>정리하면 절대 URL 경로는 새로운 페이지에 초점을 상대 URL 경로는 홈페이지 내부에서 이동의 초점이 잡혀있다. 각각 정리하면 다음과 같이 정리할 수 있다.</p>
<h3 id="절대-url">절대 URL</h3>
<ul>
<li><p>외부 웹 사이트로 링크 : 위 언급된 것처럼 다른도메인이나 서브 도메인으로 링크를 생성할 때 절대 URL을 활용한다. 타 사이트로 이동하거나 외부 이미지 동영상을 포함하는 경우 절대 URL를 활용한다 보면 된다.</p>
</li>
<li><p>외부 서비스와의 통합 : 외부 서비스(API)와 데이터를 주고 받을 경우 절대 URL을 활용하여 API 엔드 포인트를 지정하게 된다.</p>
</li>
</ul>
<p>외부 서비스 API의 경우 자체적인 API도 있지만 아래의 포스팅에서 나오는 것처럼. 과거에 비해서 기업에서 API를 제공하는 경우도 많아졌고, 클라우드도 다양한 서비스들이 있기 때문에 기초 이론을 모른다면 참조용으로 읽어보자. (카카오 맵과 같이 카카오 API나 생략해도 무관하다)</p>
<h3 id="상대-url">상대 URL</h3>
<ul>
<li><p>내부 페이지 간의 링크 : 가장 흔하게 활용될 수 있는 것이다. 태그의 값을 이용해서 내부 웹페이지 링크를 생성 후. 개발시 주로 상대 URL을 사용한다.</p>
</li>
<li><p>폴더 및 파일 관리 : 웹 사이트의 내부 구조에서 폴더 및 파일 간의 관계를 유지하면서 링크를 관리하는 경우 상대 URL을 활용하는 경우가 있다.</p>
</li>
</ul>
<p>상대 URL을 통해서 관리하는 것들은 이미지, 스타일 시트, 스크립트 파일 등과 같이 리소스를 참조하는 링크를 생성할 때 상대 URL을 활용하는데 위에 활용 예시로 나온 것처럼 '/images/img.jpg'의 경로로 활용된다.</p>
<blockquote>
</blockquote>
<p>실제 웹 서비스에서 내비게이션 및 페이지 간 이동에서 주로 상대 경로를 활용할 것 같지만 외부 리소스나 외부 서비스로 제공되는 API들과 통합할 때는 절대 URL을 사용하는 경향도 있다. 하지만 이것은 절대적이지 않고, 설계에 따라서 다양한 접근 방식에서 구조와 목적 그리고 URL 선택을 하게 된다.</p>
<h1 id="3-urn-uniform-resource-name">3. URN (Uniform Resource Name)</h1>
<h2 id="정의-2">정의</h2>
<p>URN은 (Uniform Resource Name)의 줄임말이다. URI의 표준 포맷 중 하나로, 이름으로 리소스를 특정하는 URI이다.</p>
<h2 id="특징">특징</h2>
<p>자원의 이름을 제공하지만, 그 자원이 어디에 위치해 있는지, 어떻게 접근해야 하는지에 대한 정보는 포함하지 않는다는 특징을 가진다.</p>
<h2 id="예시">예시</h2>
<p>URN은 자원의 위치가 바뀌더라도 해당 자원을 일관되게 식별할 수 있도록 설계되었다.
예를 들어, ISBN 번호는 책의 URN으로 사용할 수 있을 것이다.
이러한 경우  http와 같은 프로토콜을 제외하고 리소스의 name을 가리키는데 사용된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/fa1fc3d2-404b-464f-8b33-298a66bf3fc3/image.png" /></p>
<h1 id="4-url과-urn의-차이">4. URL과 URN의 차이</h1>
<p>그렇다면 URN과 URL의 차이는 뭐죠? (핵심)
URL은 어떻게 리소스를 얻을 것이고 자원이 어디에 있는지 명시하는 URI이다.
URN은 이러한 명시 없이 경로와 리소스 자체를 특정하는 것을 목표로 하는 URI이다.</p>
<h1 id="5-uri-url-urn의-예시">5. URI, URL, URN의 예시</h1>
<h2 id="1-urn">1) URN</h2>
<blockquote>
</blockquote>
<p>URN(Uniform Resource Name)은 자원의 고유한 이름을 식별하기 위해 사용되며, 자원의 위치(URL)와는 관계없이 특정 자원을 고유하게 나타냅니다. 여기서는 몇 가지 일반적인 URN의 예시를 소개합니다.</p>
<h3 id="1-isbn-국제-표준-도서-번호">1. ISBN (국제 표준 도서 번호)</h3>
<p>설명: 책을 고유하게 식별하기 위해 사용되는 번호입니다.
예시: urn:isbn:978-3-16-148410-0
이 URN은 특정 책의 ISBN 번호를 통해 자원을 식별합니다. 책이 어디에 위치하든지, 이 번호를 통해 동일한 책을 식별할 수 있습니다.</p>
<h3 id="2-issn-국제-표준-연속-간행물-번호">2. ISSN (국제 표준 연속 간행물 번호)</h3>
<p>설명: 학술지, 잡지 등의 연속 간행물을 고유하게 식별하는 번호입니다.
예시: urn:issn:1234-5678
특정 저널이나 잡지를 식별하는 URN입니다.</p>
<h3 id="3-urn">3. URN</h3>
<p>(National Bibliography Number)
설명: 각 나라의 국립 도서관이 책, 보고서 등 출판물을 고유하게 식별하기 위해 사용하는 번호입니다.
예시: urn:nbn:de:bvb:19-146642
독일의 특정 출판물을 식별하는 URN입니다.</p>
<h3 id="4-uuid-범용-고유-식별자">4. UUID (범용 고유 식별자)</h3>
<p>설명: 소프트웨어 개발 등에서 객체나 자원을 고유하게 식별하기 위해 사용되는 128비트 식별자입니다.
예시: urn:uuid:123e4567-e89b-12d3-a456-426614174000
UUID는 네트워크 상에서 객체를 고유하게 식별할 수 있습니다.</p>
<h3 id="5-urn">5. URN</h3>
<p>(법률 문서 식별)
설명: 특정 법률 문서를 고유하게 식별하기 위해 사용되는 URN입니다. 특히, 법률 문서가 전자적으로 저장될 때 주로 사용됩니다.
예시: urn:lex:eu:council:directive:2010-03-09;2010-019
유럽 연합의 특정 법률 문서를 고유하게 식별하는 URN입니다.</p>
<h3 id="6-swrl-semantic-web-rule-language">6. SWRL (Semantic Web Rule Language)</h3>
<p>설명: Semantic Web 기술에서 규칙을 고유하게 식별하기 위해 사용될 수 있는 URN입니다.
예시: urn:swrl:ruleset:example-rule
특정 규칙 집합을 고유하게 나타내는 URN입니다.
이러한 예시들은 URN이 자원의 물리적 위치와 관계없이, 자원 자체를 고유하게 식별하는 데 중점을 둔다는 특징을 보여줍니다. URN은 위치가 변해도 항상 동일한 자원을 식별할 수 있어야 하는 경우에 사용됩니다.</p>
<h2 id="2-url">2) URL</h2>
<blockquote>
</blockquote>
<p>URL(Uniform Resource Locator)은 웹에서 자원의 위치를 명시하여 접근할 수 있도록 해주는 주소입니다. 일반적으로 자원의 위치와 이를 접근하는 방법(프로토콜)을 포함합니다. URL의 예시는 다음과 같습니다:</p>
<h3 id="1-웹-페이지-url">1. 웹 페이지 URL</h3>
<p>설명: 특정 웹 사이트의 페이지를 가리키는 URL로, 브라우저에서 입력하면 해당 페이지로 이동할 수 있습니다.
예시: <a href="https://www.example.com/index.html">https://www.example.com/index.html</a>
https: 프로토콜 (HTTP 보안 프로토콜)
<a href="http://www.example.com">www.example.com</a>: 도메인 이름 (서버 주소)
/index.html: 자원의 경로 (파일 경로)</p>
<h3 id="2-파일-다운로드-url">2. 파일 다운로드 URL</h3>
<p>설명: 서버에서 파일을 다운로드하기 위한 URL입니다.
예시: <a href="https://www.example.com/downloads/file.zip">https://www.example.com/downloads/file.zip</a>
이 URL을 통해 파일을 다운로드할 수 있습니다.</p>
<h3 id="3-ftp-url">3. FTP URL</h3>
<p>설명: FTP 서버에서 파일에 접근하거나 다운로드할 수 있는 URL입니다.
예시: <a href="ftp://ftp.example.com/file.txt">ftp://ftp.example.com/file.txt</a>
ftp: FTP 프로토콜
ftp.example.com: FTP 서버 주소
/file.txt: 파일 경로</p>
<h3 id="4-이미지-url">4. 이미지 URL</h3>
<p>설명: 웹에 업로드된 이미지를 가리키는 URL입니다.
예시: <a href="https://www.example.com/images/photo.jpg">https://www.example.com/images/photo.jpg</a>
이 URL을 통해 웹 페이지에서 이미지를 로드하거나 직접 볼 수 있습니다.</p>
<h3 id="5-이메일-작성-url-mailto">5. 이메일 작성 URL (mailto)</h3>
<p>설명: 이메일 주소를 가리키는 URL로, 이메일 클라이언트를 열어 이메일을 작성할 수 있습니다.
예시: mailto:<a href="mailto:someone@example.com">someone@example.com</a>?subject=Hello
mailto: 이메일 전송을 위한 프로토콜
<a href="mailto:someone@example.com">someone@example.com</a>: 이메일 주소
?subject=Hello: 이메일 제목을 설정하는 쿼리 파라미터</p>
<h3 id="6-검색-url">6. 검색 URL</h3>
<p>설명: 검색 엔진에서 특정 키워드로 검색 결과를 얻기 위한 URL입니다.
예시: <a href="https://www.google.com/search?q=example">https://www.google.com/search?q=example</a>
https: HTTP 보안 프로토콜
<a href="http://www.google.com">www.google.com</a>: 검색 엔진의 도메인
/search?q=example: 검색 쿼리를 포함한 경로와 파라미터</p>
<h3 id="7-동영상-url">7. 동영상 URL</h3>
<p>설명: 온라인 동영상 플랫폼에서 동영상 콘텐츠를 가리키는 URL입니다.
예시: <a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ">https://www.youtube.com/watch?v=dQw4w9WgXcQ</a>
이 URL을 통해 특정 동영상에 접근할 수 있습니다.
이러한 URL들은 자원의 위치를 명확하게 지정하며, 이를 통해 특정 자원에 접근할 수 있도록 해주는 역할을 합니다. URL은 자원의 주소 역할을 하면서 자원에 어떻게 접근할지에 대한 정보(프로토콜)도 제공합니다.</p>
<h2 id="3-urn도-아니고-url도-아닌-uri">3) URN도 아니고 URL도 아닌 URI</h2>
<h3 id="1-data-uri">1. Data URI</h3>
<p>Data URI는 실제 파일 데이터를 인코딩하여 URI로 직접 포함합니다. 자원의 위치를 가리키는 것이 아니라 자원 자체의 내용을 포함하는 방식입니다.
예시: data:image/png;base64,iVBORw0KGgoAAAANSUhEUg...
이 URI는 이미지 파일을 Base64로 인코딩하여 웹 페이지에서 직접 사용됩니다. 즉, 이 URI는 자원의 위치가 아닌, 자원의 데이터 자체를 포함합니다.</p>
<h3 id="2-mailto-uri">2. Mailto URI</h3>
<p>mailto: URI는 이메일 주소를 통해 이메일을 작성할 수 있게 해주며, 자원의 위치가 아닌 이메일 주소를 식별합니다.
예시: mailto:<a href="mailto:example@example.com">example@example.com</a>
이 URI를 클릭하면 사용자의 이메일 클라이언트가 열리고, 지정된 주소로 이메일을 작성할 수 있는 화면이 나옵니다.</p>
<h3 id="3-tel-uri">3. Tel URI</h3>
<p>tel: URI는 전화번호를 다루며, 자원의 위치와는 무관하게 전화번호를 식별합니다.
예시: tel:+1234567890
전화번호로 전화를 걸기 위해 사용되는 URI로, 자원의 위치가 아닌 전화번호 자체를 가리킵니다.</p>
<h3 id="4-javascript-uri">4. JavaScript URI</h3>
<p>javascript: URI는 자원의 위치가 아닌, 실행할 JavaScript 코드를 포함합니다. 웹 브라우저에서 JavaScript 코드를 직접 실행할 수 있습니다.
예시: javascript:alert('Hello, World!');
이 URI를 사용하면 브라우저에서 JavaScript 코드를 실행하게 됩니다.</p>
<h3 id="5-ftp-비로그인-uri">5. FTP 비로그인 URI</h3>
<p>FTP URI 중 일부는 특정 서버나 파일 위치를 가리키는 URL로 볼 수 있지만, 비로그인 정보를 담고 있는 경우에는 단순히 특정 서비스 접근 권한만을 부여하는 URI로 사용할 수도 있습니다.
예시: <a href="ftp://anonymous@ftp.example.com">ftp://anonymous@ftp.example.com</a>
익명 사용자로 FTP 서버에 접속하는 데 사용되며, 특정 파일의 위치가 아니라 서버 접근 권한을 식별하는 용도로 사용될 수 있습니다.</p>
<h3 id="6-geo-uri">6. Geo URI</h3>
<p>geo: URI는 위치 정보를 표현하지만, 특정 웹 자원의 위치를 가리키지 않고, 지리적 좌표를 식별합니다.
예시: geo:37.7749,-122.4194
위도와 경도를 통해 특정 지리적 위치를 가리키며, 지도 서비스에서 사용할 수 있는 URI입니다.</p>
<p>이러한 예시들은 자원의 위치를 나타내기보다, 자원의 속성이나 동작을 정의하거나 식별하는 용도로 사용됩니다. URI가 반드시 자원의 위치(URL)를 가리키는 것만은 아니라는 점에서, 이와 같은 다양한 형태의 URI를 활용할 수 있습니다.</p>
<h2 id="url-과-urn-모두-포함">URL 과 URN 모두 포함</h2>
<blockquote>
</blockquote>
<p>URL과 URN의 특징을 모두 포함하는 URI는 일반적으로 자원의 <strong>위치(주소)</strong>와 <strong>이름(식별자)</strong>를 동시에 제공하는 경우입니다. 그러나 엄밀히 말해, URL과 URN은 구분되는 개념으로, 하나의 URI가 두 역할을 모두 완전히 충족하는 사례는 드물지만, 일부 URI는 URL과 URN의 성격을 모두 가지는 특성을 보일 수 있습니다.</p>
<p>예를 들어, 다음과 같은 경우를 생각해볼 수 있습니다:</p>
<h3 id="1-doi-digital-object-identifier-uri">1. DOI (Digital Object Identifier) URI</h3>
<p>DOI는 학술 논문, 전자 문서 등의 디지털 콘텐츠를 고유하게 식별하는 시스템입니다. DOI URI는 자원을 고유하게 식별하는 특성(URN)과 함께, 자원의 위치(URL)로 리디렉션할 수 있는 기능을 제공합니다.
예시: <a href="https://doi.org/10.1000/182">https://doi.org/10.1000/182</a>
여기서 10.1000/182는 DOI 시스템에서 특정 자원을 고유하게 식별합니다. 이 DOI를 통해 접근하면 실제 자원이 위치한 웹 페이지로 리디렉션되므로, URL로도 기능할 수 있습니다.
특징: DOI URI는 고유한 식별자 역할을 하면서도, 웹 상의 자원 위치로 연결되는 동작을 수행합니다. 따라서 URN과 URL의 기능을 혼합한 형태로 볼 수 있습니다.</p>
<h3 id="2-isbn을-사용한-웹-리소스-uri">2. ISBN을 사용한 웹 리소스 URI</h3>
<p>ISBN은 책의 고유 식별자(URN)로 사용되지만, 이를 URL 형태로 구현하여 웹 자원의 위치로 접근할 수 있게 할 수도 있습니다.
예시: <a href="https://example.com/book/isbn/9783161484100">https://example.com/book/isbn/9783161484100</a>
여기서 9783161484100은 특정 책의 ISBN(URN)입니다. 이 ISBN을 기반으로 책의 정보를 제공하는 웹 페이지의 위치를 지정한 URL로 활용한 것입니다.
특징: ISBN이 자원의 고유 이름(URN)으로 쓰이지만, 이를 웹 자원의 위치로 활용할 수 있어 URL의 특성을 가집니다.</p>
<h3 id="3-ark-archival-resource-key">3. ARK (Archival Resource Key)</h3>
<p>ARK는 디지털 및 물리적 자원을 영구적으로 식별하기 위해 설계된 URI 형식으로, 식별자(URN) 역할과 함께 위치 정보를 제공할 수 있는 URL 역할을 동시에 수행할 수 있습니다.
예시: <a href="http://ark.example.org/ark:/12025/654xz321">http://ark.example.org/ark:/12025/654xz321</a>
ARK URI는 특정 자원을 고유하게 식별하면서도, 웹을 통해 자원에 접근할 수 있는 주소(URL)로 동작할 수 있습니다.
특징: 자원의 고유한 식별자 기능과 위치로서의 동작이 결합된 형태입니다.
이러한 URI들은 <strong>고유한 자원 식별 기능(URN의 특징)</strong>과 <strong>웹 상에서 접근할 수 있는 위치 정보(URL의 특징)</strong>를 동시에 갖추고 있어, URL과 URN의 특성을 혼합한 사례로 이해할 수 있습니다.</p>
<p>출처 : </p>
<p><a href="https://okeybox.tistory.com/426">https://okeybox.tistory.com/426</a></p>
<p><a href="https://csg1353.tistory.com/124">https://csg1353.tistory.com/124</a></p>