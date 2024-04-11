# array 자료에 붙일 수 있는 tuple type

## Tuple 타입

- tuble type은 array에 붙일 수 있는 타입이며, 자료의 위치까지 정확히 지정할 수 있는 타입이다.

```
let 멍멍이 :[string, boolean];
멍멍이 = ['dog', true]
```

[] 괄호 안에 타입을 적으면, tuple type이 되는데, [] 안에 차례로 세부 타입을 기입하면 된다.
위에 예시 경우, 첫번째 자료는 string, 두번째 자료는 boolean만 허용가능하고, 다른 게 들어올 경우 에러로 잡아준다.

## Tuple 응용 : rest parameter

```
function 함수(...x :[string, number] ){
  console.log(x)
}
함수('kim', 123)  //가능
함수('kim', 123, 456)  //에러
함수('kim', 'park')  //에러
```

- 함수 정의할 때, 파라미터 왼쪽에 점 3개를 붙이면 rest parameter라고 한다.
- rest parameter는 몇개의 파라미터가 들어올 지 모르는 경우, 사용하는 파라미터이다.
- ... 으로 입력된 파라미터들은 array에 담겨오기 때문에, array처럼 타입지정을 해줘야하는데, tuple을 이용해서 타입지정이 가능하다.
- 일반 파라미터 2개 쓰는 거와 기능상 다를 바는 없지만, 차이는 rest parameter를 쓰면, 전부 array에 담겨서 온다.

## tuple 안에도 옵션가능

type Num = [number, number?, number?];
let 변수1: Num = [10];
let 변수2: Num = [10, 20];
let 변수3: Num = [10, 20, 10];

tuple 안에도 물음표를 넣어서 표현이 가능하다.

```
type Num = [number, number?, number];
```

그러나 array 중간에 있는 자료를 옵션 설정할 순 없다.
옵션기호는 뒤에만 붙일 수 있다.

## array 두개를 spread 연산자로 합치는 경우 타입지정은?

```
let arr = [1,2,3]
let arr2 :[number, number, ...number[]] = [4,5, ...arr]
```

- 점 3개 spread 연산자를 사용하면 array의 괄호를 벗겨주는데, 위에 arr2처럼 사용이 가능하다.
- tuple 타입에 점을 3개 붙이면 몇개의 자료가 들어올 지는 모르나, 숫자형으로 들어올 예정이라고 tuple 표현이 가능하다.

(숙제1) 여러분이 최근에 사먹은 음식의 1. 이름 2. 가격 3. 맛있는지여부를 array 자료에 담아보고 타입지정까지 해보십시오.

```
type Food = [string, number, boolean];
let food: Food = ["샌드위치", 7900, false];
```

(숙제2) 이렇게 생긴 자료는 타입지정 어떻게 해야할까요?

```
type Foods = [string, number, ...boolean[]];
let arr: Foods = ["동서녹차", 4000, true, false, true, true, false, true];
```

(숙제3) 함수에 타입지정을 해보도록 합시다.

1. 이 함수의 첫째 파라미터는 문자,

2. 둘째 파라미터는 boolean,

3. 셋째 파라미터부터는 숫자 또는 문자가 들어와야합니다.

```
function 함수(...x: [string, number, ...(string | number)[]]) {
  console.log(x);
}
```

(숙제4) 다음과 같은 문자/숫자 분류기 함수를 만들어보십시오.

```
function filtering(...x: (string | number)[]) {
  let text: string[] = [];
  let num: number[] = [];
  let total = [text, num];

  x.forEach((v) => {
    if (typeof v === "string") {
      text.push(v);
    } else {
      num.push(v);
    }
  });
  return total;
}
filtering("b", 5, 6, 8, "a");

```
