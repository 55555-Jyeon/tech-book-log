# SSR (서버 사이드 렌더링)

> LAMP 스택 <br />
> Linux(os) + Apache(서버) + MySQL(db) + PHP/Python(웹 프레임워크)

> JAM 스택 <br />
> JavaScript + API + Markup

> MEAN 스택 <br />
> MongoDB + Express.js + AngularJS + Node.js

> MERN 스택 <br />
> MongoDB + Express.js + React + Node.js

> 📖 SPA <br />
> Single Page Application의 약자 <br />
> 렌더링과 라우팅에 필요한 대부분의 기능을 브라우저의 자바스크립트에 의존하는 방식

> 📖 SSR <br />
> Server Side Rendering <br />
> 최초에 사용자에게 보여줄 페이지를 서버에서 렌더링해 빠르게 화면을 제공하는 방식

<br />
<br />

### SPA와 SSR

자바스크립트의 역할이 커짐에 따라 과거 LAMP 스택 기반의 SSR에서 SPA(CSR)이 주를 이루게 되었습니다. 하지만 웹페이지가 자바스크립트의 발전을 따라오지 못해 로딩 시간이 길어지는 문제가 발생했습니다. 따라서 현대의 SSR은 과거 SSR의 장점과 SPA(CSR)의 장점을 모두 취하는 방식으로 동작하고 있습니다.

SPA와 SSR 둘 다 모든 문제의 해결책이 될 수는 없기 때문에 개발자는 이런 모든 방법론을 이해할 필요가 있습니다. 하지만 적어도 SPA와 MPA에 대해서는 아래와 같이 얘기할 수 있습니다.

<br />

1. 가장 뛰어난 SPA는 가장 뛰어난 MPA보다 낫다.

MPA가 SPA와 같이 엄청난 최적화를 가미했다 하더라도 SPA가 가진 브라우저 API와 자바스크립트를 활용한 라우팅을 기반으로 한 매끄러운 라우팅보다 뛰어난 성능을 보여줄 수는 없기 때문입니다.

2. 평균적인 SPA는 평균적인 MPA보다 느리다.

일반적인 SPA는 사용자 기기에 따라 성능이 들쑥날쑥하고 이러한 최적화는 매우 어렵습니다.
심지어 최근에는 MPA에서 발생하는 라우팅 문제를 해결하기 위한 다양한 API가 브라우저에 추가되고 있습니다.

<br />
