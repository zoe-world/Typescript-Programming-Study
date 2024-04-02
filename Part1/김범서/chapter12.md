# Object에 타입지정하려면 interface 이것도 있음

## interface 문법

`interface` 를 통해 `object` 자료형의 타입을 보다 편리하게 지정가능하다.

```
interface SquareType {
  color: string,
  width: number,
}

let square: SquareType = { color: 'red', width: 100 };
```

## `interface` 의 `extends`

`extends` 는 오른쪽의 `interface` 를 복사해온다는 뜻이다.

```
interface Student {
  name: string,
}
interface Teacher {
  name: string,
  age: number,
}
```

위의 두 `interface` 를 `extends` 를 이용해 아래와 같이 바꿀 수 있다.

```
interface Student {
  name: string,
}
interface Teacher extends Student {
  age: number
}
```

## 타입이름 중복선언시

`interface` 의 경우 타입이름의 중복선언을 허용해준다.  
중복시 `extends` 한 것과 동일하게 동작한다.

```
interface Animal {
  name: string
}
interface Animal {
  legs: number
}
```

`type` 은 중복선언을 허용하지 않는다.
