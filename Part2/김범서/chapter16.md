# 조건문으로 타입만들기 & infer

## 조건부로 타입만들기

타입 조건식은 주로 `extends` 키워드와 삼항연산자를 이용한다.  
"`extends` 는 왼쪽이 오른쪽의 성질을 가지고 있냐" 라는 뜻으로 사용할 수 있기 때문에 조건식의 용도로도 사용할 수 있다.

```
type Age<T> = T extends string ? string : unknown;
let age : Age<string> //age: string 타입
let age2 : Age<number> //age: unknown 타입
```

이처럼 아직 타입이 확실하지 않은 타입파라미터( `<T>` )를 다룰 때 많이 사용하게 된다.

## `infer` 키워드

`infer` 키워드는 지금 입력한 타입을 변수로 만들어주는 키워드다.

```
type Person<T> = T extends infer R ? R : unknown;
type NewType = Person<string> // NewType: string 타입
```

### `infer` 의 특징

1. `infer` 키워드는 조건문 안에서만 사용할 수 있다.
2. `infer` 우측에 자유롭게 작명해주면( `R` ) 타입을 `T` 에서 유추해서 `R` 이라는 변수에 할당된다.
3. `R` 은 조건식 안에서 자유롭게 사용할 수 있다.

### `infer` 의 활용

#### 1. array 안에 있던 타입이 어떤 타입인지 뽑아서 변수로 만들어줄 수 있다.

```
type 타입추출<T> = T extends (infer R)[] ? R : unknown;
type NewType = 타입추출<boolean[]>
```

`(infer R)[]` 이렇게 하면 array가 가지고 있던 타입부분이 `R` 에 할당된다.

#### 2. 함수의 return 타입이 어떤 타입인지 뽑아서 변수로 만들어줄 수 있다.

```
type 타입추출<T> = T extends (()=> infer R) ? R : unknown;
type NewType = 타입추출<() => number>
```

`infer R` 이 함수의 리턴타입 부분에 있기 때문에 `R` 에 `number` 타입이 할당된다.
  
### `ReturnType<>` 
`ReturnType<>` 이라는 예약 타입이 있는데 여기에 함수타입을 넣으면 return 타입만 반환해준다.
