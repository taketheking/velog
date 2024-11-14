<blockquote>
</blockquote>
<p>스프링 컨트롤러에서 받아서 사용할 수 있는 파라미터를 알아보자.</p>
<h1 id="httpservletrequesthttpservletresponse">HttpServletRequest,HttpServletResponse</h1>
<ul>
<li>ServletRequest, ServletResponse 타입도 가능하다.</li>
</ul>
<h1 id="httpsession">HttpSession</h1>
<ul>
<li>HttpServletRequest를 통해 가져올 수도 있다.</li>
<li>세션만 필요한 경우라면 직접 받는것이 낫다.</li>
</ul>
<h1 id="pathvariable">@PathVariable</h1>
<ul>
<li>URL에 들어가는 패스변수를 받는다.</li>
</ul>
<h1 id="requestmappinguserviewid">@RequestMapping(&quot;/user/view/{id}&quot;)</h1>
<pre><code>public String view(@PathVariable(&quot;id&quot;) int id) { ... }</code></pre><ul>
<li>여러개의 패스 변수를 받을 수도 있다.</li>
</ul>
<h1 id="requestmappingmembermembercodeorderorderid">@RequestMapping(&quot;/member/{memberCode}/order/{orderId}&quot;)</h1>
<pre><code>public String lookup(@PathVariable(&quot;memberCode&quot;) String code, @PathVariable(&quot;orderId&quot;) int orderId) {    ... }</code></pre><ul>
<li>파라미터 타입은 URL의 내용이 적절히 변환될 수 있는 것을 사용해야 한다.</li>
<li>타입이 일치하지 않으면 예외가 발생하며 클라이언트에 400 코드가 전달될 것이다.</li>
</ul>
<h1 id="requestparam">@RequestParam</h1>
<ul>
<li>단일 HTTP요청 파라미터를 메소드 파라미터에 넣어주는 애노테이션</li>
<li>요청 파라미터의 값은 메소드 파라미터의 타입에 따라 적절하게 변환된다.<pre><code>public void view(@RequestParam(&quot;id&quot;) int id) { ... }</code></pre></li>
<li>하나 이상의 파라미터에 적용할 수 있다. 스프링의 내장 변환기가 다룰 수 있는 모든 타입을 지원한다.<pre><code>public void view(@RequestParam(&quot;id&quot;) int id, 
 @RequestParam(&quot;name&quot;) String name, 
 @RequestParam(&quot;file&quot;) MultipartFile file) { ... }</code></pre></li>
