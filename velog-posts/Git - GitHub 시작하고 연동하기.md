<blockquote>
</blockquote>
<p><strong>intro</strong>
git은 체계적인 개발과 프로그램의 배포를 도와주는 형상 관리 도구, 버전 관리 시스템입니다. 소프트웨어 개발 단계에서 버전을 효과적으로 관리하고, 동시에 협업을 도와주는 도구입니다.</p>
<p>이번 기회에 git에 대해 확실히 정리하고 한다.</p>
<h1 id="1-git-설치하기">1. Git 설치하기</h1>
<blockquote>
<p><a href="https://git-scm.com/%EB%A1%9C">https://git-scm.com/로</a> 접속한 뒤, Download for Windows or mac를 클릭해 설치를 시작합니다.</p>
</blockquote>
<p>설치 옵션은 검색해서 찾아보자.</p>
<p><a href="https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%84%A4%EC%B9%98">https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%84%A4%EC%B9%98</a></p>
<p><a href="https://codingapple.com/unit/git-install-windows-mac/">https://codingapple.com/unit/git-install-windows-mac/</a></p>
<h1 id="2-github-가입하기">2. Github 가입하기</h1>
<blockquote>
<p>github.com 사이트에 들어가서 가입하면 된다. 모르면 검색 </p>
</blockquote>
<h1 id="3-github-연동하기">3. GitHub 연동하기</h1>
<blockquote>
<p>컴퓨터의 git과 github를 연동해야 된다. git의 저장소가 github라서 여러 git(프로그램이든 파일이든)을 github에 저장하기 위해 연동한다고 생각하면 된다.</p>
</blockquote>
<p>참고로 git 생성 및 GitHub 연동은 기업에서는 팀장급 이상의 직원들만 하는 경우가 많다.</p>
<h2 id="1-개인정보-설정하기">1) 개인정보 설정하기</h2>
<h3 id="1-터미널-키고-git설치-상태-확인">1. 터미널 키고 git설치 상태 확인</h3>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/721d7c34-4951-4803-a498-1bed08fda59b/image.png" /></p>
<p>터미널에 git --version 이라고 작성하면 위처럼 버전이 나오면 설치되어있는 상태이다.</p>
<h3 id="2-유저개인정보-세팅하기">2. 유저(개인정보) 세팅하기</h3>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/42fc21e0-f8cc-4597-8c4f-90872087ae1c/image.png" /></p>
<p>먼저 , </p>
<pre><code>
git config --global user.email &quot;user_email&quot;

