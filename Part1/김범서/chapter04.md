# 타입 확정하기 Narrowing & Assertion

## Type Narrowing

Type이 아직 하나로 확정되지 않았을 경우 Type Narrowing 을 써야한다.  
Type Narrowing 은 if문 등으로 타입을 하나로 정해주는 것을 뜻한다.

```ts
function myFunction(x: number | string) {
  if (typeof x === "number") {
    return x + 1;
  } else if (typeof x === "string") {
    return x + 1;
  } else {
    return 0;
  }
}
```

### Type Narrowing 사용시 주의점

```ts
function myFunction(x: number | string): number | string {
  // error: 함수에 끝 return 문이 없으며 반환 형식에 'undefined'가 포함되지 않습니다.
  if (typeof x === "number") {
    return x + 1;
  }
  if (typeof x === "string") {
    return x + 1;
  }
}
```

"함수에 끝 return 문이 없으며 반환 형식에 'undefined'가 포함되지 않습니다." 라는 에러가 발생한다.  
두가지 조건 모두 만족하지 않을경우 아무것도 리턴하지 않아 `undefined` 를 리턴하기 때문에 에러가 발생한다.

아래와 같이 리턴 타입에 만족하는 return을 해주고 끝나도록 하거나,

```ts
function myFunction(x: number | string): number | string {
  if (typeof x === "number") {
    return x + 1;
  }
  if (typeof x === "string") {
    return x + 1;
  }
  return 0;
}
```

리턴 타입에 `undefined` 를 명시 해주면 에러가 발생하지 않는다.

```ts
function myFunction(x: number | string): number | string | undefined {
  if (typeof x === "number") {
    return x + 1;
  }
  if (typeof x === "string") {
    return x + 1;
  }
}
```

## Type Assertion

변수의 타입을 덮어씌워주는 역할을 한다.  
변수명 as string 과 같은 식으로 변수명 뒤에 as 타입명 형식으로 작성해준다.

```ts
function myFunction(x: number | string) {
  return (x as number) + 1;
}
console.log(myFunction(123));
```

### `as` 문법의 용도와 주의사항

1. Narrowing시 사용

   - as 키워드는 union type 같은 복잡한 타입을 하나의 정확한 타입으로 줄이는 역할을 수행한다. (`number` 타입을 `as` 를 이용해 `string` 타입으로 변경하려 한다면 에러가 발생한다.)

2. 입력받을 변수나 파라미터의 타입이 100% 확실할 경우 사용

   - as 문법 사용시 다른 타입이 입력될 경우 에러가 발생하지 않기 때문에 100% 확실한 경우에만 사용한다.

   ```ts
   function myFunction(x: number | string) {
     let array: number[] = []; // array 배열에는 number타입만 들어올 수 있다.
     array[0] = x as number;
   }
   myFunction("123"); // string타입을 입력해도 as number로 인해 number타입으로 인식해 에러 발생 X
   ```

- as 키워드는 엄격한 타입체크기능을 잠깐 안쓰겠다는 뜻과 동일하기 때문에

  1.  왜 타입에러가 나는지 정말 모르겠는 상황에 임시로 에러해결용으로 사용하거나
  2.  내가 어떤 타입이 들어올지 정말 확실하게 알고 있는데 컴파일러 에러가 방해할 때

  사용하도록 하자!
