<h1 id="좋은-git-commit-message-작성-가이드라인-컨벤션">좋은 Git Commit Message 작성 가이드라인 [컨벤션]</h1>
<blockquote>
<p>좋은 Git Commit Message란 무엇일까? 
git은 동료들과 함께 협업하기 위한 버전형상관리 도구이다. 즉, 직관적이면서도 가독성이 좋아야 협업의 효율성이 올라가고 유지보수도 편해진다.</p>
</blockquote>
<h2 id="1-commit-message">1. Commit Message</h2>
<blockquote>
<p>Ailbaba Fusion commit
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/95522e23-772f-483a-9985-88bed98f1517/image.png" /></p>
</blockquote>
<blockquote>
<p>NHN tui.calendar commit
<img alt="" src="https://velog.velcdn.com/images/taketheking/post/7549ff8f-5989-4f91-bfa9-a29b7f04cd55/image.png" /></p>
</blockquote>
<p>위 사진은 Alibaba Fusion의 Next 레포지토리와 NHN의 tui.calendar 레포지토리에서 가져온 커밋 히스토리이다.</p>
<p>두 커밋의 히스토리가 상당히 유사하다.
사람들이 협업하기에 좋은 메세지의 유형이 어느정도 존재한다는 것이다.</p>
<p>그렇다면 사람들이 추구하는 일반적인 규칙을 알아보자.</p>
<h2 id="2-좋은-commit-message-작성-규칙">2. 좋은 Commit Message 작성 규칙</h2>
<blockquote>
</blockquote>
<ol>
<li>제목과 본문을 빈 행으로 구분하기</li>
</ol>
<blockquote>
</blockquote>
<ol start="2">
<li>제목은 영문 기준 50글자 이내로 작성</li>
</ol>
<blockquote>
</blockquote>
<ol start="3">
<li>첫 글자는 대문자로 작성</li>
</ol>
<blockquote>
</blockquote>
<ol start="4">
<li>제목 끝에 마침표 금지</li>
</ol>
<blockquote>
</blockquote>
<ol start="5">
<li>제목은 명령문으로 사용하고 과거형 금지</li>
</ol>
<blockquote>
</blockquote>
<ol start="6">
<li>본문의 각 행은 영문 기준 72글자 이하로 하고 줄 바꾸기</li>
</ol>
<blockquote>
</blockquote>
<ol start="7">
<li>어떻게 보다는 무엇과 왜에 초점을 맞춰 작성</li>
</ol>
<blockquote>
</blockquote>
<ol start="8">
<li>검토자의 이해도를 가정하지 말고 최대한 설명 제공</li>
</ol>
<blockquote>
</blockquote>
<ol start="9">
<li>팀의 규칙을 따르자</li>
</ol>
<h2 id="3-commit-message-구조">3. Commit Message 구조</h2>
<pre><code>#
# Commit Message 기본 구조
#

&lt;type&gt;(&lt;scope&gt;): &lt;subject&gt;

&lt;body&gt;

(&lt;footer&gt;)</code></pre><h3 id="-subject--type-커밋-종류">[ Subject ] type (커밋 종류)</h3>
<p><img alt="" src="https://velog.velcdn.com/images/taketheking/post/205d6566-c359-479c-82c0-5c4c51ac9e47/image.PNG" /></p>
<h3 id="-subject--scope">[ Subject ] scope</h3>
<p>선택사항이며, 변경된 부분을 직접적으로 표기합니다.
EX) 함수가 변경되었으면 함수명, 메소드가 추가되었으면 클래스명 기입 등</p>
<h3 id="-subject--subject">[ Subject ] subject</h3>
<p>첫 글자는 대문자로 입력한다.
마지막에는 .(period)을 찍지 않는다.
특수기호를 사용하지 않는다.
영문 기준 최대 50자를 넘지 않는다.
제목은 명령문의 형태로 작성한다. (동사원형 사용)</p>
<blockquote>
<p>Fixed -&gt; Fix
  Added -&gt; Add</p>
