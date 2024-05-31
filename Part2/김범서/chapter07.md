# 타입을 파라미터로 입력하는 Generic

## Generic 함수

Generic 함수는 파라미터로 타입을 입력하는 함수다.

함수에 `<>` 괄호를 열면 파라미터를 입력할 수 있다.  
타입파라미터 문법으로 이 괄호 안에는 타입만 입력할 수 있다.

```ts
function myFunction<MyType>(x: MyType[]): MyType {
  return x[0];
}

let a = myFunction<number>([4, 2]);
let b = myFunction<string>(["kim", "park"]);
```

`myFunction<number>()` 를 사용하게 되면 `MyType` 에는 `number` 타입이 할당된다.

## Generic 타입 제한하기 (constraints)

`extends` 문법을 사용해 넣을 수 있는 타입을 제한할 수 있다.

```ts
function myFunction<MyType extends number>(x: MyType) {
  return x - 1;
}

let a = myFunction<number>(100);
```

`interface` 문법에 쓰는 `extends` 와는 다른 동작을 한다.  
`MyType` 이 `number` 타입을 가지고 있는지 체크해준다.  
이 또한 일종의 Narrowing으로 인정된다.

## 커스텀 타입 `extends`

문자로 파라미터를 넣으면 자릿수를 세어서 출력해주는 함수를 Generic으로 만들었다.

```ts
function myFunction<MyType>(x: MyType) {
  return x.length; // 'MyType' 형식에 'length' 속성이 없습니다.
}

let a = myFunction<string>("hello");
```

`string` 타입에 `.length` 를 사용했지만 에러가 발생한다.  
이유는 `MyType` 에 `string` 을 넣었지만 나중에 `number` 타입과 같은 다른 타입이 들어올 수도 있기 때문에 조작을 일단 방지해준다.

```ts
interface lengthCheck {
  length: number;
}

function myFunction<MyType extends lengthCheck>(x: MyType) {
  return x.length;
}

let a = myFunction<string>("hello"); // 가능
let a = myFunction<number>(1234); // 'number' 형식이 'lengthCheck' 제약 조건을 만족하지 않습니다.
```

1. `length` 속성을 가지고 있는 타입을 생성한다.
2. 타입을 `extends` 해주면 `MyType` 도 `length` 속성을 복사해서 가지게 된다.
3. `MyType` 은 `length` 가 분명히 있기 때문에 맘대로 `MyType` 을 부여받은 `x` 는 `.length` 조작이 가능하게 된다.

lengthCheck를 포함하지 않는 타입이 들어오게 되면  
" 'number' 형식이 'lengthCheck' 제약 조건을 만족하지 않습니다. " 라는 에러 메세지가 발생한다.

## 기타 Generic 활용

class도 `class<MyType> {}` 과 같이 만들면 객체를 생성할 때 타입파라미터를 넣을 수 있다.

`type Age<MyType> = MyType` 처럼 타입 변수에도 사용 할 수 있다.

## 문제

`data` 라는 JSON 자료를 object 자료로 변환을 해서 return 해주는 함수를 만들어보십시오.  
근데 변환된 object의 타입은 `Animal` 이 되었으면 좋겠는데 어떻게 코드를 짜면 될까요?  
오늘 배운 Generic을 이용해서 구현해보도록 합시다.

(동작 예시)  
`함수<Animal>(data)` 이렇게 쓰면 이 자리에 `{ name : 'dog' , age : 1 }` 이런 object 자료가 남아야합니다.  
타입은 `Animal` 입니다.

```ts
interface Animal {
  name: string;
  age: number;
}

function chgJsonToObject<T>(json: string): T {
  console.log(JSON.parse(json));
  return JSON.parse(json);
}

let data = '{"name" : "dog", "age" : 1 }';

chgJsonToObject<Animal>(data);
```
