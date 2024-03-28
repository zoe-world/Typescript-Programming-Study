# 타입도 변수에 담아쓰세요 type 키워드 써서 & readonly

## type alias (타입변수)

타입 정의가 너무 길고 가독성이 떨어지거나 여러번 재사용하고 싶으면 `type` 키워드를 사용 할 수 있다.  
타입명은 관습적으로 대문자로 시작한다.

```
type AnimalType = string | number | undefined;
let animal :AnimalType;
```

또한 `object` 타입도 저장가능하다.

```
type PersonType = {
  name : string,
  age : number,
};

let teacher: PersonType = { name : 'john', age : 20 };
```

## object readonly 속성

`readonly` 키워드는 속성 왼쪽에 붙일 수 있으며 특정 속성을 변경 불가능하게 잠궈준다.

```
type GirlfriendType = {
  readonly name : string,
};

let girlfriend :GirlfriendType = {
  name : '엠버'
}

girlfriend.name = '유라'; // 읽기 전용 속성이므로 'name'에 할당할 수 없습니다.
```

### TS 에러 주의사항

타입스크립트 에러는 에디터, 터미널에서만 존재하기 때문에 컴파일된 `.js` 파일에서는 에러가 발생하지 않고 동작하게 된다.

## type alias extend

OR 연산자를 이용해서 Union type을 만들 수 있다.  
`NewOne` 의 타입은 `string | number` 가 된다.

```
type Name = string;
type Age = number;

type NewOne = Name | Age;
```

object에 지정한 타입의 경우 AND 연산자를 이용해 object 안의 두개의 속성을 합칠 수 있다.  
`XandY` 타입은 `{ x : number, y : number }` 가 된다.

```
type PositionX = { x: number };
type PositionY = { y: number };

type XandY = PositionX & PositionY;

let position: XandY = { x : 1, y : 2 };
```

### `type` 키워드는 재정의가 불가능하다.

```
type Name = string;
type Name = number; // 'Name' 식별자가 중복되었습니다.
```

### object 타입을 정의한 type alias 두개를 & 기호로 합칠 때 중복된 속성이 있을 경우

중복된 속성의 타입이 같을 경우 문제없이 동작한다.

```
type PositionX = { x: number };
type PositionXY = { x: number; y: number };

type Position = PositionX & PositionXY;

const position: Position = {x: 1, y: 2};
```

중복된 속성의 타입이 다를 경우 에러가 발생한다.

```
type PositionX = { x: number };
type PositionXY = { x: string; y: number };

type Position = PositionX & PositionXY;

const position: Position = {x: 1, y: 2}; // 'number' 형식은 'never' 형식에 할당할 수 없습니다.
```

아래의 `Never` 타입의 특징으로 보아 절대 발생할 수 없는 타입이기 때문에 `Never` 타입으로 변환되는 것으로 보인다.

### Never 타입

- 절대 발생할 수 없는 타입을 나타낸다.
- 함수 표현식이나 화살표 함수 표현식에서 항상 오류를 발생시키거나 절대 반환하지 않는 반환 타입으로 쓰인다.
- 모든 타입에 할당 가능한 하위 타입이다.  
  (`never` 타입엔 다른 모든 타입이 할당될 수 없다.)
- 함수의 마지막에 도달할 수 없다.

```
function foo2(x: string | number): boolean {
  if (typeof x === "string") {
    return true;
  } else if (typeof x === "number") {
    return false;
  }
  return fail("Unexhaustive!");
}
function fail(message: string): never {
  throw new Error(message);
}

let tmp: any = { id: "123" };
console.log(foo2(tmp));
```

`never` 타입은 모든 타입에 할당 가능한 하위 타입이다.  
따라서 위 코드에서 `foo2` 함수는 `boolean` 타입을 반환해야 하지만 `never` 타입을 반환하는 `fail` 함수의 리턴값을 반환해줄 수 있다.
