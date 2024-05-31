# 함수에 타입 지정하는 법 & void 타입

## 함수의 타입 지정

1. 함수로 들어오는 자료 (파라미터)
2. 함수에서 나가는 자료 (return)

```ts
function myFunction(x: number): number {
  return x * 2;
}
```

### 함수의 `void` 타입

return할 자료가 없는 함수의 타입으로 사용가능하다.

```ts
function printSomething(): void {
  console.log("안녕?");
}

function returnNothing(): void {
  return;
}
```

### 파라미터가 옵션일 경우 (Optional Chaining)

함수의 파라미터가 여부가 옵션일 경우 파라미터 우측에 `?` 표시를 해준다.  
이를 `Optional Chaining` 이라 한다.

```ts
function myFunction(x? :number) {
    ...
}
myFunction(); //가능
myFunction(2); //가능
```

위의 옵셔널 체이닝은 아래의 유니온 타입으로 표현한 방식과 결과가 같다.

```ts
function myFunction(x : number | undefined) {
    ...
}
myFunction(); //가능
myFunction(2); //가능
```

### `Optional Chaining` 활용

- 객체 접근 : `obj?.name` => `obj` 가 `null` , `undefined` 가 아니면 `name` 을 리턴
- 배열 접근 : `arr?.[0]` => `arr` 이 `null` , `undefined` 가 아니면 첫 번째 요소를 리턴
- 함수 호출 : `func?.()` => 함수 `func` 이 `null` , `undefined` 가 아니면 `func` 함수 호출
