# Narrowing 할 수 있는 방법 더 알아보기

## `&&` 연산자를 통한 `null` & `undefined` 체크하는 법

`&&` 연산자를 이용해 파라미터가 `null` 또는 `undefined` 일 경우를 빠르게 체크할 수 있다.

```
function printAll(strs: string | undefined) {
  if (strs && typeof strs === "string") {
    console.log(strs);
  }
}
```

### `&&` 연산자의 다른 기능

`&&` 연산자는 첫 번째 피연산자의 값이 truthy값(참 같은 값) 일 경우 두 번째 피연산자를 반환해준다.  
truthy값은 falsy값을 제외한 모든 값이 된다.

falsy값으로는 다음과 같은 것들이 있다.

`false` , `null` , `undefined` , `NaN` , `""` , `0` , `-0` , `0n`

(`""` 의 경우 빈 문자열로 `string` 타입이지만 falsy값이다.)

#### `&&` 연산자 동작 예시

```
1 && null && 3; // null
undefined && '안녕' && 100; // undefined
```

falsy값이 반환될 경우 연산을 종료한다.

## in 연산자로 object 자료 narrowing

오브젝트가 속성명을 안에 가지고 있는지를 판별해준다.  
`속성명 in 오브젝트` 형식으로 사용한다.

```
type Fish = { swim: string };
type Bird = { fly: string };

const fish = { swim: "물고기" };
const bird = { swim: "못함", fly: "새" };

function myFunction(animal: Fish | Bird) {
  if ("fly" in animal) {
    console.log(animal.fly);
    return animal.fly;
  }
  console.log(animal.swim);
  return animal.swim;
}

myFunction(bird); // 새
```

`animal` 객체에 `fly` 라는 속성명이 있는지를 판별해주고 narrowing으로 인정된다.

## instanceof로 narrowing

class로부터 생산된 object라면 instanceof로 narrowing 이 가능하다.
어떤 클래스로부터 new 키워드로 생산된 object들은 부모 클래스가 누구인지 검사할 수 있다.

```
let date = new Date();
if (date instanceof Date) {
  console.log('참이에요');
}
```
