<h1 id="git연동-해제하기">Git연동 해제하기</h1>
<p> 
Git연동 해제는 간단하다.
 </p>
<h3 id="1-github-아이디-연동-해제">1. GitHub 아이디 연동 해제</h3>
<p> 
Settings -&gt; Version Control -&gt; Github 들어가서 아이디 지우기</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/6efe7de7-6b89-4928-97cb-8abdbcfb8ca0/image.PNG" /></p>
<p>해당 프로필 클릭 -&gt; &quot; - &quot; 클릭 -&gt; Apply 클릭
하면 계정과 연동이 해제되고
 
 </p>
<h3 id="2-intellij-프로젝트의-연동-해제-시">2. Intellij 프로젝트의 연동 해제 시</h3>
<p> 
File -&gt; Settings -&gt; 검색창에 mapping을 치거나 아래 사진과 같이 Directory Mappings를 찾아서 아래사진과 같이
 </p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/08c124a1-90e1-42a7-bdc0-d110ca6fc38d/image.png" /></p>
<p>VCS가 Git으로 연결되어 있는 것을 &lt;none&gt;으로 바꿔주면 연동 해제 완료!!</p>
<h3 id="3-로컬-저장소-삭제">3. 로컬 저장소 삭제</h3>
<p>IntelliJ 하단에서 Terminal을 실행한 후, Git Bash를 선택하고 해당 프로젝트의 디렉토리에서 다음의 명령어를 입력한다.</p>
<pre><code>rm -rf .git</code></pre><p>해당 명령어가 프로젝트 내의 .git 폴더 및 하위 폴더를 삭제하여 로컬 저장소를 삭제해주는 명령어이다.
 
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/35980a91-df9c-4fc3-8491-470a7e0671f6/image.png" /></p>
<p>이 때, .gitignore 파일은 삭제되지 않는데 설정을 유지하는 것이 좋다. 만약 없다면 git init 명령어를 실행시키면 되지 않을까 싶다. 문제는 git init 명령어를 실행하면 이전의 .git폴더가 그대로 복구되는 것 같아서 다른 프로젝트의 .gitignore 파일을 복사해오는 것도 방법인 것 같다. 그래서 나는 git init 명령어는 실행하지 않고 gitignore 파일을 복사해오는 후자의 방법으로 해결했다.</p>
<h1 id="intellij-터미널을-git-bash로-변경하기">IntelliJ 터미널을 Git Bash로 변경하기</h1>
<ol>
<li>Settings 창 열기<ul>
<li>File -&gt; Settings 또는 Ctrl+Alt+S 단축키 사용</li>
</ul>
</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/ce841003-cc92-4e4e-aace-4343bbc1fc61/image.png" /></p>
<ol start="2">
<li>왼쪽 메뉴 중 Tools 선택</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/4737a30b-16ef-4695-8c17-091d3061dd54/image.png" /></p>
<ol start="3">
<li>하위 메뉴 중 터미널 선택</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/f3a9e591-8f71-4008-b09e-c0e39fb1518a/image.png" /></p>
<ol start="4">
<li>Shell path 변경
Applications Settings -&gt; Shell Path -&gt; 아래와 같이 입력</li>
</ol>
<blockquote>
</blockquote>
<p>&quot;&lt;Git Shell 경로&gt;&quot; -login -i
예시) &quot;C:\Program Files\Git\bin\sh.exe&quot; -login -i</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/4efbc28d-7678-44a2-a926-5ca804ac0e72/image.png" /></p>
<ol start="5">
<li>Terminal 열어서 변경 확인</li>
</ol>
<blockquote>
</blockquote>
<p>출처</p>
<blockquote>
</blockquote>
<p><a href="https://imkdk.tistory.com/24">https://imkdk.tistory.com/24</a> [개발관:티스토리]</p>
<blockquote>
</blockquote>
<p><a href="https://its-easy.tistory.com/27">https://its-easy.tistory.com/27</a></p>
<blockquote>
</blockquote>
<p><a href="https://devlifetestcase.tistory.com/84">https://devlifetestcase.tistory.com/84</a> [개발 인생 TestCase:티스토리]</p>