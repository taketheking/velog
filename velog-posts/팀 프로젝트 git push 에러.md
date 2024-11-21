<p>팀 프로젝트를 만들어서 clone 하고 새로운 branch를 만들어서 push하는 과정에서 </p>
<blockquote>
</blockquote>
<p>remote: Permission to news-feed-project/fluffygram.git denied to taketheking. unable to access '<a href="https://github.com/news-feed-project/fluffygram.git/'">https://github.com/news-feed-project/fluffygram.git/'</a>: The requested URL returned error: 403</p>
<p>라는 문제가 발생했다.</p>
<p>오류 메시지에 따르면 taketheking이라는 계정이 news-feed-project/fluffygram.git 리포지토리에 대한 권한이 없다. 먼저, 리포지토리 소유자나 관리자에게 taketheking 계정이 해당 리포지토리에 대해 읽기/쓰기 권한이 있는지 확인해 보라고 되어 있다.</p>
<p>즉, github에서 개인 접근 코드(자격 증명용)이 문제라는 것이다.</p>
<p>이전에 한 프로젝트에서 토큰을 생성하여 사용한 적이 있는데 그것 때문에 권한이 제한이 된 것이라 예상된다.</p>
<p>이 문제는 해결은 3가지 방법이 있다. 각 환경에 따라 적용하면 될 것이다.
하지만 터미널에서 적용했다고 intellij에서도 되지는 않는다.</p>
<ol>
<li>터미널 이용</li>
</ol>
<pre><code class="language-bash">git config user.name &quot;new name&quot;
git config credential.username &quot;new name&quot;</code></pre>
<ol start="2">
<li>컴퓨터 설정 이용
<img alt="" src="https://velog.velcdn.com/images/take_the_king/post/8017231d-55fa-4caf-aafa-08fdae18c535/image.png" /></li>
</ol>
<blockquote>
</blockquote>
<p>윈도우용
창 버튼 &gt; 자격 증명 관리자 &gt; Windows 자격 증명 &gt; 일반 자격 증명을 클릭하세요.
다음으로, Github 키를 제거하거나 편집합니다.</p>
<blockquote>
</blockquote>
<p>Mac용 🍎
1 - Finder에서 Keychain Access 앱을 검색합니다.
2 -  Keychain Access에서 github.com을 검색하세요.
3 - github.com의 &quot;인터넷 비밀번호&quot; 항목을 찾으세요.
4 - 해당 항목을 편집하거나 삭제하세요.</p>
<ol start="3">
<li>github 토큰 이용<blockquote>
</blockquote>
github page &gt; profile 이미지 클릭 &gt; settings &gt; 맨아래로 스크롤다운하여 왼쪽에 Developer setting &gt; personal access token &gt; token(classic) &gt; generate new token
또는
intellij에서 안내해주는대로 토큰 설정하여 추가</li>
</ol>
<p>참고 : 
<a href="https://stackoverflow.com/questions/47465644/github-remote-permission-denied">https://stackoverflow.com/questions/47465644/github-remote-permission-denied</a></p>