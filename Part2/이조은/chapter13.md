# implements 키워드

## implements란?

- interface는 object 타입을 지정할 때 사용하는 키워드이다.
- 또한 class 타입을 확인하고 싶을 때, interface 문법을 사용할 수 있는데, 그때 필요한 것이 **implements** 라는 키워드이다.

**코드예시**

```
class Car {
  model : string;
  price : number = 1000;
  constructor(a :string){
    this.model = a
  }
}
let 붕붕이 = new Car('morning');
```

- class Car로부터 생성되는 object들은 model, price 속성을 가지게 된다.  
  만약 class가 model,price 속성을 가지고 있는지 타입으로 확인하고 싶으면 그때, **interface + implements**키워드로 확인하면 된다.

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
let 붕붕이 = new Car('morning');
```

위와 같이 class 이름 우측에 implements 를 쓰고, interface 이름을 쓰면, '이 class가 이 interface에 있는 속성을 다 들고있냐' 라고 확인이 가능하다. 혹여나 빠진 속성이 있으면 에러로 알려준다.

## implements는 타입지정문법이 아닙니다

- implements 라는 건 interface에 들어있는 속성을 가지고 있는지 확인만 하라는 뜻이고, class에다가 필드와 타입을 할당하고 변형시키는 역할은 아니다.

```
interface CarType {
  model : string,
  tax : (price :number) => number;
}

class Car implements CarType {
  model;   ///any 타입됨
  tax (a){   ///a 파라미터는 any 타입됨
    return a * 0.1
  }
}
```

class Car에다가 CarType 이 implements 되어있냐고 확인하는 문법을 위에 적어놓았다.
CarType에 있던 model 은 string형이고, Car 안에 있는 model은 타입지정이 안되어있다. 그렇다면? class Car안에 있는 model은 무슨형일까? **any** 타입

> any 타입인 이유는 implements는 class의 타입을 체크해주는 용도이지 할당하는게 아니기 때문이다.

## implements은 어디에 쓸까?

- **class가 특정 필드와 함수를 가지고 있는지 확인하고 싶은 경우**, 간혹 사용한다.
  예를 들어 클래스끼리 복잡하게 상속하고 그런 경우에 이 클래스에 어떤 필드와 함수가 들어 있는지, 추론하기 힘든 경우가 많기 떄문이다.
