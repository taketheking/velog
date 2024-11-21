<p>application.properties 에는 보안에 치명적인 정보가 많이 들어있다.</p>
<p>인텔리제이의 왼쪽 사이드에서</p>
<p>&quot;.gitignore&quot; 이라는 파일을 찾아 클릭한다.</p>
<p>없을리가 없겠지만</p>
<p>없다면 &quot;.gitignore&quot;파일을 생성해주자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/ed0d8c10-33ee-468b-b7a3-d9b918866c02/image.PNG" /></p>
<p>다음과 같은 위치에</p>
<p>&quot;application.properties&quot;를 추가해 주었다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/618b94ad-a55e-440a-94a5-b072e5dac06f/image.png" /></p>
<p>.git에서 이전에 버전으로 관리했었던 properties를 관리하지 않도록 해주어야 한다.</p>
<blockquote>
</blockquote>
<p>git rm --cached &lt;관리하지 않고자하는 파일의 경로&gt;</p>
<p>1.git rm --cached src/main/resources/application.properties
2. git status</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/5f33e10d-93d4-48c2-aa06-e01470425ee9/image.PNG" /></p>
<p>git status로 확인했더니 deleted라고 되어있다.</p>
<p>깃허브 원격저장소에서 확인해 보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/take_the_king/post/c5f0d442-a751-461d-a7fd-80c7dbf17a33/image.PNG" /></p>
<p>이전 커밋에서는 있던 &quot;application.properties&quot;가</p>
<p>해당경로에서 완전히 제거된 것을 알 수 있다.</p>