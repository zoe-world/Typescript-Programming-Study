# array 자료에 붙일 수 있는 tuple type

## `Tuple` 타입

`tuple` 타입은 `array` 에 붙일 수 있는 타입이다.  
`tuple` 타입은 자료의 위치까지 정확히 지정할 수 있다.

```ts
let dog: [string, boolean];
dog = ["dog", true];
```

`dog[0]` 은 무조건 `string` , `dog[1]` 은 무조건 `boolean` 만 허용하고 그 외의 타입이 들어올 경우 에러가 발생한다.

## `tuple` 타입의 옵션 지정

`?` 를 사용해 타입의 옵션 여부를 지정 할 수 있다.

```ts
type Num = [number, number?, number?];
let var1: Num = [10];
let var2: Num = [10, 20];
let var3: Num = [10, 20, 10];
```

주의사항으로 옵션 타입은 맨 뒤에만 사용할 수 있다.

```ts
type Num = [number, number?, number]; // 필수 요소는 선택적 요소 뒤에 올 수 없습니다.
```

옵션 타입을 2개 쓰고 싶을 경우 맨뒤의 2개를 옵션타입으로 지정해야 한다.

## `Tuple` 응용 : rest parameter

rest parameter는 배열에 담겨오기 때문에 array 처럼 타입지정을 해준다.  
함수의 rest parameter에도 `tuple` 타입을 사용 할 수 있다.

```ts
function 함수(...x: [string, number]) {
  console.log(x);
}

함수("kim", 123); // 가능
함수("kim", 123, 456); // 에러
함수("kim", "park"); // 에러
```

## array 두개를 spread 연산자로 합치는 경우 타입지정은?

배열을 spread 연산자를 통해 합쳤을 경우 해당하는 부분의 타입에 `...` 을 사용해 타입을 지정 할 수 있다.

```ts
let arr = [1, 2, 3];
let arr2: [number, number, ...number[]] = [4, 5, ...arr];
```

rest parameter 와 같은 느낌으로 이 부분에 몇개의 자료가 들어올지 모른다는 표현이 가능하다.

## 문제

다음과 같은 문자/숫자 분류기 함수를 만들어보십시오.  
파라미터 중 문자만 모아서 [] 에 담아주고, 숫자만 모아서 [] 에 담아주는 함수가 필요합니다.  
문자 숫자 외의 자료는 입력불가능하고 파라미터 갯수 제한은 일단 없습니다.  
함수 만들어보시고 함수의 파라미터/return 타입지정도 확실하게 해봅시다.

### 동작예시

함수('b', 5, 6, 8, 'a') 이렇게 사용할 경우 이 자리에 [ ['b', 'a'], [5, 6, 8] ] 이 return 되어야합니다.

```ts
function solution(...x: (string | number)[]): [string[], number[]] {
  let strArr: string[] = [];
  let numArr: number[] = [];

  x.forEach((v) => {
    if (typeof v === "string") strArr.push(v);
    else numArr.push(v);
  });

  console.log([strArr, numArr]);
  return [strArr, numArr];
}

solution("b", 5, 6, 8, "a"); //  [ ['b', 'a'], [5, 6, 8] ]
```
