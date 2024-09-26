# 리액트 훅 깊게 살펴보기

### Reach Hook

- 함수 컴포넌트가 상태를 사용하기 위해
- 클래스 컴포넌트의 생명주기 메서드를 대체하기 위해
- 클래스 컴포넌트에서만 사용 가능하던 기능(state, ref 등)을 함수도 사용할 수 있게 하기 위해

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
