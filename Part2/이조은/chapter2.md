# Narrowing 할 수 있는 방법 더 알아보기

## 1. && 연산자로 null & undefined 체크하기

변수가 undefined 라면, undefined 가 남아서, if문 조건식이 falsy 값이 남기 때문에, if문이 실행되지 않는다.

따라서 변수가 null, undefined인 경우를 쉽게 거를 수 있는 문법이다.

```
if (변수 && typeof 변수 === "string") {}
```

&& 연산자가 눈에 잘 안들어온다면, != null 을 사용하는 하여 거를 수도 있다.

```
if (변수 != null) {}
```

## 2. in 연산자로 object 자료 narrowing

만약, 파라미터로 객체가 2개 들어올 수 있다고 타입을 지정해놓자.
하나는 {a:'kim'}
나머지는 {b:'park'}

이렇게 서로 다른 속성을 가지고 있을 때 in 연산자로 객체 자료를 narrowing 할 수 있다.

```
type Fish = { swim: string };
type Bird = { fly: string };
function 함수(animal: Fish | Bird) {
  if ("swim" in animal) {
    return animal.swim
  }
  return animal.fly
}
```

## 3. class로부터 생산된 object는 instanceof로 narrowing

class 문법에선 어떤 클래스로부터 new 키워드로 생상된 object 들이 있을 때, instanceof 라는 키워드로 부모 클래스가 누군지 검사할 수 있다.

이걸 가지고 narrowing 을 할 수 있다.

new 키워드로 object 생산할 수 있는게 바로 날짜인데, ㅇ

```
let 날짜 = new Date();
if (날짜 instanceof Date){
  console.log('참이에요')
}
```

## 4. literal type으로 narrowing

literal type 은 string이나 number 로 두가지로 정할 수 있고, 문자열이나 숫자에 정확한 값을 지정하는 것을 말한다.

따라서 Car 와 Bike object 자료가 동일한 속성값을 지니고 있어서, in 문법을 쓸 수 없고, 부모가 없어서 instanceof 도 사용할 수 없을 때 정확한 값이 지정이 되있으나, 서로 다른 값을 지닌 것을 찾으면 된다.

```
type Car = {
  wheel : '4개',
  color : string
}
type Bike = {
  wheel : '2개',
  color : string
}

function 함수(x : Car | Bike){
  if (x.wheel === '4개'){
    console.log('the car is ' + x.color)
  } else {
    console.log('the bike is ' + x.color)
  }
}
```
