# class 만들 때 타입지정

## 필드값 타입지정

class 내부에는 모든 자식 object들이 사용 가능한 속성을 만들 수 있다.

예를 들어, 모든 Person 클래스의 자식들에게 data 라는 속성을 부여해보자.

```
class Person {
  data:number = 0;
}

let john = new Person();
john.data = 1;
```

class 중괄호 안에다가 변수처럼 만든 data 를 필드라고 부른다.
data 필드값 옆에 타입지정을 할 수 있다.

## constructor 타입지정

```
class Person {
  name;
  age;
  constructor (a:string){
    this.name = a;
    this.age = 20;
  }
}
```

constructor 에서 this.필드값 쓰기 위해선, 미리 필드값이 정의되어 있어야한다.

미리 정의된 필드값에 파라미터를 넣어주면 되는데,
a 라는 파라미터에는 함수처럼 지정해주고 싶은 타입을 파라미터:타입 이렇게 적어줘야한다.

이외에 다른 방법을 사용할 수 있다.

```
class Person {
  name;
  age;
  constructor ( a = 'kim' ){
    this.name = a;
    this.age = 20;
  }
}
```

함수 문법 중에 기본 파라미터라는 것이 있는데, (default parameter) 파라미터에 값을 입력 안하면 자동으로 할당해주는 것을 의미하는데, 파라미터 = 자료 이런 식으로 쓴다.
이런 거를 활용하면 타입지정을 할 필요가 없다.

참고로 constructor 함수는 return 타입지정을 하면 안된다. 이유는?constructor를 사용하는 이유는 object 자료를 생성하기 위해서 이기 때문에, return 타입을 지정할 필요가 없다.

Q. 필드값이랑 constructor랑 똑같은 역할이네요? 근데 왜 구분이 되어있을까?
new Person() 이라고 새로운 변수에 할당을 해서 사용할 때, 파라미터로 새로운 것을 집어넣고 싶다면, constructor를 사용해야 한다.
