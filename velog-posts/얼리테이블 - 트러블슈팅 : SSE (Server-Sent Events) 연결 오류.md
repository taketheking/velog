<h3 id="📌-react-프로젝트에서-sse-server-sent-events-트러블-슈팅"><strong>📌 React 프로젝트에서 SSE (Server-Sent Events) 트러블 슈팅</strong></h3>
<p>SSE (Server-Sent Events)를 React 애플리케이션에 적용하면서 발생한 문제들과 해결 방법을 정리</p>
<hr />
<h2 id="🚨-1-로그인회원가입-페이지에서도-sse가-실행되는-문제"><strong>🚨 1. 로그인/회원가입 페이지에서도 SSE가 실행되는 문제</strong></h2>
<h3 id="🔍-문제-원인"><strong>🔍 문제 원인</strong></h3>
<ul>
<li>SSE를 알림기능에 사용하기 위해 전역 페이지에 적용함.</li>
<li><code>SSEProvider</code>가 <code>App.js</code>의 <code>Router</code> 전체를 감싸고 있어 <strong>로그인/회원가입 페이지에서도 SSE가 실행됨</strong>.</li>
<li>로그인하지 않은 사용자에게는 불필요한 SSE 연결이 발생함.</li>
</ul>
<h3 id="💡-해결-방법"><strong>💡 해결 방법</strong></h3>
<p>✅ <strong>로그인 및 회원가입 페이지는 <code>SSEProvider</code>에서 제외하고, 인증된 페이지에만 적용</strong></p>
<pre><code class="language-jsx">&lt;Router&gt;
  &lt;Routes&gt;
    {/* ✅ 로그인 &amp; 회원가입에서는 SSEProvider 제외 */}
    &lt;Route path=&quot;/register&quot; element={&lt;Register /&gt;} /&gt;
    &lt;Route path=&quot;/login&quot; element={&lt;Login /&gt;} /&gt;

    {/* ✅ 나머지 페이지에서는 SSEProvider 적용 */}
    &lt;Route
      path=&quot;/*&quot;
      element={
        &lt;SSEProvider&gt;
          &lt;Routes&gt;
            &lt;Route path=&quot;/&quot; element={&lt;Home /&gt;} /&gt;
            &lt;Route path=&quot;/store/:storeId&quot; element={&lt;StoreDetails /&gt;} /&gt;
            {/* ...생략 */}
          &lt;/Routes&gt;
        &lt;/SSEProvider&gt;
      }
    /&gt;
  &lt;/Routes&gt;
&lt;/Router</code></pre>
<hr />
<h2 id="🚨-2-중복-sse-연결이-발생하는-문제"><strong>🚨 2. 중복 SSE 연결이 발생하는 문제</strong></h2>
<h3 id="🔍-문제-원인-1"><strong>🔍 문제 원인</strong></h3>
<ul>
<li><code>SSEProvider</code>의 <code>useEffect</code>가 여러 번 실행되어 <strong>기존 SSE 연결을 닫지 않은 채 새로운 연결이 계속 생성됨</strong>.</li>
<li>페이지 이동 시 <strong>기존 SSE 연결이 유지되지 않고 새로운 연결이 생성되어 중복 발생</strong>.</li>
</ul>
<h3 id="💡-해결-방법-1"><strong>💡 해결 방법</strong></h3>
<p>✅ <strong><code>eventSourceRef</code>를 <code>useRef</code>로 관리하여 중복 연결 방지</strong></p>
<p>✅ <strong>기존 SSE 연결이 있으면 새 연결을 생성하지 않도록 조건 추가</strong></p>
<pre><code class="language-jsx">onst eventSourceRef = useRef(null);

const connectSSE = () =&amp;gt; {
  if (eventSourceRef.current) {
    console.log(&quot;⚠️ 기존 SSE 연결 존재, 중복 연결 방지&quot;);
    return;
  }

  eventSourceRef.current = new EventSourcePolyfill(
    &quot;http://localhost:8080/notifications/subscribe&quot;,
    {
      headers: { Authorization: `Bearer ${localStorage.getItem(&quot;accessToken&quot;)}` },
      withCredentials: true,
    }
  );

  eventSourceRef.current.onopen = () =&amp;gt; {
    console.log(&quot;✅ SSE 연결 성공&quot;);
  };

  eventSourceRef.current.onerror = () =&amp;gt; {
    console.error(&quot;❌ SSE 연결 오류 발생, 재연결 시도...&quot;);
    eventSourceRef.current?.close();
    eventSourceRef.current = null;
    setTimeout(connectSSE, 3000); // 3초 후 재연결
  };
};

useEffect(() =&amp;gt; {
  connectSSE();

  return () =&amp;gt; {
    console.log(&quot;🛑 SSE 연결 해제&quot;);
    eventSourceRef.current?.close();
    eventSourceRef.current = null;
  };
}, []);</code></pre>
<p>✅ <strong>이제 페이지 이동 시에도 기존 SSE가 유지되며, 중복 연결이 방지됨!</strong></p>
<hr />
<h2 id="🚨-3-sse-연결-끊김-시-자동-재연결되지-않는-문제"><strong>🚨 3. SSE 연결 끊김 시 자동 재연결되지 않는 문제</strong></h2>
<h3 id="🔍-문제-원인-2"><strong>🔍 문제 원인</strong></h3>
<ul>
<li>SSE 연결이 끊기면 새로운 연결을 시도하지 않고 종료됨.</li>
<li>브라우저의 자동 재연결 기능이 동작하지 않는 경우가 발생함.</li>
</ul>
<h3 id="💡-해결-방법-2"><strong>💡 해결 방법</strong></h3>
<p>✅ <strong>SSE 에러 발생 시 자동 재연결 로직 구현</strong></p>
<p>✅ <strong>최대 3회까지 재연결을 시도하고, 실패 시 토큰 갱신 후 재시도</strong></p>
<pre><code class="language-jsx">const retryCountRef = useRef(0);

