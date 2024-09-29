# 리액트 훅 깊게 살펴보기

### Reach Hook

- 함수 컴포넌트가 상태를 사용하기 위해
- 클래스 컴포넌트의 생명주기 메서드를 대체하기 위해
- 클래스 컴포넌트에서만 사용 가능하던 기능(state, ref 등)을 함수도 사용할 수 있게 하기 위해

##### Q. 리액트의 훅 함수란?

리액트 훅은 함수형 컴포넌트에서 상태관리와 사이드 이펙트 처리를 할 수 있도록 제공되는 함수입니다. 리액트16버전 이후 추가되었으며, 클래스 컴포넌트의 생명주기 메서드를 함수 컴포넌트에서 훅 함수를 통해 활용할 수 있게 되었습니다.

<br />
<br />

### useState

함수 컴포넌트 내부에서 상태를 정의하고, 그 상태를 관리할 수 있게 해주는 훅 <br />
매번 실행되는 함수 컴포넌트 환경에서 state 값을 유지하고 사용하기 위해서 클로저를 활용 <br />

#### 게으른 초기화

useState의 인수로 원시값과 같은 변수 대신 함수를 넘기는 것

```js
// example

// 일반적인 useState
const [count, setCount] = useState(
  Number.parseInt(window.localStorage.getItem(cacheKey))
);

// 게으른 초기화
const [count, setCount] = useState(() =>
  Number.parseInt(window.localStorage.getItem(cacheKey))
);
```

<br />

- 공식 문서에서는 초깃값이 복잡하거나 무거운 연산을 포함하고 있을 경우 사용할 것을 권장
- 처음에 state가 만들어질 때에만 실행되며 리렌더링 시에는 함수 실행이 무시됨

<br />
<br />

### useEffect

애플리케이션 내 컴포넌트의 여러 값들을 활용해 동기적으로 부수 효과를 만드는 매커니즘 <br />
이 부수 효과가 '언제' 일어나는지보다 어떤 상태 값과 함께 실행되는지 살펴보는 것이 더 중요

핵심은 의존성 배열의 이전 값과 현재 값의 얕은 비교를 통해 값이 하나라도 변경됐을 경우 callback으로 선언한 부수 효과를 실행

##### 의존성 배열

1. 비었을 때 : 최초 한 번만 실행
2. 없을 때 : 렌더링 때마다 실행
3. 값이 있을 때 : 해당 값에 변경 사항이 있을 때만 실행

##### 주의 사항

1️⃣ 의존성 배열을 빈 채로 작성할 때 한 번 더 생각해보기

    1. 정말로 useEffect의 부수 효과가 컴포넌트의 상태와 별개로 작동해야 하는가?
    2. 이 위치에서 useEffect를 호출하는 것이 최선인가?

<br />

useEffect는 반드시 의존성 배열로 전달한 값의 변경에 의해 실행돼야 하는 훅입니다.

의존성 배열을 넘기지 않은 채 콜백 함수 내부에서 특정 값을 사용한다는 것은, <br />
이 부수 효과가 실제로 관찰해서 실행돼야 하는 값과는 별개로 작동한다는 것을 의미합니다.

빈 배열이 아닐 때에도 특정 값을 사용하지만 해당 값의 변경 시점을 피할 목적이라면 <br />
메모이제이션을 적절히 활용해 해당 값의 변화를 막거나 적당한 실행 위치를 고민해 볼 필요가 있습니다.

<br />

2️⃣ useEffect의 첫 번째 인수에 함수명 부여하기

공식문서에서도 첫 번째 인수로 익명 함수를 전달해줍니다. <br />
useEffect의 수가 적거나 복잡성이 낮다면 문제가 되지 않지만 반대의 경우에는 코드의 파악에 어려움을 줍니다. <br />
이럴 때에는 익명 함수를 기명 함수로 변경해 적절한 이름을 부여해주는 것이 도움이 됩니다.

<br />

3️⃣ useEffect의 크기 최소화하기

useEffect가 언제 실행되는지 명확히 하기 위해 크기를 간결하게 유지하는 것이 좋습니다. <br />
의존성이 변경될 때마다 부수 효과를 실행시키는 useEffect 훅의 특성상 크기가 커질수록 애플리케이션에 악영향을 미칠 수 밖에 없습니다.

따라서 불가피하게 의존성 배열에 여러 변수가 들어가야 하는 상황이라면, <br />
useCallback, useMemo 등으로 정제된 내용들만 useEffect에 담아두는 것이 좋습니다.

<br />

4️⃣ 불필요한 외부 함수 X

useEffect의 크기를 작게 유지하는 것과 같은 맥락입니다. <br />
useEffect 내에서 사용할 부수 효과라면 내부에서 정의해 사용하는 편이 훨씬 낫습니다.

##### Q. useEffect의 인수로 비동기 함수를 받을 수 없는 이유는?

1. state의 경쟁 상태를 야기할 수 있고
2. cleanup 함수의 실행 순서 보장이 되지 않기 때문에

개발자의 편의를 위해 useEffect에서 비동기 함수를 인수로 받지 않고 있습니다.

<br />
<br />

> 📖 메모이제이션 <br />
> 성능 최적화를 위해 함수나 값을 캐싱하여 불필요한 재계산을 방지하는 기법

