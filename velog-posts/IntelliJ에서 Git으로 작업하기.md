<h1 id="1-git-시작하기">1. Git 시작하기</h1>
<hr />
<h2 id="1-git-설치하기">1) Git 설치하기</h2>
<p><a href="https://git-scm.com/%EB%A1%9C">https://git-scm.com/로</a> 접속한 뒤, Download for Windows or mac를 클릭해 설치를 시작합니다.</p>
<p>설치 옵션은 검색해서 찾아보자.</p>
<p><a href="https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%84%A4%EC%B9%98">https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%84%A4%EC%B9%98</a></p>
<p><a href="https://codingapple.com/unit/git-install-windows-mac/">https://codingapple.com/unit/git-install-windows-mac/</a></p>
<h2 id="2-github-가입하기">2) Github 가입하기</h2>
<p>github.com 사이트에 들어가서 가입하면 된다. 모르면 검색</p>
<h2 id="3-intellij에-git-설정">3) IntelliJ에 Git 설정</h2>
<h3 id="단계별-설정">단계별 설정</h3>
<ol>
<li><p><strong>Git 설치 확인:</strong> 커맨드 라인 또는 터미널에서 <code>git --version</code>을 실행하여 Git이 설치되어 있는지 확인합니다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/86605fb6-2b8c-458a-9b46-4673161a6fbd/image.png" /></p>
</li>
<li><p><strong>유저 세팅 하기ㅣ</strong> 터미널(종류 상관x)에서 <code>git config --global user.email &quot;user_email&quot;</code>와 <code>git config --global user.name &quot;user_name&quot;</code>를 입력하여 유저 정보를 등록한다. 그리고 <code>git config --list</code>를 통해 확인한다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/39b792bd-2ac5-4dda-9405-7a6520d5bd09/image.png" /></p>
</li>
</ol>
<ol start="3">
<li><strong>IntelliJ IDEA 설정:</strong> <code>File</code> &gt; <code>Settings</code> (Windows/Linux) 또는 <code>IntelliJ IDEA</code> &gt; <code>Preferences</code> (macOS)로 이동하여 <code>Version Control</code> &gt; <code>Git</code>에서 Git 실행 파일의 경로를 확인하거나 설정합니다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/0a4f8235-742e-4152-aebd-cd7a8ca6ae6f/image.PNG" /></li>
</ol>
<ol start="4">
<li><strong>리포지토리 초기화:</strong> IntelliJ IDEA에서 <code>VCS</code> &gt; <code>Import into Version Control</code> &gt; <code>Create Git Repository</code>를 선택하여 현재 프로젝트 폴더를 Git 리포지토리로 초기화합니다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/ae42393b-fcad-47e6-ac17-47760197c191/image.png" /></li>
</ol>
<ol start="5">
<li><p><strong>Git 계정 로그인 설정:</strong></p>
<ul>
<li><p><strong>계정 연결:</strong> <code>Settings</code>/<code>Preferences</code>에서 <code>Version Control</code> &gt; <code>GitHub</code> 또는 <code>Git</code> &gt; <code>Accounts</code>를 선택합니다. 여기서 GitHub, GitLab, Bitbucket 등 다양한 Git 호스팅 서비스 중 하나를 선택하고, <code>Add Account</code> 버튼을 클릭하여 로그인 정보를 입력합니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/4e0ecbdd-547e-46b8-830a-99f6051b88a0/image.png" /></p>
</li>
</ul>
</li>
</ol>
<pre><code>- **Token:** 계정 전체 권한을 주지 않고 token 을 통해 지정한 권한만 가지도록 로그인 정보를 제한적으로 추가 할 수 있습니다.
   ![](https://velog.velcdn.com/images/taketheking/post/b53ac304-acf8-4c5c-b23e-58b7a4f9d7ac/image.png)</code></pre><p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/f3a525a7-9e25-47ee-82b7-72681a6227bc/image.png" /></p>
<p>이 단계를 완료하면 IntelliJ IDEA에서 Git 리포지토리를 관리하고, Git 호스팅 서비스에 안전하게 연결하여 작업을 수행할 준비가 완료됩니다. 계정 로그인 설정을 통해 푸시, 풀, PR 생성 등의 작업을 보다 원활하게 수행할 수 있게 됩니다.</p>
<h1 id="2-intellij-프로젝트를-gitgithub에-연동하기">2. Intellij 프로젝트를 Git/GitHub에 연동하기</h1>
<blockquote>
</blockquote>
<p>프로젝트를 생성하여 Github에 직접 올리는 과정은 프로젝트의 책임자 급에서 진행하는 과정이다. 회사에서 공동으로 작업하는 환경에서는 할 일이 없다. </p>
<hr />
<h2 id="1-프로젝트를-git-repository로-설정하기">1) 프로젝트를 Git Repository로 설정하기</h2>
<p> </p>
<h3 id="방법-1---프로젝트-생성시-초기화">방법 1 - 프로젝트 생성시 초기화</h3>
<p> 리포지토리 초기화: 프로젝트 폴더에서 Git 리포지토리를 시작하려면, IntelliJ IDEA의 프로젝트 생성시 Create Git Repository 옵션을 선택합니다. 이렇게 하면 선택한 디렉토리에 .git 폴더가 생성되며, 이 폴더는 모든 버전 관리 정보를 저장합니다.
 <img alt="" src="https://velog.velcdn.com/images/taketheking/post/7bf79592-c9cf-4745-887c-ffde6af87b9a/image.png" /></p>
