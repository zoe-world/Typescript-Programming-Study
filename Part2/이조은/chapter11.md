# 외부 파일 이용시 declare & 이상한 특징인 ambient module

- 타입스크립트 프로젝트 작업 중에 외부 자바스크립트 파일을 이용해야 하는 경우,

```
(data.js)

var a = 10;
var b = {name :'kim'};
```

```
(index.ts)

console.log(a + 1);
```

만약 간단한 개발시에 index.html에 파일을 직접 입력하면 된다.

```
(index.html)

<script src="data.js"></script>
<script src="index.js"></script>  //index.ts에서 변환된 js 파일

```

- 위와 같이 입력하면, 콘솔창에 11이 잘 나오지만, 타입스크립트에선 a가 정의가 안되어있다고 에러가 뜬다.
  어떻게 해결할 수 있을까?

## declare 키워드로 재정의하기

```
(data.js)

var a = 10;
var b = {name :'kim'};
```

```
(index.ts)

declare let a :number;
console.log(a + 1);

```

- declare 우측에 새로 정의하고 싶은 변수를 적으면 된다.

## TS의 이상한 특징 : Ambient Module

타입스크립트는 a.ts에 있던 변수나 타입정의를 b.ts에서도 아무런 설정없이 그냥 쓸 수 있는 특징이 있다.

```
(data.ts)

type Age = number;
let 나이 :Age = 20;
```

```
(index.ts)

console.log(나이 + 1) //가능
let 철수 :Age = 30; //가능
```

- data.ts 에서 타입, 변수를 만들면, index.ts에서도 자유롭게 사용 가능하다.

- ts 파일에 입력한 변수와 타입들은 전부 global 변수 취급을 받는데, 전역으로 쓸 수 있는 파일을 전문용어로 ambient module 이라고 칭한다.

**그러나, import 혹은 export 키워드가 하나라도 있으면, 해당 ts파일은 로컬 모듈로 변한다.**

거기 있는 모든 변수는 export를 해워쟈 다른 파일에서 사용이 가능하기 때문에, 다른 파일에 영향끼치는 걸 막으려면 export 키워드를 추가하면 된다.

## declare global

ts 파일에 import export 문법이 있으면 로컬 모듈이라고 앞서 말했는데, 로컬 모듈에서 전역으로 변수를 만들고 싶으면 어떻게 해야할까? **declare global** 붙여서 만들기

```
declare global {
  type Dog = string;
}
```
- 위와 같이 declare global 안에 선언된 type Dog은 모든 파일에서 사용이 가능하다.
- 이것은 일종의 namespace 문법인데, 여기다 적은 건 global 이라는 이름의 namespace 에 추가된다고 생각하면 된다.