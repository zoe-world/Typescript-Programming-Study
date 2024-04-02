# 함수와 methods에 type alias 지정하는 법

## 함수의 타입지정도 가능함

1. 함수타입은 화살표 함수 ()=>{} 모양으로
2. type alias를 사용하려면, 함수 표현식에만 사용이 가능하다.

```
    type 함수타입 = (a:string) => number;

    let 함수 : 함수타입 = function (a){
        return 10
    }
    함수();
```

## methods 안에 타입지정하기

object 자료 안에 함수를 넣을 수 있다.

```
let 회원정보 = {
    name : 'kim',
    age : 30,
    plusOne (x){
        return x+1
    },
    changeName : () => {
        console.log('hello')
    }
}
회원정보.plusOne(1);
회원정보.changeName();
```

plusOne는 일반 함수,
changeName 은 화살표함수로
둘 다 object 안에 맘대로 집어넣을 수 있다.