<h3 id="방법-2---프로젝트-생성-이후에-git-연결">방법 2 - 프로젝트 생성 이후에 Git 연결</h3>
<p>처음 Intellij에 들어가서 프로젝트를 생성하면 나오는 메뉴바이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/ccc83964-5a02-4914-8e57-911e509dc542/image.png" /></p>
<p>저기서 VCS -&gt; Git 으로 변경되어야 한다! -&gt; VCS 클릭!!</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/6d00c5d2-3d31-4526-bd53-3723899c4546/image.png" />
Enable Version Control Integration.. 클릭 후 -&gt; Git을 선택 OK를 눌러주면 VCS -&gt; Git으로 바뀌게 됨!</p>
<h2 id="2-intellij에-github-아이디-추가하기">2) Intellij에 Github 아이디 추가하기</h2>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/44dd65da-b7ba-4d38-b39c-0c55239be42f/image.png" />
그 후 File -&gt; Settings -&gt; Version Control -&gt; GitHub 으로 들어가 Add account나 + 버튼을 눌러 아이디를 추가해 준다.</p>
<p>추가로 github 홈페이지에서 인증을 하면</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/6efe7de7-6b89-4928-97cb-8abdbcfb8ca0/image.PNG" />
이렇게 연동이 완료된다.</p>
<h2 id="3-github-연결-후-commit-및-remote-repository원격-레포지토리로-share">3) Github 연결 후 Commit 및 Remote Repository(원격 레포지토리)로 SHARE</h2>
<p>Git연결을 완료했으면 그대로 프로젝트를 Git 원격 레포로 커밋을 한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/04d650b1-ae66-47b8-927d-4d03e8ea7544/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/8f286fe6-4661-4bd5-840d-2a5e6fd8fe06/image.png" />
Share Project on GitHub 클릭! -&gt; Commit 내역 작성(Description) 후 Share</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/fcf685a1-751e-4dc7-adb9-c927eb04e642/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/e4530477-dd65-4dd5-80e6-6cd3557adaac/image.png" /></p>
<p>원격 레포에 들어갈 파일들 체크하여 Add 눌러주면 -&gt; 친절하게 successfully shared project on Github이라고 뜬다!!
 
 </p>
<h2 id="4-github에서-프로젝트가-잘-들어갔는지-확인">4) Github에서 프로젝트가 잘 들어갔는지 확인</h2>
<p>Github으로 들어가 본인 repo로 들어가면 아래 사진과 같이 정상적으로 git에 등록된 것을 볼 수 있다.</p>
<blockquote>
<p>참고</p>
</blockquote>
<ul>
<li><a href="https://velog.io/@taketheking/github-%EA%B3%B5%EB%8F%99%EC%9E%91%EC%97%85-%EC%84%A4%EC%A0%95">github의 프로젝트에 다른 사람에게 권한을 부여하는 방법</a></li>
</ul>
<h2 id="5-git연동-해제하기">5) Git연동 해제하기</h2>
<p>Git연동 해제는 간단하다.</p>
<h3 id="1-github-아이디-연동-해제">1. GitHub 아이디 연동 해제</h3>
<p>Settings -&gt; Version Control -&gt; Github 들어가서 아이디 지우기
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/ab705ee5-717e-4189-81ea-62ac397c0e85/image.png" /></p>
<p>해당 프로필 클릭 -&gt; &quot; - &quot; 클릭 -&gt; Apply 클릭
하면 계정과 연동이 해제되고</p>
<h3 id="2-intellij-프로젝트의-연동-해제-시">2. Intellij 프로젝트의 연동 해제 시</h3>
<p>File -&gt; Settings -&gt; 검색창에 mapping을 치거나 아래 사진과 같이 Directory Mappings를 찾아서 아래사진과 같이
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/5361b6bd-bbe7-4bfa-a317-adf15e3f3631/image.png" /></p>
<p>VCS가 Git으로 연결되어 있는 것을 으로 바꿔주면 연동 해제 완료!!</p>
<h3 id="3-로컬-저장소-삭제">3. 로컬 저장소 삭제</h3>
<p>IntelliJ 하단에서 Terminal을 실행한 후, Git Bash를 선택하고 해당 프로젝트의 디렉토리에서 다음의 명령어를 입력한다.</p>
<p><code>rm -rf .git</code>
해당 명령어가 프로젝트 내의 .git 폴더 및 하위 폴더를 삭제하여 로컬 저장소를 삭제해주는 명령어이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/e5f6e953-da4a-4b2b-9652-8325ba03333e/image.png" /></p>
<p>이 때, .gitignore 파일은 삭제되지 않는데 설정을 유지하는 것이 좋다. 만약 없다면 git init 명령어를 실행시키면 되지 않을까 싶다. 문제는 git init 명령어를 실행하면 이전의 .git폴더가 그대로 복구되는 것 같아서 다른 프로젝트의 .gitignore 파일을 복사해오는 것도 방법인 것 같다. 그래서 나는 git init 명령어는 실행하지 않고 gitignore 파일을 복사해오는 후자의 방법으로 해결했다.</p>
<h1 id="3-git-프로젝트-가져오는-2가지-방법">3. git 프로젝트 가져오는 2가지 방법</h1>
<blockquote>
<p>일반적으로 프로젝트를 진행할 때는 github에서 프로젝트를 clone해와서 작업을 한다. </p>
</blockquote>
<hr />
<h2 id="방법-1-주소를-통해-git-프로젝트-가져오기">방법 1) 주소를 통해 git 프로젝트 가져오기</h2>
<h3 id="1-git-주소-복사하기">1. Git 주소 복사하기</h3>
<p>Clone 하려는 Git 주소를 복사합니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/4f74f75c-bf69-4150-9459-cdc8e0fec63d/image.png" /></p>
<p> </p>
<h3 id="2-1-or-2-2-선택">2-1 or 2-2 선택</h3>
<h4 id="2-1-file---new---project-from-version-control-선택">2-1. File - New - Project from Version Control... 선택</h4>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/2e215603-839d-44ea-9e9d-602c710280c9/image.png" /></p>
<h4 id="2-2-get-from-version-control-선택">2-2. Get from Version Control 선택</h4>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/6b53ba4a-30f6-455d-b8d6-d0292d6df5d1/image.png" /></p>
<blockquote>
</blockquote>
<p>참고 </p>
<ul>
<li>매번 Intellij 실행 시 최근 프로젝트가 실행이 되어서 프로젝트를 선택할 수 있는 기본 화면이 안나온다면 아래와 같이 설정 변경<blockquote>
</blockquote>
</li>
</ul>
<ol>
<li>File &gt; Settings ( 윈도우 기준 단축키 : Ctrl + Alt + s)<blockquote>
</blockquote>
</li>
<li>Reopen last project on startup 해제<blockquote>
</blockquote>
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/185fa271-0cf4-4e60-acf8-68e3def60235/image.png" /></li>
</ol>
<p> </p>
<h3 id="3-repository-url-탭">3. Repository URL 탭</h3>
<p>Version control: Git
URL:  복사한 Git 주소를 붙여넣기 합니다.
Directory: Git 소스를 Clone할 디렉토리를 지정합니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/5821d02f-de19-4b9e-882f-e997b0903136/image.png" /></p>
<p> </p>
<h3 id="4-지정한-디렉토리에-git-소스가-clone되었습니다">4. 지정한 디렉토리에, Git 소스가 Clone되었습니다.</h3>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/62278d84-88bf-4170-a1d6-30901cf6f134/image.png" /></p>
<p> 
 
 </p>
