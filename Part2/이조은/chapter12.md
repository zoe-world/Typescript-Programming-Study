# d.ts 파일 이용하기

## d.ts 파일이란?

- type을 정의하기 위해서 존재하는 파일

사용하는 이유는?

1. 타입 정의만 따로 저장해서 import하기 위해서,
2. 프로젝트에서 사용하는 타입을 쭉 정리하는 래퍼런스용으로 사용

## 타입만 모아 정의하는 d.ts

- .d.ts 파일은 타입 정의만 넣을 수 있다.
- type 키워드, interface 같은 타입 정의만 넣을 수 있다.

  - 함수의 경우 {} 중괄호 붙이는 게 불가능하고, 파라미터 & return 타입만 지정 가능하다.

    ```
    export type Age = number;
    export type multiply = (x :number ,y :number) => number
    export interface Person { name : string }
    ```

  - 정의해놓은 타입은 export 해서 써야한다. d.ts 파일은 ts 파일이 아니기 때문에, 그냥 써도 ambient module이 되지 않는다.
  - 한번에 많은 타입을 export 하고 싶은 경우, namespace에 담거나, import \* as 문법을 쓰면 된다.

## 래퍼런스용 d.ts 파일

ts 파일마다 d.ts 파일을 자동생성하면 되는데, tsconfig 에다가 declaration 옵션을 true로 바꿔주면 된다. 그럼 저장시 자동으로 ts파일마다 d.ts파일이 옆에 생성된다

```
(tsconfig.json)

{
    "compilerOptions": {
        "target": "es5",
        "module": "es6",
        "declaration": true,
    }
}
```

**예시**

```
(index.ts)

let 이름 :string = 'kim';
let 나이 = 20;
interface Person { name : string }
let 사람 :Person = { name : 'park' }
```

```
(index.d.ts)

declare let 이름: string;
declare let 나이: number;
interface Person {
    name: string;
}
declare let 사람: Person;
```

## export 없이 d.ts 파일을 글로벌 모듈로 만들기

d.ts 파일은 import 와 export가 없어도 로컬 모듈이다.
그래서 다른 타입스크립트 파일에서 import를 통해서만 사용이 가능한데, 옵션을 통해 글로벌 모듈로 만들 수 있다.

```
// tsconfig.json

{
    "compilerOptions": {
        // ...
        "typeRoots": ["./types"]
    }
}
```

typeRoots 옵션에 원하는 폴더를 입력한다. 타입스크립트 파일을 작성할 때 타입이 없으면 자동으로 해당 폴더에서 타입을 찾아서 적용해준다.

하지만 파일이 많아지면 겹치는 문제나, 신경 쓰는 부분이 많아져서 되도록이면 import / export 방식을 추천한다.
