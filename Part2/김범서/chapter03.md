# 함수에 사용하는 never 타입도 있긴 합니다

## `never` 타입 특징

- 절대 발생할 수 없는 타입을 나타낸다.
- 함수 표현식이나 화살표 함수 표현식에서 항상 오류를 발생시키거나 절대 반환하지 않는 반환 타입으로 쓰인다.
  1. 절대 return을 하지 않아야 한다
  2. 함수 실행이 끝나지 않아야 한다 (Endpoint가 없어야 한다)
- 모든 타입에 할당 가능한 하위 타입이다.  
  (`never` 타입엔 다른 모든 타입이 할당될 수 없다.)
- 함수의 마지막에 도달할 수 없다.

```ts
function foo2(x: string | number): boolean {
  if (typeof x === "string") {
    return true;
  } else if (typeof x === "number") {
    return false;
  }
  return fail("Unexhaustive!");
}
function fail(message: string): never {
  throw new Error(message);
}

let tmp: any = { id: "123" };
console.log(foo2(tmp));
```

`never` 타입은 모든 타입에 할당 가능한 하위 타입이다.  
따라서 위 코드에서 `foo2` 함수는 `boolean` 타입을 반환해야 하지만 `never` 타입을 반환하는 `fail` 함수의 리턴값을 반환해줄 수 있다.

## `never` 타입이 발생하는 경우

- 함수가 무한히 실행되는경우

```ts
function 함수(): never {
  while (true) {
    console.log(123);
  }
}
```

- 함수가 에러를 발생시키는 경우

```ts
function 함수(): never {
  throw new Error("에러메세지");
}
```

함수에서 에러가 발생하면 함수의 동작이 중간에 멈춘다.  
때문에 함수의 실행이 끝나지 않는 것과 동일해 함수의 반환 타입은 `never` 가 된다.

### 파라미터가 never 타입이 되는 경우

```ts
function myFunction(parameter: string) {
  if (typeof parameter === "string") {
    parameter + 1;
  } else {
    parameter; // never 타입 발생
  }
}
```

`parameter` 는 `string` 타입만 들어올 수 있기 때문에 첫번째 조건문에서 모든 동작이 끝난다.
`else` 는 발생할 수 없기 때문에 `else` 안에서의 `parameter` 의 타입은 `never` 가 된다.

#### 이렇게 해도 에러가 나지 않는다.

```ts
function myFunction(parameter: string) {
  parameter + 1;
}
```

파라미터 타입이 `string` 하나만 들어올 수 있기 때문에 별도의 narrowing이 필요하지 않다.

### 함수 표현식의 never 타입 발생

함수 선언문이 아무것도 return 하지 않고 끝나지도 않을 경우 void 타입이 자동으로 return 타입으로 할당되며,  
함수 표현식이 아무것도 return 하지 않고 끝나지도 않을 경우 never 타입이 자동으로 return 타입으로 할당된다.

```ts
// 함수 선언문
function myFunction() {
  throw new Error();
}

// 함수 표현식
let myFunction2 = function () {
  throw new Error();
};
```

명시적으로 return 타입을 지정해줄 경우 void타입 지정 가능하다.

```ts
let myFunction2 = function (): void {
  throw new Error();
};
```