<li>파라미터 이름을 지정하지 않고 Map&lt;String, String&gt; 타입으로 선언하면 모든 요청 파라미터를 담은 맵으로 받을 수 있다. 파라미터의 이름은 맵의 키에, 파라미터 값은 맵의 값에 담겨 전달된다.<pre><code>public void add(@RequestParam Map&lt;String, String&gt; params) { ... }</code></pre></li>
<li>@RequestParam을 사용했다면 해당 파라미터가 반드시 있어야만 한다. 없다면 400 - Bad Request를 받게 될 것이다. 파라미터를 선택적으로 제공하게 하려면 required 엘리먼트를 false로 설정해주면 된다. 요청 파라미터가 존재하지 않을 때 사용할 디폴트 값도 지정할 수 있다.<pre><code>public void view(@RequestParam(value=&quot;id&quot;, required=false, defaultValue-&quot;-1&quot; int id) { ... }</code></pre></li>
<li>메소드 파라미터의 이름과 요청 파라미터의 이름이 일치한다면 value를 생략할 수 있다.<pre><code>public void view(@RequestParam int id) { ... }</code></pre></li>
<li>String, int와 같은 단순 타입인 경우는 @RequestParam을 아예 생략할 수도 있다. 하지만 파라미터의 개수가 많고 종류가 다양하면 코드를 이해하는 데 불편할 수도 있다. 단순한 메소드가 아니라면 명시적으로 @RequestParam을 부여해주는 것을 권장한다.</li>
</ul>
<h1 id="cookievalue">CookieValue</h1>
<ul>
<li>HTTP 요청과 함께 전달된 쿠키 값을 받을 수 있다.</li>
<li>애노테이션의 기본 값에 쿠키의 이름을 지정해주면 된다.<pre><code>public String check(@CookieValue(&quot;auth&quot;) String auth) { ... }</code></pre></li>
<li>메소드 파라미터 이름과 쿠키 값이 동일하다면 쿠키 이름은 생략할 수 있다.</li>
<li>지정된 쿠키 값이 존재하지 않으면 예외가 발생한다.</li>
<li>쿠키 값이 없을 때 예외가 발생하지 않게 하려면 required 엘리먼트를 false로 선언해줘야 한다. 또한 defaultValue로 디폴트 값을 선언할 수도 있다.</li>
</ul>
<h1 id="requestheader">@RequestHeader</h1>
<ul>
<li>요청 헤더정보를 메소드 파라미터에 넣어주는 애노테이션이다.</li>
<li>헤더값이 반드시 존재해야하므로, 설정이 필요하면 required와 defaultValue를 이용할 수 있다.<pre><code>public void header(@RequestHeader(&quot;Host&quot;) String host,
 @RequestHeader(&quot;Keep-Alive&quot;) long keepAlive) { ... }</code></pre></li>
</ul>
<h1 id="map-model-modelmap">Map, Model, ModelMap</h1>
<ul>
<li>Model과 ModelMap은 모두 addAttribute() 메소드를 제공해준다.</li>
<li>일반적인 맵의 put() 처럼 이름을 지정해서 오브젝트 값을 넣을 수 있다.</li>
<li>model.addAttribute(user) 는 model.addAttribute(&quot;user&quot;, user)와 동일하다.</li>
<li>ModelMap과 Model의 addAllAttribute() 메소드를 사용하면 Collection에 담긴 모든 오브젝트를 자동 이름 생성 방식을 적용해서 모두 모델로 추가해준다.</li>
</ul>
<h1 id="modelattribute">@ModelAttribute</h1>
<ul>
<li>메소드 파라미터에도 부여할 수 있고, 메소드 레벨에 적용할 수도 있다.</li>
<li>모델 맵에 담겨서 뷰에 전달되는 모델 오브젝트의 한가지</li>
<li>하나 이상의 값을 가진 오브젝트 형태로 만들 수 있는 구조적인 정보를 담는다.</li>
<li>요청 파라미터나 폼데이터를 도메인 오브젝트나 DTO의 프로퍼티에 요청 파라미터를 바인딩해서 한번에 받을 수 있다.<pre><code>@RequestMapping(&quot;/user/search&quot;)
public String serarch(@RequestParam int id, 
  @RequestParam String name, 
    @RequestParam int level, 
    @RequestParam String email) { ... }</code></pre></li>
<li>위와 같은 컨트롤러를 클래스를 이용하여 아래와 같이 바꿀 수 있다.<pre><code>public class UserSearch {
    int id;
    String name;
    int level;
    String email;
    // 수정자, 접근자 생략
}
</code></pre></li>
</ul>
<p>@RequestMapping(&quot;/user/search&quot;)
public String serarch(@ModelAttribute UserSearch userSearch) { ... }</p>
<pre><code>
 - 생략 가능하며, 스프링은 @ModelAttribute와 @RequestParam이 생략된 파라미터를 보고 String, int와 같은 단순타입이면 @RequestParam으로 간주하고, 그 외의 복잡한 오브젝트는 모두 @ModelAttribute로 간주한다.
 - 무분별 생략은 위험할 수 있으므로 명시하는것을 권장한다.
 - 또한 컨트롤러가 리턴하는 모델에 파라미터로 전달한 오브젝트를 자동으로 추가해준다. 즉, @ModelAttribute 애노테이션을 사용하면 뷰에서 필요하다고 해서 코드에서 추가해줄 필요가 없다.

# Errors, BindingResult
 - 바로 앞의 파라미터 오브젝트를 바인딩하다가 발생한 변환 오류와 모델 검증기를 통해 검증하는 중에 발견한 오류가 저장된다.</code></pre><p>@RequestMapping(value=&quot;add&quot;, method=RequestMethod.Post)
public String add(@ModelAttribute User user, BindingResult bindingResult) { ... }</p>
<p>```</p>
<ul>
<li>BindingResult 대신 Errors 타입으로 선언할 수 있다.</li>
<li>반드시 @ModelAttribute 파라미터 뒤에 나와야 한다. 자신의 바로 앞에 있는 @ModelAttribute 파라미터의 검증 작업에서 발생한 오류만을 전달해주기 때문이다.</li>
</ul>
<h1 id="requestbody">@RequestBody</h1>
<ul>
<li>HTTP 요청의 본문 부분이 그대로 전달된다.</li>
<li>보통 @ResponseBody와 함께 사용된다.</li>
</ul>
<h1 id="value">@Value</h1>
<ul>
<li>빈의 값 주입에서 사용하던 @Value 애노테이션도 컨트롤러 메소드 파라미터에 부여할 수 있다.</li>
</ul>
<h1 id="valid">@Valid</h1>
<ul>
<li>빈 검증기를 이용해 모델 오브젝트를 검증하도록 지시하는 지시자.
보통 @ModelAttribute와 함께 사용된다.</li>
</ul>