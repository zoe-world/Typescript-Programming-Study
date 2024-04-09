# 타입을 파라미터로 입력하는 Generic

## 함수 return 값의 타입

- 아무렇게나 생긴 array 자료를 입력하면, array의 첫 자료를 출력해주는 함수를 만들었다.

```
function 함수(x: unknown[]) {
  return x[0];
}

let a = 함수([4,2])
console.log(a)// 4
```

위에 a 는 숫자 타입이 아니고, unknown 타입이다.
입력된 array 가 unknown 타입이기 때문이다.

<span style="color:red">타입스크립트는 타입을 알아서 변경해주지 않는다.</span>

따라서 아래와 같이 연산도 에러가 난다.

```
function 함수(x: unknown[]) {
  return x[0];
}

let a = 함수([4,2])
console.log(a + 1)

```

따라서, a는 사람이 보기엔 숫자가 맞지만 아직 타입은 unknown 타입이기 떄문에, 함수의 return 타입지정을 :number 이걸로 강제로 바꾸기 전까지는 number 타입으로 변하지 않는다.

> 함수에 불확실한 unknown,any,union 타입을 입력하면 나오는 값도 unknown,any,union 타입이기 때문에, 일어나는 문제가 많다.  
> 해결책은
>
> 1.  narrowing 하기(귀찮음)
> 2.  타입을 파라미터로 함수에 미리 입력하는 방법(원하는 곳에 가변적으로 타입지정 가능) 2번을 **Generic** 이라고 부름.

## Generic 적용한 함수만들기

- 함수에 <> 이런 괄호를 열면 파라미터를 또 입력할 수 있습니다.  
  근데 여기 안엔 타입만 입력할 수 있습니다. 타입파라미터 문법이다.

```
function 함수<MyType>(x: MyType[]) :MyType {
  return x[0];
}

let a = 함수<number>([4,2])
let b = 함수<string>(['kim', 'park'])

```

함수를 사용할 때도 <> 안에 파라미터처럼 타입을 입력할 수 있다. 변수에 할당할 때, 함수<타입형태>() 이렇게 선언하는 순간 MyType 이라는 변수에 지정한 타입형태가 들어간다고 보면 된다.

```
function 함수<MyType>(x: MyType[]) :MyType {
  return x[0];
}

let a = 함수([4,2])
let b = 함수(['kim', 'park'])

```

위와 같이 <> 사용을 하지 않아도, 기본 타입을 유추해서 알아서 집어넣어준다.

(참고)

- 타입파라미터는 자유작명가능 보통 <T> 이런걸로 많이 합니다.
- 일반 함수파라미터 처럼 2개 이상 넣기도 가능합니다

## Generic 타입 제한하기 (constraints)

extends 문법으로 넣을 수 있는 타입을 제한할 수 있다.
ex) MyType extends number
라고 쓰면 타입 파라미터에 넣을 수 있는 타입을 제한가능하다.

interface 문법에서 쓰는 extends 는 복사라면, Generic extends는 if문으로 체크하는 문법이라고 보면 된다.

```
function 함수<MyType extends number>(x: MyType) {
  return x - 1
}

let a = 함수<number>(100) //잘됩니다
```

## 커스텀 타입도 extends 가능

> 문자로 파라미터를 넣으면 자릿수를 세어서 출력해주는 함수를 Generic으로 만들어보자.

```
function 함수<MyType>(x: MyType) {
  return x.length
}

let a = 함수<string>('hello') // 에러
```

에러가 나는 이유는 MyType에 string 을 집어넣었지만, 나중에 number를 실수로 넣게될 수도 있기 때문에, .length 같은 조작은 일단 방지해준다.
따라서 MyType을 extends 이런걸로 정확하게 제한을 해줘야하는데, 이번에는 interface 로 만든 타입을 extends 해보자!

```
interface lengthCheck {
  length : number
}
function 함수<MyType extends lengthCheck>(x: MyType) {
  return x.length
}

let a = 함수<string>('hello')  //가능
let a = 함수<number>(1234) //에러남
```

- length 속성을 가지고 있는 타입 하나를 만들고, 이름은 lengthCheck 로 함.
- 이걸 extends하면, MyType도 length 속성을 복사해서 가지게 된다. 그러면 MyType은 length가 분명히 있기 때문에, 부여받은 x.length 조작이 가능하게 된다.

(숙제1) 문자를 집어넣으면 문자의 갯수, array를 집어넣으면 array안의 자료 갯수를 콘솔창에 출력해주는 함수는 어떻게 만들까요?

