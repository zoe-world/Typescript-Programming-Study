# 함수와 methods에 type alias 지정하는 법

## 함수타입 표현방법

함수를 type alias를 만들어 타입지정을 해주고 싶으면 함수표현식을 통해 타입선언을 해줄 수 있다.

```ts
type NumOutType = (x: number, y: number) => number;

let NumOut: NumOutType = (x, y) => {
  return x + y;
};
```

## methods 안에 타입지정하기

object 안에 함수를 선언할 수 있다.  
object의 타입을 선언해줄때도 마찬가지로 함수표현식으로 아래와 같이 표현할 수 있다.

```ts
type Member = {
  name: string;
  age: number;
  plusOne: (x: number) => number;
  changeName: () => void;
};

let 회원정보: Member = {
  name: "kim",
  age: 30,
  plusOne(x) {
    console.log(x + 1);
    return x + 1;
  },
  changeName: () => {
    console.log("안녕");
  },
};
회원정보.plusOne(1);
회원정보.changeName();
```

## 콜백함수와 타입선언

### 문제 1

다음 함수2개를 만들어보고 타입까지 정의해보십시오.

- cutZero()라는 함수를 만듭시다. 이 함수는 문자를 하나 입력하면 맨 앞에 '0' 문자가 있으면 제거하고 문자 type으로 return 해줍니다.
- removeDash()라는 함수를 만듭시다. 이 함수는 문자를 하나 입력하면 대시기호 '-' 가 있으면 전부 제거해주고 그걸 숫자 type으로 return 해줍니다.
- 함수에 타입지정시 type alias를 꼭 써보도록 합시다.

```ts
// cutZero

type CutZeroType = (str: string) => string;

const cutZero: CutZeroType = (str) => {
  if (str[0] === "0") {
    return str.slice(1);
  }
  return str;
};

// removeDash

type removeDashType = (str: string) => number;

const removeDash: removeDashType = (str) => {
  const result = str.replace(/-/g, "");
  return parseInt(result);
};
```

### 문제 2

함수에 함수를 집어넣고 싶습니다.  
숙제2에서 만든 함수들을 파라미터로 넣을 수 있는 함수를 제작하고 싶은 것입니다.  
이 함수는 파라미터 3개가 들어가는데 첫째는 문자, 둘째는 함수, 셋째는 함수를 집어넣을 수 있습니다.

이 함수를 실행하면

1. 첫째 파라미터를 둘째 파라미터 (함수)에 파라미터로 집어넣어줍니다.
2. 둘째 파라미터 (함수)에서 return된 결과를 셋째 파라미터(함수)에 집어넣어줍니다.
3. 셋째 파라미터 (함수)에서 return된 결과를 콘솔창에 출력해줍니다.

이 함수는 어떻게 만들면 될까요?  
둘째 파라미터엔 cutZero, 셋째 파라미터엔 removeDash 라는 함수들만 입력할 수 있게 파라미터의 타입도 지정해봅시다.

```ts
type myFunctionType = (
  str: string,
  아무이름이나되는디: CutZeroType,
  순서따라지정되는디: removeDashType
) => number;

const myFunction: myFunctionType = (str, func1, func2) => {
  const result = func2(func1(str));

  console.log("myFunction : ", result);
  return result;
};
```

#### 타입지정시 매개변수의 이름

JavaScript와 TypeScript에서 함수의 매개변수는 순서에 따라 결정되기 때문에 이름은 중요하지 않습니다.  
즉, 함수가 호출될 때 매개변수의 값은 함수 정의에서 선언된 순서대로 전달됩니다.  
따라서 TypeScript에서는 함수 타입을 정의할 때 매개변수의 이름을 지정하는 것은 필요하지 않습니다.  
대신, 매개변수의 타입과 그 순서를 지정하는 것이 중요합니다.  
이렇게 함으로써 함수의 타입을 명확하게 정의할 수 있고,  
이후 함수를 호출할 때도 매개변수의 순서를 따라서 값을 전달하면 됩니다.  
즉, myFunctionType에서 매개변수의 이름은 단지 가독성을 위한 것이며,  
실제로는 매개변수의 순서와 타입에 따라 함수가 동작하게 됩니다.