<h2 id="방법-2-github에-로그인하여-내-repository-clone-하기">방법 2) GitHub에 로그인하여 내 Repository Clone 하기</h2>
<hr />
<h3 id="1-file---new---project-from-version-control">1. File - New - Project from Version Control...</h3>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/3a3a2ae3-2739-4836-9449-f54bee98150e/image.png" /></p>
<p> </p>
<h3 id="2-github---log-in-via-github">2. GitHub - Log in via GitHub</h3>
<p>GitHub에 로그인하여, 내 Repository를 Clone 할 수 있습니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/e747f4fc-88a4-4130-8fe9-b400f0223557/image.png" /></p>
<p> </p>
<h3 id="3-인증">3. 인증</h3>
<p>IntelliJ가 내 계정에 접근하여 Repository를 읽어올 수 있도록 인증합니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/09bc3b3d-962d-44f8-8f08-0e21d520147f/image.png" /></p>
<p> </p>
<h3 id="4-repository-선택">4. Repository 선택</h3>
<p>인증이 끝나면, IntelliJ에서 내 Repository 목록을 조회할 수 있습니다.
Clone 하려는 Repository를 선택하고, Clone 버튼을 클릭합니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/bba1cd9f-bf3c-4121-866f-1b5b88fa2670/image.png" /></p>
<h1 id="4-기본-git-명령어">4. 기본 Git 명령어</h1>
<hr />
<h2 id="1-add-staging">1) Add (Staging)</h2>
<blockquote>
<p>파일을 수정하거나 새 파일을 생성한 후, 이러한 변경 사항을 다음 커밋에 포함시키기 위해 Git &gt; Add를 사용하여 스테이징 영역에 추가합니다.</p>
</blockquote>
<p>Git은 Index(Staging Area)에 있는 파일만 commit 할 수 있다. 새로운 파일을 생성해보면 Add File to Git이라는 확인창이 출력된다.</p>
<p>Add 버튼을 클릭하면 Index에 추가되고, 파일명이 녹색으로 바뀐다.
Cancel 버튼을 클릭하면 Index에 추가되지 않고, 파일명이 빨간색으로 바뀐다.
기존에 있던 파일의 내용을 수정하면 Index에 추가되고, 파일명이 하늘색으로 바뀐다.</p>
<p>Index에 추가되지 않은 파일을 다시 Index에 추가하려면 좌측의 파일목록 트리에서 파일 우클릭 -&gt; Git -&gt; Add를 클릭하면 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/189584ce-81b3-4eb4-92e3-23c37ee68b75/image.png" /></p>
<h2 id="2-commit">2) Commit</h2>
<blockquote>
<p>스테이징 영역에 추가된 변경 사항을 로컬 리포지토리에 기록합니다. IntelliJ IDEA의 Commit 창을 통해 커밋 메시지를 입력하고, Commit 버튼을 클릭합니다. 또한 Commit and Push 옵션을 사용하면 커밋 후 변경 사항을 바로 원격 리포지토리로 푸시할 수 있습니다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/0cca59e3-51fb-42a1-87b7-7bacd5fe38e0/image.png" /></p>
<p>Commit 버튼(우측 상단의 녹색 체크모양의 버튼)을 클릭하면 좌측 상단에 Commit 가능한 파일 목록이 표시된다.</p>
<p>Changed 하위에 있는 파일이 Git Index에 추가된 파일이고,</p>
<p>Unversioned Files 하위에 있는 파일은 Git Index에 추가되지 않은 파일이다.</p>
<p>Commit 할 파일을 체크하고, 파일 목록 아래에 Commit 메시지를 작성한 후 그 밑에 있는 Commit 버튼을 클릭하면 Local Branch에 Commit이 완료된다.</p>
<p>그 옆의 Commit and Push... 버튼을 클릭하면 Remote Branch까지 한방에 올라간다.</p>
<p>Local Branch를 굳이 활용하지 않는다면 Commit and Push...가 더 편한 방법이다.</p>
<p>우측 상단의 Push 버튼(녹색 화살표 모양 버튼)을 클릭하면 Index에 추가된 파일이 Remote Branch로 올라간다.</p>
<p>※ IntelliJ에서는 Git Index에 추가되지 않은 파일도 체크하고 Commit 버튼을 클릭하면 자동으로 Git Index에 추가 후 Local Branch에 Commit 한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/1a67e9ab-185d-4814-b5cd-3b462889a7d1/image.png" /></p>
<h2 id="3-push">3) Push</h2>
<blockquote>
<p>로컬 리포지토리의 커밋을 원격 리포지토리에 반영하기 위해 Git &gt; Push를 사용합니다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/3bc099e3-ba65-4981-85cd-88603f8cc4d7/image.png" /></p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/87c14b6b-9e3e-49a6-94d1-f47f87f2d3b5/image.png" /></p>
<ul>
<li><p>좌측의 브랜치 명은 local 브랜치명 -&gt; push할 원격저장소의 브랜치명 이다.</p>
</li>
<li><p>브랜치명 아래는 커밋 메시지.</p>
</li>
<li><p>우측에는 어떤 파일들이 커밋 되었는지 확인 할 수 있다. </p>
</li>
</ul>
<h2 id="4-pull">4) Pull</h2>
<blockquote>
<p>원격 리포지토리의 최신 변경 사항을 로컬 리포지토리로 가져오려면 Git &gt; Pull을 사용합니다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/00c21c5c-84a0-433b-8177-c9e729ab24e4/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/80f1aaea-e2e1-4d87-b0e7-bc474dad7638/image.png" /></p>
<blockquote>
</blockquote>
<ul>
<li>Push와 Pull을 하다보면 Remote Repository와 local Repository의 내용이 달라서 충돌이 일어날 수 있다. 충돌은 아래에서 설명한다.
충돌이 나면 해당 코드를 수정해서 다시 싱크를 맞추면 된다.</li>
</ul>
<h1 id="5-branch를-설정하는-방법">5. Branch를 설정하는 방법</h1>
<blockquote>
</blockquote>
<p>Git을 통해 Repository를 생성 및 clone을 하면 협업 중에 오류를 방지하기 위해 여러 Branch로 나누어서 작업했다가 다시 합치는 과정을 거친다.</p>
<blockquote>
</blockquote>
<p>Git Branch를 생성하고, 관리하는 방법은 각 회사/팀마다 다르다.</p>
<blockquote>
</blockquote>
<p>예시)
▷ master(상용서버에 배포된 버전),
▷ develop(개발서버에 배포된 버전)
▷ feature1(기능1 개발중인 Branch)
▷ feature2(기능2 개발중인 Branch)</p>
<hr />
<h2 id="1-branch-확인하기">1) Branch 확인하기</h2>
<p>Git Repository를 생성하면 기본적으로 master Branch가 생성되어 있다.</p>
<p>좌측 하단의 Git 탭을 클릭하면 Branch 목록과 Git History를 볼 수 있고,
우측 하단의 master라고 되어있는 부분을 클릭해도 Branch 목록을 볼 수 있다.
우측 하단의 master는 현재 Branch의 이름을 의미한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/f17b912d-4f7d-41e6-a13d-b10964f2e43d/image.png" /></p>
<h2 id="2-branch-생성하기">2) Branch 생성하기</h2>
<p>좌측 하단의 +버튼을 클릭하거나 우측 하단의 현재 Branch명을 클릭하면 나오는 팝업창에서 + New Branch를 클릭하면 새로운 Branch를 생성할 수 있다.</p>
<p>또는, Git &gt; Branches &gt; New Branch를 선택하여 새 브랜치를 만듭니다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/a8b9f293-8ed0-4590-9643-5db5e3f17bf2/image.png" /></p>
<p>Branch를 확인해보면 Local에만 생겼고, Remote에는 생성되지 않았다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/37bd8e67-aecf-43cb-80ea-29b6c5f31376/image.PNG" /></p>
<p>우측 상단의 Push 버튼(녹색 화살표 모양 버튼)을 클릭해서 Push를 완료하면 Remote에도 develop Branch가 생성된다.</p>
<h2 id="3-branch-전환하기-checkout">3) Branch 전환하기 (Checkout)</h2>
<blockquote>
<p>현재 작업하는 Branch에서 다른 Branch로의 이동을 Checkout이라고 한다.</p>
</blockquote>
<p>만약에 어떤 기능을 구현하여 Git에 등록했다고 하면, 지금까지 변경사항은 하나의 Branch에만 적용되어있다. 
그래서 </p>
<p>1) 다른 Branch에 변경사항을 적용하려고 하거나, 
2) 현재 Branch가 아닌 다른 Branch에서 작업을 하려고 할 때 
Branch를 전환한다.</p>
<p>다른 브랜치로 전환하려면, Git &gt; Branches에서 원하는 브랜치를 선택하고 Checkout 옵션을 사용합니다.
또는 Branch 목록에서 master를 우클릭 또는 클릭 후 checkout을 클릭하면 develop Branch에서 master Branch로 이동된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/5fdb3aa6-3191-46c0-8d62-65edf18d16f3/image.png" /></p>
<blockquote>
</blockquote>
<p>※ Local Branch로 Checkout 하든 Remote Branch로 Checkout 하든 똑같이 Local Branch로 Checkout이 되기 때문에 checkout 할때는 Local과 Remote를 신경쓸 필요는 없다.</p>
<h3 id="--smart-checkout--force-checkout">- Smart Checkout / Force Checkout</h3>
<blockquote>
<p>Local에서 소스를 수정하고, Commit하지 않은 상태에서 다른 Branch로 Checkout할 때 충돌이 발생하면 아래와 같은 팝업창이 나올 수도 있다. Force Checkout을 클릭하면 Local에서의 수정된 내용을 날려버리는 것이고, Smart Checkout은 Local에서의 변경 내용을 Git Index에 보관하는 것이다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/415fd698-280b-4c16-af7f-5510ecf967ba/image.png" /></p>
</blockquote>
<h2 id="4-브랜치-병합하기-merge">4) 브랜치 병합하기 (Merge)</h2>
<blockquote>
<p>브랜치에서의 작업이 완료되면, 해당 브랜치를 다른 브랜치(예: master 또는 main)에 병합할 수 있습니다. 병합하려는 브랜치로 전환(Checkout)한 다음, Git &gt; Merge를 선택하고, 병합할 브랜치를 지정합니다.</p>
</blockquote>
<blockquote>
<p>회사나 팀 프로젝트에서는 병합하기 이전에 Pull Request를 작성해서 변경사항을 공유하고 리뷰를 요청한다. 그리고 최종적으로 결정권자가 merge를 결정한다.</p>
</blockquote>
<p>예시 1)</p>
<blockquote>
<p>Develop 브랜치 쪽으로 master 브랜치를 합치는 예시
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/2f96ab04-bd8c-46d2-860b-2f8d535374b2/image.PNG" /></p>
</blockquote>
<p>예시 2)</p>
<blockquote>
<p>master 브랜치에 kang 브랜치를 합치려고 하면 master 브랜치로 checkout을 하고 merge 'kang' into 'master' 를 하면 된다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/a6efd629-545e-4b8a-b266-2f371d8f2afb/image.png" /></p>
</blockquote>
<p>Merge가 성공적으로 이루어 졌다면, intelliJ 우측 상단, 혹은 Git/push 클릭. 
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/d3d6b707-81ce-460d-b1ca-f71beef3c8a7/image.png" /></p>
<p>원격 저장소에 업데이트 되었는지 확인. </p>
<blockquote>
<p>다양한 옵션을 선택할 수 있습니다.</p>
</blockquote>
<p>--no-ff : fast-foward 관계라 하더라도 강제로 merge commit을 생성하고 병합
--ff-only : 대상 브랜치가 fast-foward 관계에 있는 경우 새로운 커밋을 생성하지 않음
--squash : 강제 병합
-m : 메시지 포함
--no-commit : 메시지 미포함
--no-verify : 머지 커밋 메시지 무시
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/02a9e577-1ad8-48ad-8987-ae74538bdb19/image.png" /></p>
<h1 id="6-conflict-해결">6. Conflict 해결</h1>
<p>Push, Pull, Merge, Update Project 등을 할 때 같은 파일의 동일한 부분을 서로 다르게 수정되어 있으면 Conflict가 발생한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/0a70ab38-f08d-4262-a9e9-6f785759c061/image.PNG" /></p>
<p>Conflict가 발생하면 아래와 같은 팝업이 호출되는데 왼쪽과 오른쪽을 참고해서 가운데를 적당하게 수정해주면 가운데의 소스로 적용이 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/3ef57e36-4f90-4aa9-9bd0-83fef7ca46ea/image.png" /></p>
<p>충동 해결하고 다시 push를 해야한다.</p>
<h1 id="7-git-로그log히스토리history-확인">7. Git 로그(Log)=히스토리(History) 확인</h1>
<ul>
<li><strong>커밋 로그 확인:</strong> IntelliJ IDEA의 <code>Version Control</code> 창에서 <code>Log</code> 탭을 선택하면, 프로젝트의 커밋 히스토리를 시각적으로 볼 수 있습니다. 이 로그를 통해 커밋 사이를 탐색하고, 브랜치의 히스토리를 확인하며, 다양한 브랜치 간의 관계를 이해할 수 있습니다. (또는 <code>Git</code> &gt; <code>Show Git Log</code> )
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/f06add7e-edeb-49dd-a4ad-1cf0d318c54e/image.png" /></li>
</ul>
<ul>
<li><p><strong>변경 사항 비교:</strong> <code>Log</code> 탭에서 특정 커밋을 선택하면, 해당 커밋과 이전 커밋 간의 변경 사항을 비교할 수 있는 <code>Diff</code> 창이 열립니다. 이를 통해 변경된 코드를 세밀하게 검토할 수 있습니다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/737065f2-1c5f-496c-a0ed-1499ef8114c0/image.png" /></p>
<p>이 단계를 통해 IntelliJ IDEA에서 Git을 사용한 기본적인 버전 관리 방법을 익힐 수 있으며, 프로젝트의 소스 코드 관리를 효과적으로 수행할 수 있습니다.</p>
</li>
</ul>
<h1 id="8-pr-pull-request-작성-및-관리-📤">8. PR (Pull Request) 작성 및 관리 📤</h1>
<blockquote>
<p>IntelliJ IDEA를 사용하여 PR(Pull Request)을 작성하고 관리하는 기능은 협업 중인 프로젝트에서 코드 변경 사항을 공유하고 리뷰를 요청하는 중요한 과정입니다. IntelliJ IDEA는 GitHub, GitLab과 같은 주요 Git 호스팅 서비스와 통합되어 있어, IDE 내에서 직접 PR을 생성하고 관리할 수 있습니다. 이 과정을 통해 개발자는 코드 변경 사항에 대한 피드백을 받고, 최종적으로 메인 프로젝트 브랜치에 병합될 수 있도록 합니다.</p>
</blockquote>
<h2 id="githubgitlab과-intellij-idea-연동">GitHub/GitLab과 IntelliJ IDEA 연동</h2>
<ul>
<li><strong>계정 연결:</strong> 먼저, IntelliJ IDEA에 GitHub 또는 GitLab 계정을 연결해야 합니다. <code>File</code> &gt; <code>Settings</code> (Windows/Linux) 또는 <code>IntelliJ IDEA</code> &gt; <code>Preferences</code> (macOS)로 이동한 다음, <code>Version Control</code> &gt; <code>GitHub</code> 또는 <code>Git</code> &gt; <code>Accounts</code>에서 계정을 추가합니다.</li>
<li><strong>인증:</strong> 인증 방법으로는 OAuth, Personal Access Token(PAT), 또는 SSH 키가 있습니다. IntelliJ IDEA는 이 과정을 안내하여 계정 연동을 완료할 수 있게 돕습니다.</li>
</ul>
<h2 id="pull-request-설정">Pull Request 설정</h2>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/c2111380-43fd-46f4-836e-acc7e95ac64d/image.png" />
Settings &gt; Branches로 이동합니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/a20ebf36-f8a2-4a64-93df-1a7c1f90ac3a/image.png" />
위처럼 체크하고 코드 리뷰할 인원을 설정해줍니다.
(위에서 체크한 수 만큼의 인원이 승인해줘야 branch를 merge할 수 있습니다)</p>
<h3 id="pull-request-형성하기">pull request 형성하기</h3>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/d9815aab-b031-44a8-bf30-e554bd8564a2/image.png" />
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/2d32ffbe-f9a4-401a-842b-dc1c396c89f4/image.png" />
위 버튼을 누르면 B→A로 병합을 요청하는 pull request를 형성 할 수 있습니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/5518e56d-a186-40cb-aafe-a73d46de4588/image.png" />
[1] 어느 branch에서 어느 branch로 merge할 지 정해줄 수 있습니다!</p>
<p>[2] 해당 branch 에 대한 내용을 작성합니다</p>
<p>[3] 어떤 팀원에게 review를 받을 지 정할 수 있어요.
    (설정하지 않으면 팀원 중 누구에게나 review를 받아서 merge 할 수 있습니다)</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/76892f94-08ac-47c1-9db9-2a916bae0dd8/image.png" />
