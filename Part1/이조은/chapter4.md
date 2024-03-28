# 타입 확정하기 Narrowing & Assertion

아래는 숫자 또는 문자를 집어넣으면 +1 해주는 함수이다.

```
function 내함수(x :number | string){
   return x + 1  //에러남
}

```

근데 에러가 난다.
**Operator '+' cannot be applied to types 'string | number' and 'number'**

string | number 같은 union type에는 일반적으로 조작을 못하게 막아놔서 그렇다.
이런 메세지를 보면 1. 타입을 하나로 Narrowing 해주거나 2. Assert 해주거나 둘 중 하나 해주면 된다.

## Type Narrowing

if문 등으로 타입을 하나로 정해주는 것을 뜻합니다. 그래서 아까 함수를 사용할 때

```
function 내함수(x :number | string){
  if (typeof x === 'number') {
    return x + 1
  }
  else if (typeof x === 'string') {
    return x + 1
  }
  else {
    return 0
  }
}
```

if문과 typeof 키워드로 현재 파라미터의 타입을 검사해서
이게 'number' 타입일 경우 이렇게 해주세요.
이게 'string' 타입일 경우 이렇게 해주세요.
이렇게 코드를 짜야 정상적으로 사용이 가능하다.

타입스크립트는 타입이 애매한 걸 싫어해서 귀찮아도 이렇게 해야한다!
타입이 확실치 않을 때 생기는 부작용을 막기위한 장치라고 보면 된다.
가끔 이걸 'defensive'하게 코딩한다' 라고 하기도 한다.

근데 또 함수안에서 if문 쓸 때는 마지막에 else{} 이게 없으면 에러가 난다.
return 하지 않는 조건문이 있다면 나중에 버그가 생길 수 있어서 에러를 내주는 건데,
**"noImplicitReturns": false, **

이게 성가시다면 tsconfig.js 파일에서 이걸 추가하면 된다. 근데 굳이 수정하는 것 보다는 엄격하게 쓰는게 낫다.

- 꼭 typeof를 쓸 필요는 없고 타입을 하나로 확정지을 수 있는 코드라면 어떤 것도 Narrowing 역할을 할 수 있습니다.

- in, instanceof 키워드도 사용가능합니다.

## Type Assertion

아니면 타입을 간편하게 asset 할 수도 있다.
'이 변수의 타입을 number 로 생각해주세요.' 이런 뜻으로 코드를 짜면
타입스크립트 컴파일러가 눈감아준다.

**변수명 as string**
이런 식으로 as 라는 키워드를 쓰면 된다.

```
function 내함수(x :number | string){
    return (x as number) + 1
}
console.log( 내함수(123) )
```

변수명 as number 라고 쓰면
'나는 이 변수를 number 라고 주장하겠습니다~' 라는 뜻이며 실제로 그렇게 타입을 변경해준다.
근데 그러려면 내가 '함수에 무조건 숫자가 들어올 것이다' 라는 사실을 알고 있어야 안전하게 쓸 수 있는 문법이라는 뜻이다.

as 키워드 사용시 특징이 있는데,

1. as 키워드는 union type( x: number | string) 같은 복잡한 타입을 하나의 정확한 타입으로 줄이는 역할을 수행한다.

```
 function 내함수(x :number){
    return (x as string) + 1
}
```

(number 타입을 as string 이렇게 사용하면 에러가 난다)

2. 실은 그냥 타입실드 임시 해제용이다. 실제 코드 실행경과는 as 있을 때나 없을 때나 거의 동일하다.

Q. 근데 내함수('123') 이렇게 숫자말고 문자를 입력하면 어떻게 될까?
A. as number 라고 썼긴 했지만, number 타입처럼 +1 를 해주진 않는다. '1231' 이렇게 출력이 된다.
as 는 그냥 주장만 할 뿐 실제로 타입을 바꿔주진 않는다.

as를 쓰면 간편해지지만, 정확히 코드를 짜기 위해선 narrowing을 써야한다.

as 문법은 이럴 때 쓰자.

1. 타입에러가 나는지 정말 모르겠는 상황에 임시로 에러해결용으로 사용
2. 내가 어떤 타입이 들어올지 정말 확실하게 알고 있는데, 컴파일러 에러가 방해할 때

### 옛날 assertion 문법은

as 키워드 대신 <> 이런 괄호를 사용했다.

```
let 이름 :number = 123;

(이름 as string) + 1;  //현재문법
<string>이름 + 1;   //옛날문법
```

위에 두 줄 코드는 똑같은 의미이다.
다만, html 과 js 가 혼연일체가 된 리액트에서 사용시 html 태그 이런거랑
헷갈릴 수 있기 떄문에 지금은 그냥 as 키워드르 주로 쓴다.

### 혹은 as는 이럴 때 유용하게 쓰기도 합니다

가끔 타입을 강제로 부여하는 기계를 하나 만들어쓰고 싶을 때가 있다.
그럴 때 함수에 데이터를 넣으먄 as 타입명을 붙여서 return 하는 함수를 만들어 사용하면 된다.

```
type Person = {
    name : string
}
function 변환기<T>(data: string): T {
    return JSON.parse(data) as T;
}
const jake = 변환기<Person>('{"name":"kim"}');
```

변환기 하는 함수를 만들었는데,
이 함수에 자료를 입력하면 as 키워드로 타입을 하나 붙여준다
<타입을 파라미터로 넣는 방법> 그리고 type 키워드 이런게 두드등장!
하지만 아직 배우지 않은 문법이 등장하니, 지금은 그렇구나 정도 알면 된다!
