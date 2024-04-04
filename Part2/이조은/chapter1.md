# 함수 rest 파라미터, destructuring 할 때 타입지정

## rest 파라미터란?

: 매개변수 이름 앞에 ...을 붙여서 정의한 매개변수

- rest parameter는 이름 그대로 먼저 선언된 매개변수에 할당된 전달인자를 제외한 나머지 전달인자들이 모두 배열에 담겨 할당된다.
- 따라서 rest parameter는 반드시 마지막 매개변수여야 한다.

## rest 파라미터 특징

1. rest parameter는 함수 객체의 length 프로퍼티에 영향을 주지 않는다.

```
function func(...rest) {}
func.length; // 0

function func1(arg1, arg2, ...rest) {}
func1.length; // 2

```

2. 전달인자의 수가 정해져 있지 않은 경우에 사용할 수 있다.

```
function 전부더하기(...a){
  console.log(a)
}

전부더하기(1,2,3,4,5)
```

## 그렇다면 rest 파라미터 타입지정은?

rest 파라미터는 항상 [] 안에 담겨오기 때문에, 타입지정도 array 처럼 해주면 된다.

```
function 전부더하기(...a :number[]){
  console.log(a)
}

전부더하기(1,2,3,4,5)
```

## spread operator란?

Spread 문법(Spread Syntax, ...)는 대상을 개별 요소로 분리한다. Spread 문법의 대상은 이터러블이어야 한다.

> 쉽게 얘기하면, 괄호를 벗긴다고 생각하면 편하다.

```
let arr = [3,4,5];
let arr2 = [1,2, ...arr]
console.log(arr2) // [1,2,3,4,5]
```

## Destructuring 이란

배열이나 객체의 속성을 해체하여 그 값을 변수에 담을 수 있게 하는 ES6 문법이다.

## 1. Array Destructuring

### 중첩되지 않은 배열의 경우

**기존 방법**  
중첩되지 않은 배열은 인덱스를 통한 접근이 가능하다.

```
let arr = [10, 20, 30]
let arr1 = arr[0];
let arr2 = arr[1];
let arr3 = arr[2];
```

**ES6**

```
let arr = [10, 20, 30];
let [arr1, arr2, arr3] = arr;
console.log(`arr1: ${arr1}, arr2: ${arr2}, arr3: ${arr3}`);

// 출력값 : arr1: 10, arr2: 20, arr3: 30
```

```
let kors = [10, 20, 30];

let [kor1, kor2, kor3] = kors;

console.log(`kor1: ${kor1}, kor2: ${kor2}, kor3: ${kor3}`);
// 출력값 : kor1: 10, kor2: 20, kor3: 30

[kor1] = [1, 3, 5];
[, kor2] = [1, 3, 5];
[, , kor3] = [1, 3, 5];

console.log(`kor1: ${kor1}, kor2: ${kor2}, kor3: ${kor3}`);
// 출력값 : kor1: 1, kor2: 3, kor3: 5

```

### 중첩된 배열의 경우

```
let kors = [10, 20, 30, [40, 50]];

let [kor1, kor2, kor3, [kor4, kor5]];

console.log(`${kor1}, ${kor2}, ${kor3}, ${kor4}, ${kor5}`);
//출력값 10, 20 , 30, 40, 50
```

## 2. Object Destructuring

### 중첩되지 않은 객체인 경우

```
let exam = {
    kor: 30,
    eng: 40,
    math: 50
};

//기존 문법
let kor = exam.kor;
let eng = exam.eng;
let math = exam.math;

// ES6 Destructuring 문법
let {kor, eng, math} = exam;

console.log(`kor: ${kor}, eng: ${eng}, math: ${math}`);
// 출력값 : kor1: 30, kor2: 40, kor3: 50
```

### 중첩된 객체인 경우

```
let exam = {
    kor: 30,
    eng: 40,
    math: 50,
    student: {
        name: "newlec",
        phone: "010-1111-2222"
    }
};

// 기존 문법
let name = exam.student.name;
let phone = exam.student.phone;

// ES6 Destructuring 문법
let {student:{name}, student:{phone}} = exam;

console.log(`name: ${name}, phone: ${phone}`);
// 출력값 : student: newlec, phone: 010-1111-2222
```

## Destructuring 문법 함수 파라미터에 사용가능

- 함수 파라미터 작명하는 것도 변수만드는 문법과 똑같아서 그렇다.  
   변수만들 때 기존 object에 있던 자료를 파라미터로 집어넣고 싶으면  
   기존 object에 있던 걸 person.student, person.age 이렇게 넣으면 되긴 하는데,
  destructuring 문법을 이용하면 더 쉽게 가능하다.

**기존 방법**

```
let person = { student : true, age : 20 }

function 함수(a, b){
  console.log(a, b)
}
함수(person.student, person.age)

```

**destructuring 문법**

```
let person = { student : true, age : 20 }

function 함수({student, age} :{student : boolean, age : number}){
  console.log(student, age)
}
함수({ student : true, age : 20 })
```
 