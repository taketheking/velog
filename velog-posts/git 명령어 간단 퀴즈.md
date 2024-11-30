<h1 id="git-명령어-간단-퀴즈">git 명령어 간단 퀴즈</h1>
<h3 id="1프로젝트를-처음-만들었고git으로-버전관리를-시작하고-싶을-때-작성하는-명령어는">1.프로젝트를 처음 만들었고,git으로 버전관리를 시작하고 싶을 때 작성하는 명령어는?</h3>
<p>ㄴ git init</p>
<h3 id="2코드를-작성하고코드를-저장하고-싶다이때-사용해야-할-두-가지-명령어는">2.코드를 작성하고,코드를 저장하고 싶다.이때 사용해야 할 두 가지 명령어는?</h3>
<p>ㄴ 1.git add.
ㄴ 2.git commit -m “메세지”</p>
<h3 id="3코드-저장-이후-저장-기록커밋-내역을-보고-싶다면-어떤-명령어를-입력할까">3.코드 저장 이후 저장 기록(커밋 내역)을 보고 싶다면 어떤 명령어를 입력할까?</h3>
<p>ㄴ git log</p>
<h3 id="4현재-git상태를-확인하고-싶다면">4.현재 git상태를 확인하고 싶다면?</h3>
<p>ㄴ git status</p>
<h3 id="5github에-코드를-업로드하고-싶을-때-사용하는-명령어는">5.github에 코드를 업로드하고 싶을 때 사용하는 명령어는?</h3>
<p>ㄴ git push origin 브랜치명</p>
<h3 id="6github에-있는-프로젝트를-복제해오고-싶은-경우-사용하는-명령어는">6.github에 있는 프로젝트를 복제해오고 싶은 경우 사용하는 명령어는?</h3>
<p>ㄴ git clone github주소 .(폴더를 이미 만들었다면 .붙이기)
ㄴ git clone github주소 (폴더가 없다면 .없애기)</p>
<h3 id="7github-repository에서-변경된-코드를-내-로컬-컴퓨터로-가져오고-싶을-때-사용하는-명령어는">7.github repository에서 변경된 코드를 내 로컬 컴퓨터로 가져오고 싶을 때 사용하는 명령어는?</h3>
<p>ㄴ git pull origin브랜치명</p>
<h3 id="8충돌-발생-시-어떻게-해결하면-될까요">8.충돌 발생 시 어떻게 해결하면 될까요?</h3>
<p>1.&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;HEAD삭제
2. =======삭제
3. &gt;&gt;&gt;&gt;&gt;&gt;&gt;4182….삭제
4. 원하는 코드로 수정
5. git add / git commit / git push</p>
<h3 id="9-origin은-무슨-의미인가">9. origin은 무슨 의미인가?</h3>
<ol>
<li><p>git remote add origin &lt;github 주소&gt;</p>
<ul>
<li>gitpush“github 주소” 브랜치명 =&gt; 귀찮다!</li>
<li>github 주소를 origin이라는 이름으로 저장</li>
<li>gitpushorigin브랜치명 =&gt; 편하다!</li>
</ul>
</li>
<li><p>git branch -M main</p>
<ul>
<li>기본 브랜치 = master =&gt; 노예 제도와 관련</li>
<li>현재 브랜치명(master)을 main으로 바꾼다!</li>
</ul>
</li>
<li><p>git push -u origin main</p>
<ul>
<li>git push origin main =&gt; 귀찮아!</li>
<li>git push=&gt;이 명령어만 입력해도 git push origin main 해줘!</li>
</ul>
</li>
</ol>
<h3 id="10-branch를-이동하는-방법은">10. Branch를 이동하는 방법은?</h3>
<ol>
<li>git switch 브랜치이름</li>
<li>git check out 브랜치이름</li>
</ol>
<h3 id="11-branch-생성-및-이동하는-명령어는">11. Branch 생성 및 이동하는 명령어는?</h3>
<ol>
<li>git switch -c 브랜치이름</li>
<li>git check out -b 브랜치이름</li>
</ol>
<h3 id="12-main-branch에-login-branch를-합치는-방법은">12. Main Branch에 Login Branch를 합치는 방법은?</h3>
<p> 1단계 - git switch 최종브랜치이름(Main)</p>
<p> 2단계 - git git merge 합칠브랜치이름(Login)</p>
<h3 id="13-log가-보고-싶으면">13. log가 보고 싶으면?</h3>
<p> ㄴ git log --oneline --all --graph</p>
<h3 id="14-head가-멀까">14. HEAD가 멀까?</h3>
<p> ㄴ 나의 현재 Branch 위치</p>
<h3 id="15-branch-삭제-명령어는">15. Branch 삭제 명령어는?</h3>
<p> ㄴ git branch -d 브랜치명</p>
<h3 id="16-rebase-쓰는-이유는">16. rebase 쓰는 이유는?</h3>
<p> rebase는 브랜치 분기 시점을 변경해서 간단한 브랜치를 합치기 쉽게 하기 위한 것 
 단점 충돌이 많이 남</p>
<p> <img alt="" src="https://velog.velcdn.com/images/taketheking/post/742075f0-eaac-48cd-a35c-4ab2cb53af7b/image.PNG" />
위에가 일반 3-way merge이고 아래가 rebase merge이다.</p>
<p> 1단계 - 합칠 브랜드로 이동 (Login)
 2단계 - git rebase 최종브랜치이름(Main)
 3단계 - git merge 합칠브랜치명(Login)</p>
<h3 id="17-squash는-어떻게-쓰는거야">17. squash는 어떻게 쓰는거야?</h3>
<p> squash는 다른 브랜치의 내역들을 제거하고 최종 결과물을 최종 브랜치에 합친다.</p>
<p> <img alt="" src="https://velog.velcdn.com/images/taketheking/post/c6dac46f-d886-4500-b975-451e9b48c485/image.PNG" /></p>
<p> git merge --squash 합칠브랜치명(Login)</p>
<h3 id="18-특정-commit-시점으로-파일-복구하는-법은">18. 특정 commit 시점으로 파일 복구하는 법은?</h3>
<ol>
<li><p>직전 commit으로 복구 - git restore 파일명</p>
</li>
<li><p>특정 시점으로 복구 - git restore --source 특정커밋아이디 파일명</p>
</li>
<li><p>add 취소 - git restore --staged 파일명</p>
</li>
</ol>
<h3 id="19-특정-commit의-내용을-취소하는-법은">19. 특정 commit의 내용을 취소하는 법은?</h3>
<p> revert로 특정 commit 내용을 제거한 새로운 commit을 등록함
 merge도 취소가능함</p>
<p> git revert 특정커밋아이디(여러개도 가능함)</p>
<p> git revert 커밋아이디1 커밋아이디2</p>
<p> git revert HEAD</p>
<h3 id="20-특정-commit으로-아예-되돌리는-방법은">20. 특정 commit으로 아예 되돌리는 방법은?</h3>
<p> reset으로 가능 but 특정 commit 이후의 commit은 삭제됨</p>
<p> git reset --hard 커밋아이디</p>
<h3 id="21-git-변경점">21. git 변경점</h3>
<p> <img alt="" src="https://velog.velcdn.com/images/taketheking/post/7d2a9779-8282-42b3-bb66-6d69b65e28a5/image.png" /></p>