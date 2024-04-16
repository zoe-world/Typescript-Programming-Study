# object 타입 변환기 만들기

## `keyof` 연산자
`keyof` 는 object의 key를 뽑아서 새로운 타입을 만들고 싶을 때 사용하는 연산자다.  
`keyof` 는 object 타입에 사용하면 object 타입이 가지고 있는 모든 key값을 union type으로 합쳐서 내보내준다.  

```
interface Person {
  age: number;
  name: string;
}
type PersonKeys = keyof Person;   // "age" | "name" 타입
let a :PersonKeys = 'age'; // 가능
let b :PersonKeys = 'ageeee'; // 불가능
```

`keyof Person` 은 `"age" | "name"` 타입이 된다.   
따라서 `PersonKeys` 타입은 `"age" | "name"` 이 된다.

## Mapped Types 

object를 다른 타입으로 변환하고 싶을 때 사용 할 수 있다.  
모든 속성들에 문자가 들어오는 타입을  숫자가 들어오도록 바꾸고 싶을 때,   
그럴 땐 처음부터 타입을 다시 작성하는 것이 아니라 mapping을 이용하면 된다.

```
type Car = {
  color: boolean,
  model : boolean,
  price : boolean | number,
};

type TypeChanger <MyType> = {
  [key in keyof MyType]: string;
};
```

`[ 자유작명 in keyof 타입파라미터 ] : 원하는 타입` 형식으로 사용한다.  

`in` 키워드는 왼쪽이 오른쪽에 들어있는지 확인하는 역할을 한다.







 