# React + TypeScript 사용할 때 알아야할 점

타입스크립트를 도입해야하는 경우

- 프로젝트 사이즈가 큰 경우

- 협업시 다른 사람이 짠 코드를 참조할 일이 많은 경우

- 장기적으로 유지보수에 도움이 필요한 경우

- 나중에 팀원이 더 필요해도 인력수급이 쉽게 가능한 경우

- 팀원들 학습에 필요한 시간과 비용이 적게 드는 경우

> 왠만하면 타입스크립트 사용하자

리액트에서 TS 문법을 어디에 써야하는지 4개로 정리해보자.

## 1. 일반 변수, 함수 타입지정

**변수 타입지정**

```
let 이름 :string = 'kim';
let 나이 :number = 20;
let 결혼했니 :boolean = false;
let 회원들 :string[] = ['kim', 'park'];
let 내정보 : { age : number } = { age : 20 };

let 어레이 : (string | number)[] = [1,"2", 3];
let 오브젝트 : {data : (string | number)} = {data: '123'}
```

**함수 타입지정**

```
// 파라미터와 리턴 두곳에 타입 지정
function 내함수(x :number) :number {
  return x * 2
}

// return 할 자료가 없는 경우
function 내함수(x :number) :void {
  return x * 2 //여기서 에러남
}

// 파라미터가 옵션인 경우
function 내함수(x? :number) {
}
내함수(); //가능
내함수(2); //가능

// union type인 경우
function 내함수(x :number | string){
  if (typeof x === 'number') {
    return x + 1
  }
  else if (typeof x === 'string') {
    return x + 1
  }
  else {
    return 0
  }
}
```

## JSX 타입지정

리액트에서는 변수나 자료에 <div></div> 이런걸
담아서 사용할 수 있다. 이런 자료를 타입지정하고 싶으면, JSX.Element 라는 타입을 사용하면 된다.

```
let 박스 :JSX.Element = <div></div>
let 버튼 :JSX.Element = <button></button>
```

## function component 타입지정

```
function App () {
  return (
    <div>안녕하세요</div>
  )
}
```

리액트의 컴포넌트는 위와 같이 생겼는데, 컴포넌트의 타입지정은 어떻게 할까? 컴포넌트도 함수이기 때문에, 파라미터와 return 타입지정을 하면된다.

- 파라미터는 항상 props 때문에, prop가 어떻게 생겼는지, 조사해서 타입지정을 하면된다.
- 컴포넌트는 JSX를 return 하는데, return 타입에는 어떤 타입을 기입해야될까? JSX>Element 를 써주면 된다.

```
type AppProps = {
  name: string;
};

function App (props: AppProps) :JSX.Element {
  return (
    <div>{message}</div>
  )
}
```

### pros로 JSX를 입력할 수 있게 코드를 짜는 경우도 있다.

- 이럴 때는 JSX.IntrinsicElements라는 이름의 타입을 사용가능하다. `<div> <a> <h4>` 같은 기본 태그들을 표현해주는 타입을 사용해서 넣을 수 있다.

```
type ContainerProps = {
  a: JSX.IntrinsicElements['h4'];
};

function Container (props: ContainerProps) {
  return (
    <div>{props.a}</div>
  )
}
```

## 4. state 문법 사용시 타입지정

```
const [user, setUser] = useState<string | null>('kim');
```

Generic 문법을 이용해서 타입을 useState함수에 넣는 형태로 설정하면 된다.

## 5. type assertion 문법 사용할 때

assertin 을 하고 싶으면, as 또는 <>를 쓰면 되는데, 리액트에선 컴포넌트로 오해할 수 있어서, 꺾쇠 괄호는 리액트에서 쓰지 않는다.