<br />

useMemo와 useCallback은 성능 최적화와 메모이제이션과 연관된 훅 함수 <br />
메모이제이션을 활용하면 무거운 연산을 다시 수행하는 것을 막을 수 있다는 장점이 있습니다.

두 함수 모두 동일한 역할을 하지만 어떤 것을 메모이제이션하느냐에 차이가 있습니다.

<br />

### useMemo

비용이 큰 연산에 대한 결과를 저장(메모이제이션)해 두고, 이 저장된 값을 반환하는 훅

렌더링 발생 시 의존성 배열의 값 변경 유무에 따라 아래와 같이 동작 :

- 값 변경 X → 함수를 재실행하지 않고 이전에 기억해 둔 해당 값을 반환
- 값 변경 O → 첫 번째 인수의 함수를 실행한 후 그 값을 반환하고 그 값을 다시 기억

<br />
<br />

### useCallback

useCallback은 인수로 넘겨받은 콜백 자체를 기억 <br />
특정 함수를 새로 만들지 않고 다시 재사용한다는 의미

의존성 배열이 변경되지 않는 한 함수를 재생성하지 않습니다. <br />
함수의 재생성을 막아 불필요한 리소스 또는 리렌더링을 방지할 때 사용 가능합니다.

<br />
<br />

### useRef

useRef는 컴포넌트 내부에서 렌더링이 일어나도 변경 가능한 상태값을 저장한다는 점에서 useState와 공통점을 갖습니다.

하지만 useRef는 반환값인 객체 내부에 있는 current로 값에 접근 또는 변경 가능할 뿐만 아니라 그 값이 변하더라도 렌더링을 발생시키지 않는다는 점에서 차이가 있습니다.

이러한 특징을 기반으로 useRef는 useState의 이전 값을 저장하는 `usePrevious()`와 같은 훅 함수를 만드는데 유용하게 쓰일 수 있습니다.

또한 Preact에서 useMemo로 구현이 가능합니다.

<br />
<br />

### useContext

상위 컴포넌트에서 만들어진 context를 함수 컴포넌트에서 사용할 수 있도록 만들어진 훅 <br />

 <br />

여러 개의 provider가 있다면 가장 가까운 Provider의 값을 사용합니다. <br />
다수의 provider와 useContext를 사용할 때, 특히 타입스크립트를 사용하고 있다면 별도 함수로 감싸서 사용하는 것이 좋습니다.

useContext는 상태 관리가 아닌 상태를 주입해 주는 API이므로 단순히 props 값을 하위로 전달해주는 역할을 수행합니다. <br />

또한 부모 컴포넌트가 렌더링되면 하위 컴포넌트는 모두 리렌더링된다는 점을 유의해 사용해야 합니다.<br />
따라서 useContext가 사용되는 범위를 좁게 유지하는 것이 좋습니다. <br />

<br />
<br />

### useReducer

state 값을 변경하는 시나리오를 제하적으로 두고 이에 대한 변경을 빠르게 확인할 수 있게 하는 것이 useReducer의 목적

```js
// sample

// 기본적인 type 형태로 정의
type State = { count: number };
type Action = { type: "up" | "down" | "reset", payload?: State };

// 무거운 연산이 포함된 게으른 초기화 함수
function init(count: State): State {
  return count;
}

// 초깃값
const initialState: State = { count: 0 };

// 앞서 선언한 state와 action을 기반으로 state가 어떻게 변경될지 정의
function reducer(state: State, action: Action): State {
  switch (action.type) {
    case "up":
      return { count: state.count + 1 };
    case "down":
      return { count: state.count - 1 > 0 ? state.count - 1 : 0 };
    case "reset":
      return init(action.payload || { count: 0 });
    default:
      throw new Error(`unexpected action type: ${action.type}`);
  }
}
```

```js
// 사용 예시

export default function App() {
  const [state, dispatcher] = useReducer(reducer, initialState, init);

  function handleUpButtonClick() {
    dispatcher({ type: "up" });
  }
  function handleDownButtonClick() {
    dispatcher({ type: "down" });
  }
  function handleResetButtonClick() {
    dispatcher({ type: "reset", payload: { count: 1 } });
  }

  return (
    <div className="App">
      <h1>{state.count}</h1>
      <button onClick={handleUpButtonClick}>+</button>
      <button onClick={handleDownButtonClick}>-</button>
      <button onClick={handleResetButtonClick}>reset</button>
    </div>
  );
}
```

<br />

##### useState, useReducer

useReducer는 useState의 심화 버전이라고 볼 수 있습니다.

둘은 세부 작동과 쓰임에 차이가 있을 뿐,
결국 클로저를 활용해 값을 가둬 state를 관리한다는 사실에는 변함이 없습니다.

때론 여러 개의 state를 관리하는 것보다 성격이 비슷한 여러 개의 state를 묶어 useReducer로 관리하는 것이 더 효율적일 수 있습니다.

세 번째 인수인 게으른 초기화 함수는 필수 요소는 아니지만 넣어줌으로써
useState에 함수를 넣은 것과 동일한 이점을 누릴 수 있고, <br />
추가로 state에 대한 초기화가 필요할 때 reducer에서 이를 재사용할 수 있다는 장점도 있습니다.

<br />
<br />
