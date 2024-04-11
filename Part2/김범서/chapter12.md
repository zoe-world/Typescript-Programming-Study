# d.ts 파일 이용하기

이 파일은 타입만 저장할 수 있는 파일형식으로, .js파일로 컴파일되지 않는다.

```
export type Age = number;
export type multiply = (x :number ,y :number) => number
export interface Person { name : string }
```

## 타입저장용으로 d.ts 사용하기

1. 정의해둔 타입은 `export` 해서 사용한다.  
   d.ts 파일은 ts 파일이 아니기 때문에 그냥 써도 ambient module이 되지 않는다.  
   그래서 `export` 를 추가해줘야 다른 ts 파일에서 가져다쓸 수 있다.

2. 한 번에 많은 타입을 `export` 하고 싶은 경우 `namespace` 에 담아서 사용하거나,  
   `import * as ~~~` 방식으로 `import` 할 수 있다.

## 레퍼런스용으로 d.ts 사용하기

tsconfig 파일에 `declaration` 옵션을 `true` 로 설정한다.
파일을 저장하면 자동으로 ts파일마다 d.ts 파일이 생성된다.

```
// tsconfig.json

{
    "compilerOptions": {
        "declaration": true,
    }
}
```

`~~~.d.ts` 라는 파일엔 `~~~.ts` 파일에 있는 모든 변수와 함수 타입정의가 들어있다.  
자동생성의 경우 따로 수정할 수 없고, 레퍼런스용으로 사용하면 된다.

## 유명한 JS 라이브러리들은 d.ts 파일을 제공한다.

- [Definitely Typed](https://github.com/DefinitelyTyped/DefinitelyTyped) 는
  github repository로 대부분 라이브러리의 타입정의 파일을 찾을 수 있다.

- npm으로 라이브러리 설치시 타입스크립트 타입이 정의된 버전을 따로 찾아서 설치할 수 있다.  
  [TypeSearch](https://www.typescriptlang.org/dt/search?search=) 에서 타입이 정의된 npm 패키지를 검색 할 수 있다.   
  타입이 정의된 라이브러리를 npm으로 설치하면 node_modules/@types 경로에 타입이 설치된다.  
  그리고 타입스크립트 컴파일러는 자동으로 여기 있는 타입 파일을 참고해서 타입을 가져온다.
