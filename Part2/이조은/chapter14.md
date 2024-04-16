# object index signature

object 자료에 타입을 미리 만들어주고 싶은데,

1. object 자료에 어떤 속성들이 들어올 수 있는지 아직 모르는 경우
2. 타입지정할 속성이 너무 많은 경우
   index signatures 를 사용한다.

## index signatures 란?

인덱스 시그니처는 {[key:T]:U} 형식으로 객체가 여러 Key를 가질 수 있으며 Key와 매핑이 되는 Value를 가지는 경우 사용한다.

```
interface StringOnly {
  [key: string]: string
}

let obj :StringOnly = {
  name : 'kim',
  age : '20',
  location : 'seoul'
}
```

StringOnly라는 interface가 하나 있다.<span style="background-color:#f9baba;color:black">[key: T]: U</span> 로 타입을 적게 되면, key값이 string이면 할당되는 value 값은 string 이어야하는 타입이라는 뜻이다.
{모든 속성: string} 이라는 뜻과 동일하다고 생각하면 편하다.

```
interface StringOnly {
  age : number,   ///에러
  [key: string]: string,
}

interface StringOnly {
  age : number,   ///가능
  [key: string]: string | number,
}
```

[key: T]: U 이 문법은 다른 속성과 함께 사용할 순 있지만,
{모든 속성:string, age:number} 라는 건 논리적으로 말이 되지 않아 선언이 불가능하다. 따라서, {모든 속성: string | number, age: number} 이런 식으로 해주면 논리적으로 말이 된다.

## array 형태도 가능

```
interface StringOnly {
  [key: number]: string,
}

let obj :StringOnly = {
  0 : 'kim'
  1 : '20',
  2 : 'seoul'
}
```

[] 여기 안에 key값의 타입으로 number 로 지정할 수 있다.
object의 키값이 숫자로 들어오는 경우, value 로 string을 가져야 한다는 타입이다.
따라서, array처럼 쓰고 싶은 Object가 있으면 저렇게 타입지정도 가능하다는 소리이다.

💡 Key의 타입은 string, number, symbol, Template literal 타입만 가능하다.

## Recursive Index Signatures

- object안에 object안에 object

```
let obj = {
  'font-size' : {
    'font-size' : {
      'font-size' : 14
    }
  }
}
```

위의 중첩된 object들을 한 번에 타입 지정하는 방법은?

```
interface MyType {
  'font-size': MyType | number
}


let obj :MyType = {
  'font-size' : {
    'font-size' : {
      'font-size' : 14
    }
  }
}
```

'font-size' 속성은 MyType이랑 똑같이 생긴 타입을 만들면, 타입을 중첩해서 안 써도 된다.

(숙제1) 다음 자료의 타입을 지정해보시오.

```
let obj = {
  model : 'k5',
  brand : 'kia',
  price : 6000,
  year : 2030,
  date : '6월',
  percent : '5%',
  dealer : '김차장',
}
```

**정답**

```
interface CarType {
  [key: string]: string | number;
}
let obj: CarType = {
  model: "k5",
  brand: "kia",
  price: 6000,
  year: 2030,
  date: "6월",
  percent: "5%",
  dealer: "김차장",
};
```

(숙제2) 다음 object 자료의 타입을 interface 써서 만들어보십시오.

```
let obj = {
  'font-size' : 10,
  'secondary' : {
    'font-size' : 12,
    'third' : {
      'font-size' : 14
    }
  }
}
```

