# Object에 타입지정하는 방법

## Object에 쓸 수 있는 interface 문법

interface 문법을 사용하면, object 자료형의 타입을 편리하게 지정이 가능하다.

```
interface Square {
  color :string,
  width :number,
}

let 네모 :Square = { color : 'red', width : 100 }

```

interface는 object랑 비슷한 모습으로 작성하면 된다
type alias 와 용도와 기능이 똑같다.

1. 대문자로 작명하기
2. {} 안에 속성값의 타입을 명시해주면 된다.

그렇다면 왜 type 말고 interface가 object 타입지정에 편리할까?

## interface 는 extends 가능하다.

```
interface Student {
    name:string
}
interface Teacher {
    name:string,
    age:number
}
```

위와 같이 Student 와 Teacher 를 만들었는데,
name 속성이 중복이 된 것을 확인할 수 있다.
중복을 줄이고 싶다면? 어떻게 쓰면 될까?

extends 문법을 쓰면, 중복을 줄일 수 있다.
extends 문법은 interface 를 여기에 복사해달라는 뜻이다.

```
interface Student{
    name: string,
}
interface Teacher extends Student{
    age: number
}
```

위와 같이 쓰면,
Teacher 인터페이스 에다가 Student 을 복사해달라는 뜻

그러게 되면, Teacher 타입은 age, name 속성을 다 가지고 있다.

## type 키워드와의 차이점 - extends

type alias 와 interface는 거의 같은 기능을 제공한다.
다만, 차이점이 있는데 extends 문법이 약간 다르다는 것이다.

```
interface Animal {
  name :string
}
interface Cat extends Animal {
  legs :number
}
```

interface 의 경우 일반적으로 위와 같이 extends 한다.

```
type Animal = {
  name :string
}
type Cat = Animal & { legs: number }
```

type alias의 경우 extends는 안되고 & 기호를 사용하면 object 두개를 합칠 수 잇다.

위와 같은 경우 Cat 은 name, legs 속성을 갖게 된다.

### interface도 type처럼 & 기호를 이용해도 복사가능

```
interface Student {
  name :string,
}
interface Teacher {
  age :number
}

let 변수 :Student & Teacher = { name : 'kim', age : 90 }
```

& 기호 쓰는 걸 intersection이라고 부르는데, extends와 유사하게 사용가능하다.
(주의) extends 쓸 때 타입끼리 중복속성이 발견될 경우 에러로 혼내주는데 & 쓰면 때에 따라 아닐 수도 있습니다.

## 타입이름 중복선언시

### interface 경우

```
interface Animal {
  name :string
}
interface Animal {
  legs :number
}
```

interface의 경우 타입이름 중복선언을 허용해주며, 중복시 extends 한 것이랑 동일하게 작동한다.
Animal 타입은 name, legs 속성을 가질 수 있게 된다.

### type 경우

```
type Animal = {
  name :string
}
type Animal = {
  legs :number
}
```

위와 같이 적용하면, 에러가 난다.
type의 경우 중복 선언을 허용하지 않는다.

## extends 할 때 object 안의 속성이 중복될 경우

```
interface Animal {
  name :string
}
interface Dog extends Animal {
  name :number
}

interface Animal {
  name :string
}
interface Dog {
  name :number
}

let 변수 :Dog & Animal = { name : '멍멍' }

```

위와 같이 name 속성이 중복될 경우 에러가 나는데, 그 이유는 타입이 string, number 로 다르기 때문이다. 둘 다 string일 경우 에러가 나지 않고 하나로 합쳐준다.