git config --global user.name &quot;user_name&quot;
</code></pre><p>명령어를 통해 나의 email과 이름을 등록한다.</p>
<p>이것을 통해 컴퓨터에서 git 프로그램을 사용하는 사용자을 등록한 것이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/91c40ece-51c0-4c16-a203-3a51bef85855/image.png" /></p>
<p>잘 등록되었는지 확인하려면</p>
<pre><code>git config --list</code></pre><p>을 통해 user 정보를 확인하면 된다.</p>
<h2 id="2-repository-생성하기">2) Repository 생성하기</h2>
<ol>
<li><p>GitHub 메인 화면 좌측 상단의 Create repository나 우측 상단의 +를 눌러 레파지토리 생성 페이지로 이동합니다. </p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/36832001-914e-4d68-9aba-a13ac0bfd1a9/image.PNG" /></p>
</li>
</ol>
<ol start="2">
<li>필요한 정보를 입력합니다. (*필수)</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/d23df506-4c34-402b-80dd-997362256bdc/image.png" /></p>
<ol start="3">
<li>레파지토리 생성이 완료된 모습입니다. 상단의 url은 생성한 레파지토리의 주소입니다. 
이 주소를 이용해 나의 로컬 저장소와 레파지토리를 연동할 수 있습니다. 기억해 둡시다!</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/31d3c789-12c1-4053-9792-2db0f4e6a7a2/image.png" /></p>
<h2 id="3-repository-연동하기">3) Repository 연동하기</h2>
<ol>
<li>Visual Studio Code를 열어 상단의 Terminal → New Terminal을 선택합니다.</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/b9919bb2-07fd-4dd3-ad8d-03e9a37a45ac/image.png" /></p>
<ol start="2">
<li>하단 터미널창의 + 버튼을 눌러 Git Bash를 선택합니다.
(Select Default Profile을 통해 기본 터미널을 Git Bash로 지정할 수도 있습니다.)</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/75e06d93-8d97-4391-919c-3619a6f21284/image.png" /></p>
<h3 id="주의사항">주의사항</h3>
<blockquote>
</blockquote>
<p>인텔리제이는 cmd 터미널을 기본으로 사용하므로 터미널을 bash로 변경하거나 따로 git bash 프로그램을 실행시키고 파일 위치를 일치시키고 진행하면 된다. </p>
<blockquote>
</blockquote>
<p><a href="https://velog.io/@jonghne/IntelliJ-%ED%84%B0%EB%AF%B8%EB%84%90-Git-Bash%EB%A1%9C-%EB%B3%80%EA%B2%BD%ED%95%98%EA%B8%B0">https://velog.io/@jonghne/IntelliJ-%ED%84%B0%EB%AF%B8%EB%84%90-Git-Bash%EB%A1%9C-%EB%B3%80%EA%B2%BD%ED%95%98%EA%B8%B0</a></p>
<ol start="3">
<li>저장소에 업로드할 파일을 생성하고, 터미널에 git init을 입력해 초기화합니다.  <img alt="" src="https://velog.velcdn.com/images/taketheking/post/be6174b9-d897-4752-8f22-4ee12556613b/image.png" /></li>
</ol>
<ol start="4">
<li>이제 내 원격 저장소를 연결할 차례입니다. </li>
</ol>
<p><strong><code>git remote add origin [https://저장소url.git](https://저장소)</code></strong> 을 입력해 조금 전에 생성했던 저장소를 연동합니다. <strong><code>git remote -v</code></strong> 명령어를 통해 연결이 잘 되었는지 확인할 수 있습니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/c8615653-b02d-4db9-9a0b-0f99dd038c30/image.png" /></p>
<ol start="5">
<li><strong><code>git add .</code></strong>을 입력해 생성한 파일 <code>모두</code> working tree에 추가합니다.</li>
</ol>
<p><strong><code>git status</code></strong> 명령어를 통해 제대로 추가되었는지 확인할 수 있습니다. 
아직 commit되지 않은 파일이 있네요! </p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/83567d93-0475-45e0-87b7-2f36a681ae8c/image.png" /></p>
<ol start="6">
<li><strong><code>git commit -m &quot;커밋 메시지&quot;</code></strong>를 입력해 추가한 파일을 commit 합니다.</li>
</ol>
<p><strong><code>git status</code></strong> 명령어를 통해 다시 확인해 볼까요? </p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/6901a3dd-89db-41b6-ae7f-8b5cb0c059d8/image.png" /></p>
<pre><code>모든 파일이 커밋 되어 working tree가 비워졌음을 확인할 수 있습니다!</code></pre><blockquote>
<p>  💡 커밋 메시지는 자유롭게 입력할 수 있지만, 가급적 ‘무엇을’, ‘왜’ 했는지 알 수 있도록 작성하는 것이 좋습니다. 팀원들 간에 ‘Commit Message Convention’을 지정해 더 효율적인 협업을 할 수도 있습니다.</p>
</blockquote>
<ol start="7">
<li><strong><code>git push origin 브랜치 이름</code></strong> 명령어로 원격 저장소에 commit list를 업로드 합니다.
우리는 따로 새 branch를 생성하지 않았기 때문에 default branch인 master 브랜치에 
push를 하겠습니다.  </li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/e578ab6d-f193-462c-90ea-2888218108ac/image.png" /></p>
<blockquote>
<p>remote: Invalid username or password. 오류
github의 저장소가 프라이빗이거나 로그인이 풀렸을 때 나타나는 오류이다.
그래서 먼저 기존 원격 저장소를 해체하고 </p>
</blockquote>
<pre><code>$ git remote remove origin</code></pre><p>다시 연결한다.</p>
<pre><code>$ git remote add origin https://github.com/깃허브아이디/깃허브저장소명.git</code></pre><p>그리고 상태 확인</p>
<pre><code>git remote -v</code></pre><p>로컬 저장소의 원격 저장소 지정</p>
<pre><code>$ git push --set-upstream origin main</code></pre><p>upstream은 로컬 저장소와 연결되어 있는 원격 저장소를 말한다.
즉 로컬 저장소의 원격 저장소를 위에서 입력한 원격 저장소로 지정하여 push 하는 것이다.
이 명령을 입력하고 나면 push 할 때 git push만 입력하면 된다.</p>
<blockquote>
<p>error: src refspec main does not match any error: failed to push some refs to '<a href="https://github.com/.../...git">https://github.com/.../...git</a> 해결
터미널에서 master에서 main으로 바꾸기</p>
</blockquote>
<pre><code>git branch -m master main</code></pre><blockquote>
<p> ❓ <strong>branch?</strong> 
    <code>브랜치 branch</code>는 이름 그대로 나뭇가지처럼 나뉘어 뻗어나가는 Repository의 가지입니다. 기능에 맞춰 branch를 나누어 작업하고, 작업이 완료되면 합치는(<strong>Merge</strong>) 식으로 더욱 편한 작업을 할 수 있습니다.</p>
</blockquote>
<blockquote>
</blockquote>
<pre><code>❓ 저는 **master branch가 없는데요?**
main branch로 push해보세요! GitHub는 최근 ‘Black Lives Matter’ 운동에 발맞추어 기존의 default branch명인 `master`를 `main`으로 변경하였습니다. 이로 인해 `main` 브랜치가 default branch로 생성되어 있을 수 있습니다.</code></pre><ol start="8">
<li><p>이제 실제로 저장소에 파일이 업로드되었는지 확인해 볼까요?</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/96d80b22-0813-419e-a61a-e35a3dc8d77c/image.png" /></p>
</li>
</ol>
<pre><code>**성공적으로 commit 내역이 업로드된 것을 확인할 수 있습니다.
파일명이나 커밋 메시지를 클릭하면 더욱 자세한 내용을 볼 수 있습니다.** </code></pre><p> <img alt="" src="https://velog.velcdn.com/images/taketheking/post/249a6ffd-8907-4014-a627-61b09359c7db/image.png" /></p>
<blockquote>
</blockquote>
<pre><code>📎 이후로는 다음 세 가지만 기억하면 됩니다! 
`git add .` or `git add “파일명”`
`git commit -m 커밋메시지`
`git push origin 브랜치이름`</code></pre><hr />
<h1 id="organization">Organization</h1>
<blockquote>
<p>Organization은 일종의 단체 계정입니다. 개인 계정의 Repository에서도 저장소를 만들고, 다른 개발자를 초대해 협업을 할 수 있지만, 
Organization을 사용하면 좀 더 쉽게 단체에 속한 Repository와 멤버를 관리할 수 있습니다. </p>
</blockquote>
<h2 id="1--organization-생성하기">1. ### Organization 생성하기</h2>
<p>1) 상단의 <code>+</code> 항목 내의 New organization을 선택해주세요.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/2c12744b-f85c-410e-94fe-67a1b5656fb5/image.png" /></p>
<p>2) Create a free organization을 눌러 무료 단체 계정을 생성합니다. </p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/052c12b4-4637-4339-b1e7-c8bf23f92acd/image.png" /></p>
<p>3) 필수 항목을 순서대로 작성하고, Next를 눌러 진행해 주세요.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/3415710e-403a-4c7e-9b02-9cdd61237a23/image.png" /></p>
<p>4) Organization에 초대할 사용자의 이메일을 입력하고, Complete setup 버튼을 눌러주세요. 
Skip this step 버튼을 통해 이 단계를 건너뛰고 나중에 진행할 수도 있습니다. </p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/f2b1afc3-c87c-47dd-b92e-57e7a2816ed3/image.png" /></p>
<p>5) 다음은 설문 항목입니다! 따로 입력 없이 Submit 버튼을 통해 단계를 넘어갈 수 있습니다. </p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/394ed59b-7a7b-4dcc-88e5-c7f9935864e5/image.png" /></p>
<p>6) organization이 생성되었습니다! 
이후 Create a new repository 버튼을 통해 organization 내에 새 저장소를 생성할 수 있습니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/9c63575c-e959-401e-a9f9-6d86da2302ad/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/a06e0187-282e-4893-b770-6b8514d8180c/image.png" /></p>
<blockquote>
<p>organization 내의 repository는 어떻게 사용하나요?
organization의 repository는 일반적인 개인 계정의 repository와 동일한 방법으로 연동하고, 사용할 수 있습니다. 해당 항목을 참고해주세요!</p>
</blockquote>
<h2 id="2--organization-생성-이후-사용자-추가하기">2. ### Organization 생성 이후 사용자 추가하기</h2>
<p>1) organization 상단 메뉴의 <code>People</code>을 선택합니다. </p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/1f5013d5-c42e-44c1-8612-9104a88b05e7/image.png" /></p>
<p>2) <code>Invite member</code> 버튼을 누르고, 초대할 사용자의 username, 혹은 이메일을 입력해 초대합니다. </p>
<p>  <img alt="" src="https://velog.velcdn.com/images/taketheking/post/bfde089f-0e37-4355-b047-252bc3b451ab/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/ef77f527-cbe3-4c38-8409-09f994733a6c/image.png" /></p>
<p>3) 초대할 사용자의 권한을 설정하고, <code>Send invitation</code> 버튼을 눌러 사용자에게 초대장을 발송합니다. </p>
<pre><code>![](https://velog.velcdn.com/images/taketheking/post/01372d72-04ce-45e2-a203-c59c8fcb6cbd/image.png)</code></pre><p>4) 초대받은 사용자는 초대를 수락하기 전까지 Pending 상태로 표시됩니다. 
(초대는 7일간 유효합니다.)</p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/e49db580-f2fc-4c45-a3d1-03349b76e2ec/image.png" /></p>
<p>5) 사용자는 이메일로 발송된 초대장, 혹은 GitHub 계정의 Your organization 항목에서 <code>Join</code> 버튼을 눌러 초대를 수락할 수 있습니다. </p>
<p>   <img alt="" src="https://velog.velcdn.com/images/taketheking/post/60f05bd7-7a0c-4e98-a948-8f19464b60cb/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/60a84901-f925-4fb8-9236-8c937ecd4f0e/image.png" /></p>
<p>   <img alt="" src="https://velog.velcdn.com/images/taketheking/post/cfa4882f-d9b9-4697-ac4d-e7fa9f7fae02/image.png" /></p>
<ol>
<li><p>이후 정상적으로 멤버가 추가된 것을 확인할 수 있습니다. </p>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/efbf236b-53d1-4da0-99ae-447bb43a1fa1/image.png" /></p>
</li>
</ol>