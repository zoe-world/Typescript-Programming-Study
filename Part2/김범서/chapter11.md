# 외부 파일 이용시 declare & 이상한 특징인 ambient module

## `declare` 키워드

`declare` 를 쓰면 이미 정의된 변수나 함수, 타입을 재정의할 수 있다.

```ts
// data.js

var a = 10;
var b = { name: "kim" };
```

```ts
// index.ts

declare let a: number;
console.log(a + 1);
```

다른 파일에 정의되어 있는 변수나 함수를 TS파일에서 에러가 나지 않도록 재정의를 해준다.

`declare` 가 붙은 코드들은 .js 로 변환되지 않는다.  
그냥 컴파일러에게 힌트를 주는 역할이다.

자바스크립트로만 작성된 외부 라이브러리들을 쓸 때 활용 할 수 있다.

tsconfig.json 안에 allowJs 옵션을 true로 켜두면 js파일도 타입지정이 알아서 implicit 하게 된다.

## ambient module (글로벌 모듈)

타입스크립트의 특징으로 `import` `export` 없이도 같은 폴더에 있는 타입이나 변수들을 다른 파일에서 가져다쓸 수 있다.

.ts 파일에 입력한 변수와 타입들은 전부 global 변수 취급을 받는다.  
전역으로 쓸 수 있는 파일을 ambient module 이라고 칭한다.

반면에 `import` 혹은 `export` 키워드가 들어간 ts 파일은 다르다.  
`import` , `export` 키워드가 적어도 하나 있으면 그 파일은 로컬 모듈이 되고  
거기 있는 모든 변수는 `export` 를 해줘야 다른 파일에서 사용할 수 있다.  
그래서 타입스크립트 파일이 다른 파일에 영향끼치는걸 막고싶으면 `export` 키워드를 강제로 추가하면 된다.

```ts
export {};
```

## `declare global`

로컬 모듈에서 전역으로 변수를 만들고 싶을 때 사용 할 수 있다.

```ts
declare global {
  type Dog = string;
}
```

로컬파일의 `declare global` 안에 선언한 타입은 프로젝트 내 모든 파일에서 사용할 수 있다.  
일종의 namespace 문법인데 여기다 적은건 global 이라는 이름의 namespace에 추가된다고 볼 수 있다.
