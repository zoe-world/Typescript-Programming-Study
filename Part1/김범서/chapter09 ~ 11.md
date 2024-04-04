# class 키워드 알아보기

## class 사용하는 이유

비슷한 object 자료를 여러개 만들고 싶을 때 사용한다.

```
class People {
  name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}

const 김범서 = new People("김범서", 26);
console.log(김범서); // People { name: '김범서', age: 26 }
```

## class 생성 및 할당

- `class` 의 `constructor` 안에 생성할 object를 넣어준다.

- 타입스크립트에서는 `constructor` 안에 `this.속성명` 을 선언해주기 전에 미리 선언 및 타입을 지정해줘야 한다.

- `constructor` 에서 받아올 파라미터는 함수와 동일하게 타입을 지정해준다.

- 생성한 클래스는

  ```
  new People("김범서", 26);
  ```

  와 같이 `new` 키워드를 사용해 함수처럼 사용할 수 있다.

- `constructor` 안에서의 this는 `People` 클래스를 가르킨다.

## class 필드값

class 내부에는 모든 자식 object들이 사용가능한 속성을 만들 수 있다.  
모든 Person 클래스의 자식들에게 data 라는 속성을 부여해주고 싶으면 아래와 같이 필드값을 선언해준다.

```
class Person {
  data: number = 0;
}

let john = new Person();
let kim = new Person();

console.log(john.data); // 0
console.log(kim.data); // 0
```

## constructor 타입지정

타입스크립트에서는 `constructor` 안에 `this.속성명` 을 선언해주기 전에 미리 선언 및 타입을 지정해줘야 한다. (필드값으로 만들어줘야 한다.)

```
class Person {
  name;
  age;
  constructor (){
    this.name = 'kim';
    this.age = 20;
  }
}
```

### constructor 의 파라미터 타입지정

`constructor` 에서 받아올 파라미터는 함수와 동일하게 타입을 지정해준다.

```
class Person {
  name;
  age;
  constructor ( a :string ){
    this.name = a;
    this.age = 20;
  }
}
```

### default parameter

파라미터에 값을 입력하지 않으면 할당할 기본값을 지정해 줄 수 있다.

```
class Person {
  name;
  age;
  constructor ( a = 'kim' ) {
    this.name = a;
    this.age = 20;
  }
}
```

이런식으로도 가능하다.

```
class Person {
  name;
  age;
  constructor ( a: string = 'kim' ) {
    this.name = a;
    this.age = 20;
  }
}
```

## prototype

모든 객체는 자신을 만드는데 사용된 원형인 프로토타입 객체를 이용하여 객체를 만든다.  
`객체.prototype` 을 통해 프로토타입 객체에 접근할 수 있다.

```
Person.prototype.gender = 'male';
const 김범서 = new Person();

console.log(김범서); // {name: 'kim', age: 20}
console.log(김범서.gender); // male
```

프로토타입 객체에 추가한 속성은 생성된 객체에서도 접근 할 수 있다.  
생성된 객체의 속성에는 추가되지 않는다.


### 클래스 내부의 prototype 함수를 만드는 방법

```
class Person {
  name;
  age;
  constructor ( a: string = 'kim' ) {
    this.name = a;
    this.age = 20;
  }

	myFunction(txt: string): void {
		console.log(`${txt} :`, this);
	}
}

const a = new Person();
a.myFunction(); // 안녕 : Person { name: 'kim', age: 20 }
```

클래스 내부에 함수명으로 바로 선언해준다.  
타입지정도 일반 함수와 동일하게 해준다.

