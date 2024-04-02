# 타입도 변수에 담아쓰세요 type 키워드 써서 & readonly

## 타입 정의가 너무 길면 Type Aliases (별칭)

코드를 짜다보면,

```
let 동물 :string | number | undefined;
```

매우 길고, 복잡한 타입을 나열하는 경우가 많다.

1. 이게 길고 보기 싫고,
2. 나중에 또 사용하고 싶다면?

변수에 담아서 쓰면 된다.

변수만드는 것 처럼 type 이라는 키워드를 쓰면 된다.
type 키워드 쓰는걸 type alias 라고 한다.
alias를 번역하자면 별칭인데 저는 그냥 쉽게 변수라고 부른다.

```
type Animal = string | number | undefined;
let 동물 :Animal;
```

type 타입변수명 = 타입종류
타입을 변수처럼 만들어서 쓰는 alias 문법인데, 관습적으로 대문자로 시작한다.
일반 자바스크립트 변수랑 차별을 두기 위해 AnimalType 이런 식으로 작명하는 것이 좋다.

## object 타입도 저장가능합니다

```
type 사람 = {
  name : string,
  age : number,
}

let teacher :사람 = { name : 'john', age : 20 }
```

object에 타입지정할 때 자주 활용할 수 있다.

type 키워드 안쓴다면?

```
let teacher :{
  name : string,
  age : number,
} = { name : 'john', age : 20 }
```

위와 같이 이해하기가 어려운 코드는 보기 좋은 코드가 아니다.

## readonly로 잠그기

```
const 출생지역 = 'seoul';
출생지역 = 'busan'; //const 변수는 여기서 에러남
```

const 변수는 값이 변하지 않는 변수를 만들고 싶을 때 const 쓰면 된다.
재할당시 에러가 나기 때문에 값이 변하는걸 미리 감지하고 차단할 수 있기 떄문!

```
const 여친 = {
  name : '엠버'
}
여친.name = '유라';  //const 변수지만 에러안남
```

하지만 object 자료를 const에 집어넣어도 object 내부는 마음대로 변경가능하다.

const 변수는 재할당만 막아줄 뿐이지 그 안에 있는 object 속성 바꾸는 것까지 관여하지 않기 때문이다.

object 속성을 바뀌지 않게 막고 싶으면 타입스크립트 문법을 쓰면 된다!

```
type Girlfriend = {
  readonly name : string,
}

let 여친 :Girlfriend = {
  name : '엠버'
}

여친.name = '유라' //readonly라서 에러남
```

한번 부여된 후엔 앞으로 바뀌면 안될 속성들을 readonly로 잠굴 수 있다.

(물론 readonly는 컴파일시 에러를 내는 것일 뿐 변환된 js 파일 보시면 잘 바뀌긴 한다)

## 속성 몇개가 선택사항이라면

그니까 어떤 object자료는 color, width 속성이 둘다 필요하지만
어떤 object 자료는 color 속성이 선택사항이라면
type alias를 여러개 만들어야하는게 아니라 **물음표연산자만** 추가하면 됩니다.

```
type Square = {
  color? : string,
  width : number,
}

let 네모2 :Square = {
  width : 100
}
```

Square라는 type alias를 적용한 object 자료를 하나 만들었는데, color 속성이 없어도 에러가 나지 않습니다.

실은 물음표는 "undefined 라는 타입도 가질 수 있다~"라는 뜻임을 잘 기억해두자!

진짠지 확인하고싶으면 마우스 올려보면 알 수 있다.

## type 키워드 여러개를 합칠 수 있습니다.

```
type Name = string;
type Age = number;
type NewOne = Name | Age;
```

OR 연산자를 이용해서 Union type을 만들 수도 있다.

위 코드에서 NewOne 타입에 마우스 올려보시면 string | number라고 나온다.

```
type PositionX = { x: number };
type PositionY = { y: number };
type XandY = PositionX & PositionY
let 좌표 :XandY = { x : 1, y : 2 }
```

object에 지정한 타입의 경우 합치기도 가능하다.
& 기호를 쓴다면 object 안의 두개의 속성을 합쳐준다.
위 코드에서 XandY 타입은 { x : number, y : number } 이렇게 정의되었다.
합치기는 초딩용어고 멋진 개발자말로 **extend** 한다라고 합니다.

물론 Type alias & Type alias 만 가능한게 아니라
Type alias & { name : string } 이런 것도 가능하다.

## type 키워드는 재정의가 불가능합니다.

type Name = string;
type Name = number;

이러면 ㅌㅌ에러가 난다.

나중에 type 키워드랑 매우 유사한 interface 키워드를 배우게 되는데,
이 키워드를 쓰면 재정의가 가능하다. 재정의하면 & 하는거랑 똑같은 기능을 하는데
하지만 재정의 불가능한 편이 더 안전하지 않을까..?