위에서 정해준 수 만큼 code review가 없다면 merge를 할 수 없습니다…!</p>
<h3 id="코드-리뷰-후-병합하기">코드 리뷰 후 병합하기</h3>
<p>코드 리뷰는 코드를 작성한 사람이 아닌 다른 팀원들이 해야 하는 부분입니다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/0e9ba48e-cacc-4532-9492-53786adfa84a/image.png" />
팀원들끼리 코드를 공유하고 수정해야 할 사항을 남길 수도 있어요
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/0a7369e9-6232-4c58-9fba-43e96f2b1d37/image.png" />
위처럼 팀원이 코드리뷰 후 approval을 해주면 아래 버튼으로 merge를 할 수 있습니다.
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/ad93de7e-8b2e-481b-aec9-ff1aa08f2cc6/image.png" />
merge를 완료하였다면 Delete branch버튼을 통해 branch를 삭제할 수 있습니다.</p>
<h2 id="pr-생성하기">PR 생성하기</h2>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/57a3400f-fb89-4aef-8348-1c39f6defc45/image.png" /></p>
<ul>
<li><strong>변경 사항 커밋 및 푸시:</strong> PR을 생성하기 전에, 변경 사항을 로컬 브랜치에 커밋하고 해당 브랜치를 원격 리포지토리에 푸시해야 합니다.</li>
<li><strong>PR 생성:</strong> IntelliJ IDEA에서는 GitHub 또는 GitLab 프로젝트에 직접 PR을 생성할 수 있습니다. <code>VCS</code> &gt; <code>Git</code> &gt; <code>Create Pull Request</code>를 선택하여 PR 생성 창을 엽니다. 여기서는 대상 브랜치 선택, PR 제목 및 설명 입력, 리뷰어 지정과 같은 옵션을 설정할 수 있습니다.</li>
<li><strong>리뷰 요청:</strong> PR이 생성된 후, 지정된 리뷰어에게 코드 리뷰를 요청할 수 있습니다. 리뷰어는 코드 변경 사항에 대한 코멘트를 달거나, 승인 또는 요청 사항을 제출할 수 있습니다.</li>
</ul>
<h2 id="pr-검토-및-머지-과정">PR 검토 및 머지 과정</h2>
<ul>
<li><strong>PR 검토:</strong> 리뷰어는 PR에 대해 코멘트를 남기고, 변경 사항에 대한 의견을 제시할 수 있습니다. 개발자는 이 피드백을 바탕으로 코드를 개선하고 추가 커밋을 PR에 포함시킬 수 있습니다.</li>
<li><strong>머지:</strong> 모든 리뷰어의 승인을 받고, 빌드 및 테스트가 성공적으로 완료되면, PR을 메인 브랜치에 머지할 수 있습니다. IntelliJ IDEA에서는 GitHub 또는 GitLab 웹 인터페이스를 통해 머지 과정을 완료할 수 있습니다.</li>
</ul>
<h2 id="pr-후-청소-작업">PR 후 청소 작업</h2>
<ul>
<li><strong>브랜치 정리:</strong> PR이 머지된 후, 더 이상 필요하지 않은 기능 브랜치는 삭제할 수 있습니다. IntelliJ IDEA에서는 머지된 브랜치를 쉽게 정리할 수 있는 옵션을 제공합니다.</li>
</ul>
<p>IntelliJ IDEA에서 PR을 작성하고 관리하는 과정은 코드 품질을 높이고, 팀 내에서 효과적인 코드 리뷰 및 협업을 가능하게 합니다. IDE 내에서 직접 이러한 과정을 관리할 수 있다는 것은 개발자에게 큰 이점을 제공하며, 프로젝트의 생산성과 효율성을 향상시킵니다.</p>
<h2 id="github-홈페이지에서-pull-request-하기">github 홈페이지에서 pull Request 하기</h2>
<blockquote>
<p>기능 개발 후 해당 브랜치에서 <code>git push</code> 명령어를 입력하고, <strong>github repository</strong>로 이동하면 다음과 같은 알림이 뜹니다. 이때 <strong>Compare &amp; pull request</strong> 버튼을 클릭합니다.
(만약 뜨지 않는다면 git push가 제대로 됐는지 확인하시거나, <strong>Pull requests 탭</strong>을 클릭 후 <strong>New pull request</strong> 버튼을 클릭해보세요)</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/b0fd8770-f828-4f7c-a08e-3f7b6fc56691/image.png" /></p>
<h3 id="base-브랜치--compare-브랜치-확인">base 브랜치 &amp; compare 브랜치 확인</h3>
<p>아래 이미지에서 <strong>1번 붉은 박스 부분</strong>을 자세히 보시면 <strong>base: main</strong> 이라고 되어있습니다. PR을 생성할 때 <strong>base 브랜치</strong>를 <strong>main</strong>으로 설정한다는 의미인데요. <strong>base 브랜치</strong>란 수정된 코드가 합쳐질 최종 브랜치라는 의미입니다.</p>
<p>아래 이미지의 <strong>2번 붉은 박스 부분</strong>을 보시면 <strong>compare: feature/login</strong> 이라고 되어있는데요. 수정된 코드가 있는 브랜치를 의미합니다. compare라고 되어있는 이유는 base 브랜치인 main과의 비교 대상이기 때문입니다.</p>
<p>다시 정리하자면, <strong>feature/login</strong>(compare) 브랜치에서 수정된 코드를 <strong>main</strong>(base) 브랜치로 합치기 위한 PR을 생성할 것이라는 뜻입니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/dcf89f31-fd7a-4ee5-b7a3-4767c35aa0c3/image.png" /></p>
<ul>
<li>제목 입력 및 Create pull request 클릭</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/d10917d2-e49a-4256-8cd9-4e7e0a11b952/image.png" /></p>
<ul>
<li>PR 생성 완료 및 <strong>file changed 탭</strong> 클릭 후 변경된 코드 확인 가능</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/3a206022-5a4c-4ef4-9b9f-7f7f5377c859/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/bb583d61-3704-4dcd-85e5-a90b4995fb7d/image.png" /></p>
<ul>
<li>문제가 없을 경우 <strong>merge</strong> 하기 (<strong>Merge pull request</strong>)</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/4a401cdc-352f-43a3-8565-46014d140dbb/image.png" /></p>
<ul>
<li><strong>PR (Pull request)는 왜 만들어야 하나요?</strong>
PR을 생성하면 github repository에서 코드를 최종적으로 합치기 전 (main으로 merge하기 전) 코드를 점검할 수 있습니다.</li>
</ul>
<h3 id="merge-전-conflict-해결">merge 전 Conflict 해결</h3>
<p>아래 이미지의 붉은 박스처럼 <strong>Merge pull request</strong> 버튼이 비활성화 됐죠? 이처럼 <strong>merge</strong> 전에 <strong>Conflict</strong>가 났다면 Github에서 <strong>merge</strong>를 할 수 없습니다. </p>
<p><strong>Conflict</strong>가 발생했다는 건 팀원이 <strong>같은 위치의 같은 코드를 수정했기 때문</strong>에 최종적으로 어떻게 변경해야 하는지 Github에서 물어보는 것입니다. 이럴 경우 어떻게 해결하는지 확인해볼게요!</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/376a6fcf-ee1b-49c9-b021-3dd6f87c33b7/image.png" /></p>
<ol>
<li>기능 개발한 브랜치에서 <code>git pull origin main</code> 명령어로 메인 코드를 가져온다. </li>
</ol>
<pre><code class="language-jsx">    $ git pull origin main</code></pre>
<ol start="2">
<li><p>conflict 난 코드 확인</p>
<p> 좌측 아이콘(Source Control) → 파일 선택 → conflict난 부분 코드 확인</p>
</li>
</ol>
<ol start="3">
<li><p>conflict 가 발생한 코드 수정</p>
<p> 최종적으로 바꾸고 싶은 코드로 변경합니다.</p>
</li>
</ol>
<ol start="4">
<li>다시 <code>git add</code> &amp; <code>git commit</code> &amp; <code>git push</code> 명령어를 입력합니다.</li>
</ol>
<pre><code class="language-jsx">    $ git add .
    $ git commit -m &quot;메세지&quot;
    $ git push origin &lt;브랜치명&gt;</code></pre>
