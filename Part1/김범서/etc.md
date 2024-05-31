# Typescript 필수문법 10분 정리와 설치 셋팅

## 기본 설치
NodeJs 설치 후 터미널에 명령어를 입력해준다.
```
npm install -g typescript 
```
**React** 
```
npx create-react-app my-app --template typescript
```
## TS -> JS 컴파일
.ts파일은 브라우저에서 인식할수 없어 .js파일로 변환 해주는 과정이 필요하다.  
작업폴더경로 -> cmd -> "tsc -w" 명령어 입력 시   
.ts 파일을 .js파일로 컴파일 해준다.

## tsconfig.json 쓸만한 설정들 모음
```json
{
 "compilerOptions": {

  "target": "es5", 
    // 'es3', 'es5', 'es2015', 'es2016', 'es2017','es2018', 'esnext' 가능
  "module": "commonjs", 
    //무슨 import 문법 쓸건지 'commonjs', 'amd', 'es2015', 'esnext'
  "allowJs": true, 
    // js 파일들 ts에서 import해서 쓸 수 있는지 
  "checkJs": true, 
    // 일반 js 파일에서도 에러체크 여부 
  "jsx": "preserve", 
    // tsx 파일을 jsx로 어떻게 컴파일할 것인지 'preserve', 'react-native', 'react'
  "declaration": true, 
    //컴파일시 .d.ts 파일도 자동으로 함께생성 (현재쓰는 모든 타입이 정의된 파일)
  "outFile": "./", 
    //모든 ts파일을 js파일 하나로 컴파일해줌 (module이 none, amd, system일 때만 가능)
  "outDir": "./", 
    //js파일 아웃풋 경로바꾸기
  "rootDir": "./", 
    //루트경로 바꾸기 (js 파일 아웃풋 경로에 영향줌)
  "removeComments": true, 
    //컴파일시 주석제거 

  "strict": true, 
    //strict 관련, noimplicit 어쩌구 관련 모드 전부 켜기
  "noImplicitAny": true, 
    //any타입 금지 여부
  "strictNullChecks": true, 
    //null, undefined 타입에 이상한 짓 할시 에러내기 
  "strictFunctionTypes": true, 
    //함수파라미터 타입체크 강하게 
  "strictPropertyInitialization": true, 
    //class constructor 작성시 타입체크 강하게
  "noImplicitThis": true, 
    //this 키워드가 any 타입일 경우 에러내기
  "alwaysStrict": true, 
    //자바스크립트 "use strict" 모드 켜기

  "noUnusedLocals": true, 
    //쓰지않는 지역변수 있으면 에러내기
  "noUnusedParameters": true, 
    //쓰지않는 파라미터 있으면 에러내기
  "noImplicitReturns": true, 
    //함수에서 return 빼먹으면 에러내기 
  "noFallthroughCasesInSwitch": true, 
    //switch문 이상하면 에러내기 
 }
}
```

## ts-node
기존 JS파일을 콘솔창에서 실행시키기 위해서는 NodeJs를 설치하면 된다.  
하지만 TS파일을 실행시키면 node가 타입스크립트를 해석하지 못해 실행이 되지 않는다.  

때문에 터미널에 아래와 같이 ts-node Install을 해줘야 한다.
```
npm install -g ts-node
```
