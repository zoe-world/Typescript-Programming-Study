# 타입을 미리 정하기 애매할 때 (union type, any, unknown)

## Union type

타입을 두개 이상 합친 새로운 타입을 만든다.
변수의 타입이 `string` 또는 `number` 라면 `|` 연산자 (or연산자)를 통해 `Union type` 을 지정할 수 있다.

```
let name: string | number = 'kim';
let age: (string | number) = 100;
```

변수에 `string` 데이터를 할당해줄 경우 해당 변수의 타입은 더이상 `string | number` 가 아닌 `string` 타입으로 확정된다.  
이 상태에서 다시 `number` 데이터를 할당해줄 경우 다시 `number` 타입으로 변환된다.

### 숫자 OR 문자가 가능한 array, object 타입 지정

```
const array: (number | string)[] = [1,'2',3];
let object: {data : (number | string) } = { data : '123' };
object = { data : 123 };
```

아래와 같이 `()` 가 없이 타입 지정을 한 경우에는 위와 달리 `number` 또는 `string[]` 타입을 할당 가능한 변수가 된다.

```
let array: number | string[] = 123;
array = ['1','2','3'];
```

### Union type에 + 연산이 안되는 이유

```
let age: string | number;
age + 1;
```

JS에서는 `string` 타입과 `number` 타입 모두 `+` 연산이 가능하다.  
`string | number` 는 `string` 타입과도 다르고 `number` 타입과도 다른 `string | number` 이라는 새로운 유니온 타입이기에 에러가 발생한다.

추가로 각 타입에 대한 연산 결과는 아래와 같다.

```
let age: string = '1';
console.log(age + 1); // 11
```

```
let age: number = 1;
console.log(age + 1); // 2
```

## Any

어떤 타입이든지 할당 할 수 있는 타입이다.  
또한 타입 실드기능을 해제한다.

```
let name: any = 'kim';
name = 123;
name = undefined;
name = [];
```

## Unknown

`any` 와 똑같이 모든 타입을 할당 할 수 있다.

```
let 이름: unknown = 'kim';
이름 = 123;
이름 = undefined;
이름 = [];
```

## Any 와 Unknown 타입의 차이

### 다른 변수 또는 연산에 들어갈 경우

```
let unknownVar: unknown;

let var1: string = unknownVar;  // 'unknown' 형식은 'string' 형식에 할당할 수 없습니다.
let var2: boolean = unknownVar; // 'unknown' 형식은 'boolean' 형식에 할당할 수 없습니다.
let var3: number = unknownVar;  //  'unknown' 형식은 'number' 형식에 할당할 수 없습니다.

unknownVar.data; // 'unknownVar'은(는) 'unknown' 형식입니다.

let age: unknown = 1;
age - 1;  // 'age'은(는) 'unknown' 형식입니다.
```

`Unknown` 의 경우 에러가 발생하지만 `any` 의 경우 발생하지 않는다.  
`any` 타입의 변수와 만난 변수도 타입 실드 기능이 해제된다.

`.name`과 같은 호출은 `object` 류의 타입만 할 수 있다.

```
let age: unknown = 1;
age - 1;  // 'age'은(는) 'unknown' 형식입니다.
```

타입스크립트에서 뺄셈은 `number` 류의 타입만 할 수 있다.  
`age`에 할당된 데이터는 `number` 타입이지만 `age` 는 `unknown` 타입이므로 에러가 발생한다.

**타입스크립트는 언제나 확실한걸 좋아한다!**
