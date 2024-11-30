<h1 id="basic-터미널-명령어">Basic 터미널 명령어</h1>
<h2 id="작업위치">작업위치</h2>
<h3 id="pwd">pwd</h3>
<p>Print working directory; 현재 작업 위치 알려줌.</p>
<h3 id="ls">ls</h3>
<p>list files; 현재의 directory의 모든 파일들을 보여줌.</p>
<h3 id="cd-">cd ..</h3>
<p>상위 디렉토리로 이동.</p>
<h3 id="cd--1">cd ~</h3>
<p>사용자의 홈디렉토리(/Users/hannah)로 감.</p>
<h3 id="cd-디렉토리명">cd 디렉토리명</h3>
<p>change directory; 원하는 디렉토리로 이동; 다만 건너뛸 수는 없음. 한 칸씩 단계적으로 들어가야 함.</p>
<h2 id="디렉토리--폴더">디렉토리 / 폴더</h2>
<h3 id="mkdir-디렉토리명">mkdir 디렉토리명</h3>
<p>make directory; 새로운 directory 생성.</p>
<h3 id="rm-rf-디렉토리명">rm –rf 디렉토리명</h3>
<p>디렉토리 삭제. 디렉토리와 디렉토리 하위의 모든 파일까지 삭제.</p>
<h3 id="cp--r-sourcedir-destdir">cp -R &lt;/sourcedir/&gt; &lt;/destdir/&gt;</h3>
<p>디렉토리 복사.
Sourcedir: 카피하고 싶은 폴더명, destdir: 옮기고싶은 폴더명.
검색키워드# mac terminal commands copy directory</p>
<h2 id="파일">파일</h2>
<h3 id="cat-파일명">cat 파일명</h3>
<p>파일의 contents를 보여줌.</p>
<h3 id="touch-파일명">touch .파일명</h3>
<p>파일 만들기.
Ex) touch .DS_Store (DS_Store라는 파일 만들기)</p>
<h3 id="echo-파일내용--파일명">echo &quot;파일내용&quot; &gt; 파일명</h3>
<p>내용과 함께 새로운 파일 만들기.
Ex) echo &quot;project test&quot; &gt; test.html ('project test'라는 내용이 있는 test.html 파일을 생성)</p>
<h3 id="파일명-gitignore">파일명 .gitignore</h3>
<p>무시해야할 소스파일 만들기. 소스파일 버전관리.</p>
<h3 id="ls--al--grep-파일명">ls -al | grep .파일명</h3>
<p>특정 파일 불러오기. 찾고 싶은 파일이 있을 때.
Ex) ls -al | grep .gitconfig (gitconfig파일 찾기)</p>
<h2 id="etc">etc.</h2>
<h3 id="ctrl--l">ctrl + L</h3>
<p>터미널 화면 clear (화면이 너무 복잡할 때).</p>