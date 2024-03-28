# Literal Types로 만드는 const 변수 유사품

## literal types

특정 글자나 숫자만 가질 수 있게 제한을 두는 타입을 literal type 이라고 한다.

```
let 방향: 'left' | 'right';
방향 = 'left';
```

## literal type 문제점과 as const 문법

```
var data = {
  name : 'kim'
};

function myFunction(a: 'kim') {
	...
}
myFunction(data.name); // 'string' 형식의 인수는 '"kim"' 형식의 매개 변수에 할당될 수 없습니다.
```

`data.name` 은 `'kim'` 을 리턴해주지만 위와 같은 에러가 발생한다.  
`data.name` 은 `string` 타입이고 파라미터 `a` 는 `'kim'` 이라는 자료형을 허용하는 것이 아닌 `'kim'` 타입을 허용하는 것이기 때문이다.

### 객체의 타입 지정을 통한 해결

```
var data: {name: 'kim'} = {
  name : 'kim'
};
```

### assertion 을 통한 해결

```
myFunction(data.name as 'kim');
```

### as const 를 통한 해결

1. object의 value값을 그대로 타입으로 지정해준다.
2. object 안의 속성들을 모두 `readonly` 로 바꿔준다.

```
var data = {
  name : 'kim'
} as const;
```

### literal type을 string type에 할당하면 어떻게 될까?

정상적으로 할당된다.

```
var data = {
  name : 'kim'
} as const;

function myFunction(a: string) {
	console.log(a);
}

myFunction(data.name);
```
