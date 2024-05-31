# React + TypeScript 사용할 때 알아야할 점

## JSX 타입

React에서 Html태그가 들어있는 자료는 JSX라고 부르는 자료가 된다.  
이 자료의 타입은 `JSX.Element` 가 된다.

```tsx
let 박스: JSX.Element = <div></div>;
let 버튼: JSX.Element = <button></button>;
```

## function component 타입지정

`props` 는 object의 형식으로 저장되기 때문에 object의 타입을 지정하는 것과 동일한 방식으로 타입지정을 할 수 있다.

또한 컴포넌트는 JSX를 리턴하기 때문에 리턴 타입은 `JSX.Element` 가 된다.

```tsx
type AppProps = {
  name: string;
};

function App(props: AppProps): JSX.Element {
  return <div>{message}</div>;
}
```

## state 문법 사용시 타입지정

Generic 문법을 이용해 useState함수의 타입을 지정 할 수 있다.

```ts
const [user, setUser] = useState<string | null>("kim");
```