const connectSSE = () =&amp;gt; {
  if (eventSourceRef.current) {
    console.log(&quot;⚠️ 기존 SSE 연결 존재, 중복 연결 방지&quot;);
    return;
  }

  eventSourceRef.current = new EventSourcePolyfill(
    &quot;http://localhost:8080/notifications/subscribe&quot;,
    {
      headers: { Authorization: `Bearer ${localStorage.getItem(&quot;accessToken&quot;)}` },
      withCredentials: true,
    }
  );

  eventSourceRef.current.onopen = () =&amp;gt; {
    console.log(&quot;✅ SSE 연결 성공&quot;);
    retryCountRef.current = 0; // 재연결 카운트 초기화
  };

  eventSourceRef.current.onerror = async (error) =&amp;gt; {
    console.error(&quot;❌ SSE 연결 오류 발생:&quot;, error);
    eventSourceRef.current?.close();
    eventSourceRef.current = null;

    if (retryCountRef.current &amp;gt;= 3) {
      console.warn(&quot;🚨 SSE 재연결 3회 실패, 토큰 갱신 후 재시도&quot;);
      const success = await getRefreshToken();
      if (success) connectSSE();
    } else {
      setTimeout(() =&amp;gt; {
        retryCountRef.current += 1;
        console.log(`🔄 SSE 재연결 (${retryCountRef.current}번째 시도)`);
        connectSSE();
      }, 3000);
    }
  };
};</code></pre>
<p>✅ <strong>이제 SSE 연결이 끊겨도 자동으로 재연결됨!</strong></p>
<hr />
<h2 id="🚨-4-액세스-토큰-만료-시-401-오류가-반복되는-문제"><strong>🚨 4. 액세스 토큰 만료 시 401 오류가 반복되는 문제</strong></h2>
<h3 id="🔍-문제-원인-3"><strong>🔍 문제 원인</strong></h3>
<ul>
<li>액세스 토큰 만료 시 새로운 토큰을 받아오지 못해 SSE 연결이 실패함.</li>
<li>SSE 요청 시 <strong>만료된 액세스 토큰을 계속 사용하여 401 에러가 반복됨</strong>.</li>
</ul>
<h3 id="💡-해결-방법-3"><strong>💡 해결 방법</strong></h3>
<p>✅ <strong>401 오류 발생 시 자동으로 리프레시 토큰을 요청하고, 성공하면 SSE 재연결</strong></p>
<p>✅ <strong>토큰 갱신 요청의 중복 실행을 방지하도록 <code>isRefreshingRef</code> 활용</strong></p>
<pre><code class="language-jsx">const isRefreshingRef = useRef(false);

const getRefreshToken = async () =&amp;gt; {
  if (isRefreshingRef.current) return false;
  isRefreshingRef.current = true;

  try {
    console.log(&quot;🔄 토큰 갱신 시도...&quot;);
    const response = await instance.post(&quot;/users/refresh&quot;, {}, {
      headers: { &quot;Content-Type&quot;: &quot;application/json&quot; },
      withCredentials: true,
    });

    const newAccessToken = response.data.accessToken;
    if (!newAccessToken) throw new Error(&quot;새로운 토큰 없음&quot;);

    localStorage.setItem(&quot;accessToken&quot;, newAccessToken);
    console.log(&quot;✅ 토큰 갱신 성공&quot;);
    return true;
  } catch (err) {
    console.error(&quot;❌ 토큰 갱신 실패, 로그인 페이지로 이동&quot;);
    navigate(&quot;/login&quot;);
    return false;
  } finally {
    isRefreshingRef.current = false;
  }
};

eventSourceRef.current.onerror = async (error) =&amp;gt; {
  console.error(&quot;❌ SSE 연결 오류 발생:&quot;, error);
  eventSourceRef.current?.close();
  eventSourceRef.current = null;

  if (error.status === 401) {
    const success = await getRefreshToken();
    if (success) {
      console.log(&quot;🔄 SSE 재연결 시도...&quot;);
      connectSSE();
    }
  } else {
    setTimeout(() =&amp;gt; {
      console.log(&quot;🔄 SSE 자동 재연결...&quot;);
      connectSSE();
    }, 3000);
  }
};</code></pre>
<p>✅ <strong>이제 토큰이 만료되어도 자동으로 갱신되고 SSE가 재연결됨!</strong></p>
<hr />
<h2 id="🎯-정리"><strong>🎯 정리</strong></h2>
<table>
<thead>
<tr>
<th><strong>문제</strong></th>
<th><strong>해결 방법</strong></th>
</tr>
</thead>
<tbody><tr>
<td>로그인/회원가입에서도 SSE 실행됨</td>
<td>로그인/회원가입에서는<code>SSEProvider</code>제외</td>
</tr>
<tr>
<td>중복 SSE 연결 발생</td>
<td><code>useRef</code>를 사용해 중복 연결 방지</td>
</tr>
<tr>
<td>SSE가 끊겨도 자동 재연결 안 됨</td>
<td>최대 3번 재연결 후 토큰 갱신 후 재시도</td>
</tr>
<tr>
<td>액세스 토큰 만료 시 401 오류 반복</td>
<td>401 발생 시 토큰 갱신 후 SSE 재연결</td>
</tr>
</tbody></table>