<ol start="5">
<li><p><strong>merge</strong> 한다. </p>
<p> 이제 Merge 버튼이 활성화됐네요! </p>
</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/445429bd-481e-4ec9-9324-a9811ac8161c/image.png" /></p>
<h1 id="9-그-외의-명령어">9. 그 외의 명령어</h1>
<h2 id="1-fetch">1) Fetch</h2>
<p>Fetch는 전체 Remote Branch의 변경내용이 있는지 확인하는 기능이다.</p>
<p>테스트를 위해 develop Branch로 Checkout한 후 feature1 Branch를 생성하고, Push 해준다.</p>
<p>다시 develop Branch로 Checkout한 후 feature2 Branch를 생하고, Push 해준다.</p>
<p>※ 새로 생성된 Branch는 현재 Checkout된 Branch를 기준으로 생성되는 것임을 기억해야 한다.</p>
<p>Github 웹페이지에서 feature1/feature2 Branch의 소스를 직접 수정하고, Commit 한다.</p>
<p>그리고 IntelliJ 화면 상단 메뉴에서 Git -&gt; Fetch를 클릭한다.</p>
<p>feature1/feature2 Remote Branch의 History는 Github 웹페이지에서 수정된 내역이 추가되어 있고,</p>
<p>feature1/feature2 Local Branch 우측에는 아래쪽으로 향한 파란색 화살표 아이콘이 생긴 것을 확인할 수 있다.</p>
<p>내려받을 소스가 있다는 의미이다.</p>
<p>Fetch는 변경사항이 있는 Remote Branch의 History를 업데이트 해주고, Local Branch는 내려받을 소스가 있다고 알려주는 기능이라고 볼 수 있다.</p>
<h2 id="2-update-project">2) Update Project...</h2>
<p>현재 Checkout된 Branch를 Fetch 하고, Remote Branch를 Local Branch로 Merge 해주는 기능이다.</p>
<h2 id="3-commit-취소">3) Commit 취소</h2>
<p>Git History에서 아직 Push되지 않은 이력을 우클릭 한 후 Undo Commit...을 클릭하면 Commit 취소가 가능하다.</p>
<p> <img alt="" src="https://velog.velcdn.com/images/taketheking/post/803df383-30da-4f8f-99a7-c234cc5489c8/image.png" /></p>
<p>  <img alt="" src="https://velog.velcdn.com/images/taketheking/post/8ed98af6-88dc-4f0c-afdf-d8fb67a79e00/image.png" /></p>
<p>(1) Undo Commit
[ Undo Commit ] 명령을 수행하면 선택한 커밋을 제거 및 커밋 기록까지 삭제합니다.
즉, 작업 트리(Working Tree)를 이전 상태로 되돌리게 됩니다. 해당 명령은 로컬 저장소(Local Repository)에서만 커밋을
취소하는 명령이며 원격 저장소(Remote Repository)에는 영향을 주지 않습니다.</p>
<blockquote>
<p>인텔리제이 - Undo Commit 기능을 git 명령어인 git reset 명령어를 사용하여 구현됩니다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/761d4c2b-3076-4a32-acbe-18e8c3b1577c/image.png" /></p>
<p>(2) Revert Commit
위 [ Undo Commit ]을 보면 커밋을 취소함과 동시에 커밋 내역까지 삭제하는 것을 볼 수 있습니다.
반면, [ Revert Commit ] 은 기존 커밋 내역을 그대로 유지하는 동시에 해당 커밋을 되돌리기 위해 새로운 커밋을 생성합니다.
쉽게 말하자면 커밋 취소할 내역을 그대로 남기고, 해당 커밋의 작업 내역을 취소(이전 상태로 되돌리는 것)하는
새로운 커밋을 생성한다고 말할 수 있습니다.
물론 작업 트리(Working Tree)에도 Revert Commit을 사용한 기록에 남게 됩니다.</p>
<blockquote>
<p>인텔리제이 - Revert Commit 기능을 git 명령어인 git revert 명령어를 사용하여 구현됩니다.</p>
</blockquote>
<p>출처: <a href="https://backendcode.tistory.com/323">https://backendcode.tistory.com/323</a> [무작정 개발:티스토리]</p>
<h2 id="4-rollback">4) Rollback</h2>
<p>수정된 내용을 가장 최근 History로 되돌릴 수 있다.</p>
<p>파일소스에서 우클릭 -&gt; Git -&gt; Rollback을 클릭하여 할수도 있고, 파일목록에서 여러개를 한꺼번에 선택하고 우클릭 -&gt; Git -&gt; Rollback을 클릭해서도 할 수 있다.</p>
<p>참고 </p>
<p><a href="https://its-easy.tistory.com/27">https://its-easy.tistory.com/27</a></p>