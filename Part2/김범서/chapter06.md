# 타입도 import export 해서 씁니다 그리고 namespace

## 타입의 `import` , `export`

만든 타입변수를 다른 파일에서 사용하고 싶은 경우 자바스크립트 `import` `export` 문법을 사용 할 수 있다.

```
// a.ts

export var 이름 = 'kim';
export var 나이 = 30;
```

변수를 다른 파일에서 쓰이게 내보내고 싶으면 `export` 문법으로 내보낸다.

```
// b.ts

import {이름, 나이} from './a';
console.log(이름);
```

`export` 된 변수를 가져와서 쓰고 싶으면 `import` 문법으로 가져온다.

#### 정의된 타입을 가져다 쓰고 싶은 경우에도 동일하게 사용한다.

```
// a.ts

export type Name = string | boolean;
export type Age = (a: number) => number;
```

```
// b.ts

import {Name, Age} from './a';
let 이름: Name = 'kim';
let 함수: Age = (a) => { return a + 10 };
```

#### 과거엔 namespace를 썼다.
과거에는 `export` `import` 문법이 없어 파일들에서 변수명이 겹치는 위험이 많이 발생했다.  
그래서 외부 파일에서 사용하지 않을 변수들은 함수로 감싸는 형식으로 해결했고,    
타입변수들은 `namespace` 문법으로 숨겼다.

```
namespace MyNamespace {
  export interface PersonInterface { age : number };
  export type NameType = number | string;
} 

let 이름 :MyNamespace.NameType = '민수';
let 나이 :MyNamespace.PersonInterface = { age : 10 };
```

중요한 타입정의들을 다른 파일들에서 쓰고 싶으면 안전하게 `namespace` 안에 써서 `export` 해줬다. 
