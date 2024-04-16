# 조건문으로 타입만들기 & infer

## 조건부로 타입만들기

타입조건식은 <span style="color:red">extends 키워드와 삼항연산자</span>를 이용합니다.
"extends는 왼쪽이 오른쪽의 성질을 가지고 있냐" 라는 뜻으로 사용할 수 있기 때문이다.

```
type Age<T> = T extends string ? string : unknown;
let age : Age<string> //age는 string 타입
let age2 : Age<number> //age는 unknown 타입
```

위를 해석해자면, "T라는 파라미터가 string 성질 가지고 있냐? 그러면 string 남기고 아니면 unknown 남겨라"
라는 뜻이다. 아직 타입이 확실하지 않을 때 <타입파라미터> 다룰 때 많이 사용한다.

Q. 그럼 파라미터로 array 자료를 입력하면 array의 첫 자료의 타입을 그대로 남겨주고, array 자료가 아니라 다른걸 입력하면 any 타입을 남겨주는 타입은 어떻게 만들면 될까요?

```
let age1 :FirstItem<string[]>;
let age2 :FirstItem<number>;
```

```
type Age<T> = T extends any[]? T[0] : any

let age1 :FirstItem<string[]>;
let age2 :FirstItem<number>;

```

## infer 키워드

- 조건문에 사용할 수 있는 특별한 infer 키워드가 있는데, infer 키워드는 지금 입력한 타입을 변수로 만들어주는 키워드이다.

```
type Person<T> = T extends infer R ? R : unknown;
type 새타입 = Person<string> // 새타입은 string 타입입니다
```

1. infer 키워드는 조건문 안에서만 사용가능하다.
2. infer 우측에 자유롭게 작명해주면 타입을 T에서 유추해서 R이라는 변수에 집어넣어라 ~라는 뜻이다.
3. R을 조건식 안에서 맘대로 사용이 가능하다.

이런 식으로 타입파라미터에서 타입을 추출해서 쓰고 싶을 때 쓰는 키워드라고 보면 된다.

1. array 안에 있던 타입이 어떤 타입인지 뽑아서 변수로 만들어줄 수 있다.

```
type 타입추출<T> = T extends (infer R)[] ? R : unknown;
type NewType = 타입추출< boolean[] > // NewType 은 boolean 타입
```

2. 함수의 return 타입이 어떤 타입인지 뽑아서 변수로 만들어줄 수 있습니다.

```
type 타입추출<T> = T extends ( ()=> infer R ) ? R : unknown;
type NewType = 타입추출< () => number > // NewType은 number 타입입니다

```

(숙제1) 타입 파라미터로

1. array 타입을 <> 안에 입력하면
2. array 타입의 0번째 타입이 string이면 string 타입을 그대로 남겨주고
3. array 타입의 0번째 타입이 string이 아니면 unknown 을 남겨주려면 어떻게 타입을 만들어놔야할까요?

(동작예시)

```
let age1 :Age<[string, number]>;
let age2 :Age<[boolean, number]>;

```

이러면 age1에 마우스 올렸을 때 타입은 string, age2는 타입이 unknown이 되어야합니다.

이걸 만족하는 type Age를 만들어봅시다.

**정답제출**

```
type Age<T> = T extends [string, ...any] ? T[0] : unknown;

let age1: Age<[string, number]>;
let age2: Age<[boolean, number]>;


```

(숙제2) 함수 타입을 입력하면 함수 파라미터 부분의 타입을 뽑아주는 기계를 만들어보십시오.

```
타입뽑기<(x :number) => void> //이러면 number가 이 자리에 남음
타입뽑기<(x :string) => void> //이러면 string이 이 자리에 남음
```

타입뽑기<> 에다가 () => {} 타입을 넣었는데 소괄호 파라미터 타입이 number인 경우 number를 그 자리에 뱉어줘야합니다.
type 타입뽑기<> 라는 타입을 어떻게 만들어야할까요?

**정답제출**

```
type 타입뽑기<T> = T extends (x: infer R) => any ? R : any;
type a = 타입뽑기<(x: number) => void>; //이러면 number가 이 자리에 남음

```
