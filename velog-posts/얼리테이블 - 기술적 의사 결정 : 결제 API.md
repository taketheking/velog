<h1 id="결제-api--kg-이니시스-vs--kakaopay--vs--tosspay">결제 API : KG 이니시스 vs  KakaoPay  vs  TossPay</h1>
<h3 id="kg-이니시스-🏦💳">KG 이니시스 🏦💳</h3>
<ul>
<li><strong>🟩장점</strong><ul>
<li><strong>다양한 결제 수단 지원💳</strong> : 신용카드, 가상계좌, 계좌이체, 휴대폰 결제 등 다양한 결제 옵션을 제공</li>
<li><strong>API 안정성 높음🔒</strong> : 오랜 기간 운영된 PG사인만큼 장애 발생 가능성이 적음</li>
<li><strong>정기 결제 &amp; B2B 기능 제공📅🔄</strong> : 정기 결제, 자동 결제 같은 고급 기능 지원</li>
<li><strong>대량 거래 처리 가능📊</strong> : 트래픽이 많거나 B2B 환경에서도 안정적</li>
</ul>
</li>
<li><strong>🟥단점</strong><ul>
<li><strong>JSON기반이 아닌 key=value방식 사용📝</strong> : RESTful API를 제공하지만, 최신 표준인 JSON이 아닌 key = value 형태로 데이터를 주고 받음</li>
<li><strong>불편한 테스트 환경⚠️</strong> : API를 테스트 하려면 실제 가맹점 가입 &amp; 승인 절차가 필요</li>
<li><strong>UI/UX 개선 필요🖥️</strong> : 다른 결제 서비스 대비 사용자 경험이 다소 불편할 수 있음</li>
</ul>
</li>
</ul>
<h3 id="kakaopay💛💬💰">KakaoPay💛💬💰</h3>
<ul>
<li><strong>🟩장점</strong><ul>
<li><strong>높은 사용자 접근성🌍</strong> : 카카오톡 기반으로 가입자가 많아 결제 전환율이 높음</li>
<li><strong>모바일 친화적인 UX/UI📱</strong> : 카카오톡 기반 결제로 사용자에게 익숙한 환경</li>
<li><strong>편의성🔑</strong> : 결제 연동이 간편하고 문서화가 잘 되어 있음</li>
<li><strong>손쉬운 테스트 환경🧪</strong> : 별도의 심사 없이 테스트 가능</li>
</ul>
</li>
<li><strong>🟥단점</strong><ul>
<li><strong>카카오톡 중심📲</strong> : 카카오톡을 사용하지 않는 사용자에게는 접근성이 낮음, 카카오톡 장애 발생시 결제 불가</li>
<li><strong>일부 결제 수단 제한 🌍💳</strong> : 해외 결제 및 일부 신용카드 결제 제한</li>
<li><strong>제한된 UI커스텀🛠️</strong> : 카카오 페이에서 제공하는 방식으로만 창을 띄울 수 있고 PC환경에서는 결제를 위해 QR코드 스캔 또는 전화번호 입력의 번거로움이 있음</li>
</ul>
</li>
</ul>
<h3 id="tosspay-💙⚡💰">TossPay 💙⚡💰</h3>
<ul>
<li><strong>🟩장점</strong><ul>
<li><strong>손쉬운 테스트 환경🧪</strong> : 별도의 심사 없이 테스트 가능</li>
<li><strong>결제창 UI 커스텀 가능🎨</strong> : Toss Paymemts SDK를 활용하여 자체 UI구성이 가능</li>
<li><strong>광범위한 결제 수단 지원 💳📱</strong> : 신용카드, 계좌이체, 가상계좌, 휴대폰결제 등 다양한 결제 수단을 제공</li>
</ul>
</li>
<li><strong>🟥단점</strong><ul>
<li><strong>제한적인 사용자층👥</strong> : 토스 사용자가 빠르게 증가하였지만, 카카오페이에 비해 인지도나 이용자가 적을 수 있음 (4050세대 이상)</li>
<li><strong>PC환경에서의 결제 경험 제한💻</strong> : 모바일 중심 서비스라 PC웹에서 UX가 최적화 되지 않았을 수도 있음</li>
</ul>
</li>
</ul>
<h2 id="왜-kakaopay를-선택했는가-💛"><strong>왜 KakaoPay를 선택했는가?</strong> 💛</h2>
<p>이번 프로젝트에서 KakaoPay를 선택한 이유는 주로 사용자 접근성과 편의성에 큰 장점이 있기 때문입니다. 여러 PG사들이 각각의 장단점이 있지만, KakaoPay는 다음과 같은 이유로 더 적합한 선택이었습니다.</p>
<h3 id="1-높은-사용자-접근성-🌍">1. 높은 사용자 접근성 🌍</h3>
<p> KakaoPay는 <strong>카카오톡</strong>과 깊게 통합되어 있기 때문에, 이미 <strong>많은 사용자들이 익숙</strong>하게 사용하고 있는 환경입니다. 결제 과정에서 사용자가 별도의 앱을 설치할 필요 없이 <strong>카카오톡만 있으면 바로 결제</strong>가 가능합니다. 이는 사용자 경험을 크게 향상시키며, <strong>결제 전환율</strong>을 높이는 데 유리한 요소입니다. 토스도 마찬가지로 많은 사용자와 모바일에 친숙하지만 토스 이용자 수 보다 카카오톡을 이용하는 이용자 수가 더 높을 것으로 예상되었습니다.</p>
<h3 id="2-모바일-친화적인-uxui📱">2. 모바일 친화적인 UX/UI📱</h3>
<p>KakaoPay는 <strong>모바일 환경에서 최적화된 UI/UX</strong>를 제공하여, 사용자가 결제 과정을 더욱 직관적으로 진행할 수 있습니다. 특히 카카오톡 사용자들에게 매우 익숙한 UI를 제공하여 <strong>사용자의 불편함을 최소화</strong> 할 수 있습니다. 이런 점에서 <strong>모바일 중심의 비즈니스</strong>에 특히 유리합니다. 완성까지의 시간을 고려하여 앱이 아닌 웹으로 유저의 UI를 제작하게 되었지만 기존에 앱을 생각하며 구상하였기 때문에 더욱 적합하다 생각하였습니다.</p>
<h3 id="3-편리한-테스트-환경-🧪">3. 편리한 테스트 환경 🧪</h3>
<p>다른 PG사들과 달리 KakaoPay는 <strong>심사 없이 테스트가 가능</strong>하다는 점이 큰 장점으로 와닿았습니다.  개발 및 테스트가 용이하고, 별도의 절차 없이 바로 테스트를 시작할 수 있어 <strong>개발 속도와 효율성</strong>을 크게 개선할 수 있었습니다.</p>
<h2 id="선택의-아쉬운-점-😮💨"><strong>선택의 아쉬운 점</strong> 😮‍💨</h2>
<h3 id="1-결제-수단의-제한🚫💳"><strong>1. 결제 수단의 제한🚫💳</strong></h3>
<p>KakaoPay는 카카오톡 기반이라 편리하지만, <strong>카카오톡을 사용하지 않거나 카카오페이 결제에 익숙하지 않은 사용자에게는 접근성이 떨어질 수 있습니다</strong>. 또한, 특정 해외 결제 및 일부 신용카드 사용이 제한되는 점도 아쉬운 부분입니다.</p>
<h3 id="2-ui-커스터마이징의-한계🎨🔒">2. UI 커스터마이징의 한계🎨🔒</h3>
<p>KakaoPay의 결제 화면은 <strong>카카오가 제공하는 방식 그대로 사용</strong>해야 하므로, 서비스에 맞게 자유롭게 <strong>커스터마이징하기 어렵습니다</strong>. 특히 PC 환경에서는 <strong>QR코드 스캔 또는 전화번호 입력 과정이 다소 번거롭게 느껴졌습니다</strong>.</p>
<h3 id="3-여러-pg사-연동의-어려움🔄🛠️">3. 여러 PG사 연동의 어려움🔄🛠️</h3>
<p>이번 프로젝트에서는 KakaoPay만을 고려하여 개발을 진행했기 때문에 당장은 문제가 없지만, 추후 다른 결제 서비스를 추가하려면 <strong>각각 개별로 연동해야 하는 번거로움</strong>이 발생할 수 있습니다.</p>
<p>이러한 경험을 바탕으로, 다음 프로젝트에서는 &quot;포트원(PortOne)&quot;을 활용하여 여러 PG사를 <strong>하나의 API로 통합하여 연동하는 방식</strong>을 시도할 계획입니다. 이를 통해 다양한 <strong>결제 수단을 유연하게 추가하고, 유지보수의 부담을 줄일 수 있을 것</strong>으로 기대됩니다.</p>