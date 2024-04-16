# object 타입 변환기 만들기

object를 다른 타입으로 변환하고 싶을 때, 처음부터 타입을 다시 작성하는 것이 아니라, mapping을 이용하면 된다.

## keyof 연산자란?

- keyof는 object 타입에 사용하면 **object 타입이 가지고 있는 모든 key 값을 union type으로 합쳐서 보내준다.**
- object의 key를 뽑아서 새로운 타입을 만들고 싶을 때 사용하는 연산자이다.

```
interface Person {
  age: number;
  name: string;
}
type PersonKeys = keyof Person;   //"age" | "name" 타입됩니다
let a :PersonKeys = 'age'; //가능
let b :PersonKeys = 'ageeee'; //불가능
```

Person 타입은 age, name 이라는 key를 가지고 있어서, 이제 PersonKeys는 정말 'age'|'name' 타입이 되므로, literal type이 된다.

```
interface Person {
  [key :string]: number;
}
type PersonKeys = keyof Person;   //string | number 타입됩니다
let a :PersonKeys = 'age'; //가능
let b :PersonKeys = 'ageeee'; //가능
let c :PersonKeys = 124; //가능

```

Person 타입은 모든 문자 key를 가질 수 있게 [key:string] : number 로 타입이 지정되었으므로, keyof Person 이렇게 하면, **string | number** 타입이 된다.

## Mapped Types

- object 안에 있는 속성들을 다른 타입으로 한번에 변환하고 싶을 경우, 유용한 타입 변환기를 만들어보자.

```
type Car = {
  color: boolean,
  model : boolean,
  price : boolean | number,
};
```

위에 모든 속성을 string으로 바꾸고 싶다면 어떻게 해야할까?

```
type Car = {
  color: boolean,
  model : boolean,
  price : boolean | number,
};

type TypeChanger <T> = {
  [P in keyof T]: string;
};

type 새로운타입 = TypeChanger<Car>;

let obj :새로운타입 = {
  color: 'red',
  model : 'kia',
  price : '300',
}
```

[자유작명 in keyof 타입파라미터] : 원하는 타입

in 키워드는 왼쪽이 오른쪽에 들어있냐는 뜻으로 keyof 는 오브젝트 타입에서 key값만 union type으로 뽑아주는 역할로 이해하면 된다.

새로운 타입은 color, model, price 속성을 가지고 있으며 전부 string 타입이 된다.

(숙제1) 다음 타입을 변환기 돌려보십시오.

```
type Bus = {
  color : string,
  model : boolean,
  price : number
}
```

동료가 잘못 만든 타입입니다.

color, model, price 속성은 전부 string 또는 number 타입이어야합니다.

1. 변환기 하나 만드시고

2. 기존 Bus 타입을 변환기 돌려서 위 조건을 충족하는 새로운 타입을 하나 만들어보십시오.

**정답제출**

```
type Bus = {
  color: string;
  model: boolean;
  price: number;
};

type TypeBus<T> = {
  [p in keyof T]: string | number;
};

type result = TypeBus<Bus>;

```

(숙제2) 이런 변환기는 어떻게 만들어야할까요?

object안에 들어있는 모든 속성을
string, number 이렇게 고정된 타입으로 변환해주는게 아니라
내가 원하는 타입을 입력하면 그걸로 변환해주는 범용성 좋은 변환기를 만들어보십시오.

**정답제출**

```
type Bus = {
  color: string;
  model: boolean;
  price: number;
};

type TypeBus<Mytype, T> = {
  [p in keyof Mytype]: T;
};

type result = TypeBus<Bus, boolean>;
type result2 = TypeBus<Bus, number>;

```