</blockquote>
<h3 id="-body-">[ Body ]</h3>
<p>각 줄은 최대 72자를 넘지 않도록 한다.
어떻게 변경했는지보다, 무엇을 변경했고, 왜 변경했는지를 설명한다.</p>
<h3 id="-footer-">[ Footer ]</h3>
<p>꼬릿말은 다음의 규칙을 지킨다.</p>
<p>꼬리말은 optional이고 이슈 트래커 ID를 작성한다.
꼬리말은 &quot;유형: #이슈 번호&quot; 형식으로 사용한다.
여러 개의 이슈 번호를 적을 때는 쉼표(,)로 구분한다.
이슈 트래커 유형은 다음 중 하나를 사용한다.</p>
<ul>
<li>Fixes: 이슈 수정중 (아직 해결되지 않은 경우)</li>
<li>Resolves: 이슈를 해결했을 때 사용</li>
<li>Ref: 참고할 이슈가 있을 때 사용</li>
<li>Related to: 해당 커밋에 관련된 이슈번호 (아직 해결되지 않은 경우)
ex) Fixes: #45 Related to: #34, #23</li>
</ul>
<blockquote>
</blockquote>
<p>대부분 가장 많이 사용하는 것은 feat와 fix입니다. style, design처럼 로직적인 변화가 없을 경우에 커밋 메세지에 명시해주는 것만으로도 추후 오류를 찾을 때 많은 도움이 됩니다.</p>
<blockquote>
</blockquote>
<p>타입 뒤에 변경된 함수나 메소드를 직접적으로 명시하기도 합니다.</p>
<blockquote>
</blockquote>
<p>ex) fix(tab): ...</p>
<h2 id="4-예시">4. 예시</h2>
<h3 id="1-기능-추가">1. 기능 추가</h3>
<blockquote>
</blockquote>
<p>feat: Add dark mode toggle feature</p>
<blockquote>
</blockquote>
<p>Implemented a dark mode toggle on the settings page. Users can now switch between light and dark themes, which is saved in their profile settings.</p>
<h3 id="2-버그-수정">2. 버그 수정</h3>
<blockquote>
</blockquote>
<p>fix: Correct issue with data fetching on mobile</p>
<blockquote>
</blockquote>
<p>Fixed a bug where data was not being fetched properly on mobile devices due to a wrong user-agent check. Updated the user-agent detection logic.</p>
<blockquote>
</blockquote>
<p>Closes #123</p>
<h3 id="3-코드-리팩토링">3. 코드 리팩토링</h3>
<blockquote>
</blockquote>
<p>refactor: Simplify form validation logic</p>
<blockquote>
</blockquote>
<p>Refactored the form validation module to reduce code duplication and improve readability. Merged similar validation functions into a single utility.</p>
<h3 id="4-호환성-이슈">4. 호환성 이슈</h3>
<blockquote>
</blockquote>
<p>feat: Add support for user avatars</p>
<blockquote>
</blockquote>
<p>Users can now upload and display avatars on their profiles. 
Added image upload handling and a new API endpoint to serve the images.</p>
<blockquote>
</blockquote>
<p>BREAKING CHANGE: The user profile model has been updated to include a new 'avatar_url' field. This change may affect existing profile data.
Fixes #456</p>
<h3 id="5-한국어-예시">5. 한국어 예시</h3>
<blockquote>
</blockquote>
<p>Feat: &quot;회원 가입 기능 구현&quot;</p>
<blockquote>
</blockquote>
<p>SMS, 이메일 중복확인 API 개발</p>
<blockquote>
</blockquote>
<p>Resolves: #123
Ref: #456
Related to: #48, #45</p>
<h2 id="5-commit-message-emogji">5. Commit Message Emogji</h2>
<p>아래는 자주 쓰이는 대표적인 몇가지 일부만 정리</p>
<p>Emogi    Description
🎨    코드의 형식 / 구조를 개선 할 때
📰    새 파일을 만들 때
📝    사소한 코드 또는 언어를 변경할 때
🐎    성능을 향상시킬 때
📚    문서를 쓸 때
🐛    버그 reporting할 때, @FIXME 주석 태그 삽입
🚑    버그를 고칠 때
🔥    코드 또는 파일 제거할 때 , @CHANGED주석 태그와 함께
🚜    파일 구조를 변경할 때 . 🎨과 함께 사용
🔨    코드를 리팩토링 할 때
💄    UI / style 개선시
♿️    접근성을 향상시킬 때
🚧    WIP (진행중인 작업)에 커밋, @REVIEW주석 태그와 함께 사용
💎    New Release
🔖    버전 태그
✨    새로운 기능을 소개 할 때
⚡️    도입 할 때 이전 버전과 호환되지 않는 특징, @CHANGED주석 태그 사용
💡    새로운 아이디어, @IDEA주석 태그
🚀    배포 / 개발 작업 과 관련된 모든 것</p>
<p><a href="https://treasurebear.tistory.com/70">https://treasurebear.tistory.com/70</a> 을 참고하자.</p>
<h2 id="6-여러줄-입력-방법">6. 여러줄 입력 방법</h2>
<h3 id="1-를-열고-메세지를-작성">1) “를 열고 메세지를 작성</h3>
<pre><code>git commit -m &quot;title
body
footer&quot;</code></pre><p>결과)</p>
<pre><code>title
body
footer</code></pre><h3 id="2-여러개의--m-태그-사용">2) 여러개의 -m 태그 사용</h3>
<p>-m 태그마다 paragraphs를 생성하므로 blank line이 생긴다.</p>
<pre><code>git commit -m &quot;title&quot; -m &quot;body&quot; -m &quot;footer&quot;</code></pre><p>결과)</p>
<pre><code>title

body

footer
</code></pre><hr />
<p>참고</p>
<p><a href="https://jane-aeiou.tistory.com/93">https://jane-aeiou.tistory.com/93</a></p>
<p><a href="https://meetup.nhncloud.com/posts/106">https://meetup.nhncloud.com/posts/106</a></p>
<p><a href="https://nohack.tistory.com/17">https://nohack.tistory.com/17</a></p>
<p><a href="https://treasurebear.tistory.com/70">https://treasurebear.tistory.com/70</a></p>
<p><a href="https://velog.io/@shin6403/Git-git-%EC%BB%A4%EB%B0%8B-%EC%BB%A8%EB%B2%A4%EC%85%98-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0">https://velog.io/@shin6403/Git-git-%EC%BB%A4%EB%B0%8B-%EC%BB%A8%EB%B2%A4%EC%85%98-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0</a></p>