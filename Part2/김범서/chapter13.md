# `implements` 키워드

class의 타입을 확인하고 싶을 때 `implements` 키워드와 함께 `interface` 문법을 사용할 수 있다.

```
interface CarType {
  model : string,
  price : number
}

class Car implements CarType {
  model : string;
  price : number = 1000;
  constructor(a :string){
    this.model = a
  }
}
let myCar = new Car('morning');
```

`class Car implements CarType` 는  
`Car` 가 `CarType` 의 속성을 전부 가지고 있는지 확인해준다.

## `implements` 는 타입지정문법이 아니다.

```
interface CarType {
  model : string,
  tax : (price :number) => number;
}

class Car implements CarType {
  model;   // model: any 타입
  tax (a){   // a: any 타입
    return a * 0.1;
  }
}
```

`implements` 는 `interface` 에 들어있는 속성을 가지고 있는지 확인만하라는 뜻이다.  
class에 필드와 타입을 할당하고 변형시키는 역할은 하지 않기 때문에 착각하지 않도록 주의해야 한다.

## `implements`를 어디에 쓸까?

클래스끼리 복잡하게 상속하는 경우 이 클래스에 어떤 필드와 함수가 들어있는지 추론하기 힘든 경우가 생긴다.   
그럴때 class가 특정 필드와 함수를 가지고 있는지 확인하고 싶은 경우 사용한다.
