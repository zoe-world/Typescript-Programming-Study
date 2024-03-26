# 타입스크립트 기본 타입 정리 (primitive types)

## 변수 만들 때 타입정하기 (타입 실드씌우기)
타입스크립트는 변수에 타입을 지정해 줄 수 있다.  
변수명 옆에 타입명을 지정해준다.   
```
let 이름: string = 'kim';
```
위와 같이 ```:```를 이용하여 자바스크립트 코드에 타입을 정의하는 방식을 타입 표기(Type Annotation)라고 한다.

이 변수에 string타입이 아닌 다른 타입이 들어오게 되면 에러가 발생한다.

## 기본 타입들 (primitive types)
* String
* Number
* Boolean
* Array
* Object
* Tuple
* any & Unknown
* void
* null
* undefined
* Enum
* never

### String
타입이 문자열인 경우
```
let str: string = 'hi';
```

### Number
타입이 숫자인 경우
```
let num: number = 10;
```

### Boolean
타입이 진위값인 경우
```
let bool: boolean = false
```

### Array
타입이 배열인경우
```
let arr: number[] = [1,2,3];
```
또는 제네릭
```
let arr: Array<number> = [1,2,3];
```

### Object
타입이 객체인 경우
```
let object: { age : number } = { age : 20 };
```

### Tuple
배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열 형식
```
let arr: [string, number] = ['hi', 10];
```

### Any & Unknown
모든 타입을 허용한다.  
any 타입은 타입실드 기능을 해제하여 타입을 바꿔도 에러가 발생하지 않는다.

**any**
```
let name: any = 'kim'; 
```
 
**unknown**
```
let name: unknown = 'kim';
```
```
name = 123;
name = undefined;
name = [];
```

### Void
반환 값이 없는 함수의 반환 타입.  
return이 없거나 return이 있더라도 반환하는 값이 없으면 함수의 반환 타입을 void로 지정한다.
```
function printSomething(): void {
  console.log('바보야!');
}

function returnNothing(): void {
  return;
}
```

## TIP!
타입스크립트라고 해서 꼭 모든 변수에 타입지정을 해줄 필요는 없다.  
VSCode가 선언한 변수에 들어간 데이터에 따라 자동으로 타입지정을 해주기 때문!