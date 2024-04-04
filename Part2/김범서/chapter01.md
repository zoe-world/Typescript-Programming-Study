# 함수 rest 파라미터, destructuring 할 때 타입지정

## rest parameter

함수에 어떤 파라미터가 몇개 들어올지 미리 정의가 불가능한 경우 사용한다.

```
function myFunction(...a) {
  console.log(a);
}

myFunction(1,2,3,4,5);
```

- 파라미터 앞에 `...` 을 붙여 사용한다.
- rest 파라미터는 일반 파라미터의 맨 마지막에 선언해줘야 한다.
- rest 파라미터자리에 집어넣은 값들은 전부 [ ] 안에 담겨있다. (배열이 된다.)

### rest parameter의 타입지정

rest parameter에 입력된 값들은 배열에 저장되기 때문에 배열타입으로 선언해준다.

```
function myFunction(...a: number[]) {
  console.log(a);
}

myFunction(1,2,3,4,5);
```

## destructuring (구조 분해 할당)

배열이나 객체 안에 있는 데이터를 빼서 변수로 만들고 싶을 때 쓰는 문법이다.

```
let { student, age } = { student : true, age : 20 };
let [a, b] = ['안녕', 100];

console.log(student); // student : true
console.log(b); // 100
```

### 배열의 구조 분해 할당 규칙

배열의 각 요소는 순서대로 할당된다.  
따라서 배열 구조 분해 할당에서는 할당되는 변수의 위치가 배열의 순서와 일치해야 한다.

#### 배열에서 특정값만 필요한 경우

배열 구조 분해 할당에서 필요하지 않은 값을 무시하려면 해당 값을 받을 변수 이름을 생략하면 된다.  
필요하지 않은 값을 받을 변수를 생략하면 해당 값은 무시되고, 할당하려는 변수만 선언하여 값을 받을 수 있다.

```
const array = [1, 2, 3];
const [a, b] = array;

console.log(a); // 1
console.log(b); // 2
```

두번째값 없이 첫번째와 세번째 값만 필요로 하는 경우 해당 위치의 변수를 비워주는 식으로 할당 할 수 있다.

```
const array = [1, 2, 3];
const [a, , c] = array;

console.log(a); // 1
console.log(c); // 3
```

### 객체의 구조 분해 할당 규칙

객체의 속성은 이름에 따라 할당된다.  
따라서 객체 구조 분해 할당에서는 할당되는 변수의 이름이 객체의 속성 이름과 일치해야 한다.

```
const person = { name: 'John', age: 30 };
```

#### 올바른 구조 분해 할당

name에 'John'이 할당되고, age에 30이 할당된다.

```
const { name, age } = person;
```

#### 잘못된 구조 분해 할당

n과 a에 undefined가 할당된다.

```
const { n, a } = person;
```

## 함수 파라미터에서의 Destructuring 문법 활용

```

let person = { student : true, age : 20 }

function myFunction(a, b) {
console.log(a, b);
}
myFunction(person.student, person.age);

```

객체 안의 속성을 각각 파라미터로 입력해주는 대신 Destructuring 문법을 통해 아래와 같이 입력 할 수 있다.

```

let person = { student : true, age : 20 }

function myFunction( {student, age} ) {
console.log(student, age);
}
myFunction({ student : true, age : 20 });

```

### 타입 지정

object 와 동일한 형태이기 때문에 동일한 방식으로 타입을 지정해 줄 수 있다.

```
interface PersonType {
  student: boolean;
  age: number;
}

const person: PersonType = { student: true, age: 20 };

function myFunction({ student, age }: PersonType) {
  console.log(student, age);
}

myFunction(person);
```
