# 타입도 import export 해서 씁니다 그리고 namespace

## (JS문법) import export 문법

- a.ts 에 있던 변수나 함수를 가져다가 쓰고 싶은 경우에 다른 파일에서 쓰이게 내보내고 싶으면 **export 문법**으로 내보내야 한다.
- export된 변수를 가져와서 쓰고 싶은 경우 **import 문법**으로 가져와야한다.

```
(a.ts)

export var 이름 = 'kim';
export var 나이 = 30;
```

```
(b.ts)

import {이름, 나이} from './a'
console.log(이름)
```

- export 변수나 함수
- import {변수명} from '파일경로'

## TS 에서 정의된 타입을 가져다가 쓰고 싶은 경우

```
(a.ts)

export type Name = string | boolean;
export type Age = (a :number) => number;
```

```
(b.ts)

import {Name, Age} from './a'
let 이름 :Name = 'kim';
let 함수 :Age = (a) => { return a + 10 }
```

## namespace

- 타입스크립트 1.5버전 이하에는 자바스크립트 import/export 문법이 없었고, <script src=""> 를 여러개 써서 파일들을 첨부해서 사용했다.
  문제는 파일이 많아질 수록 변수명이 겹치는 위험이 발생한다는 점이다.

=> 그래서 외부 파일에서 사용하지 않을 변수들은 함수로 감써거나 타입변수들은 namespace 문법으로 숨겼다.

- TypeScript 1.5버전 이전에는 내부 모듈(Internal modules)이라고 불렸다.

**namespace export 방법**

```
(a.ts)

namespace MyNamespace {
  export interface PersonInterface { age : number };
  export type NameType = number | string;
}
```

중요한 타입정의들은 다른 파일들에서 쓰고 싶으면 안전하게 namespace 안에 써서 export 했다.

**namespace import 방법**

```
(b.ts)

/// <reference path="./a.ts" />

let 이름 :MyNamespace.NameType = '민수';
let 나이 :MyNamespace.PersonInterface = { age : 10 };

```

<reference/> 라는 태그를 이용해서 다른 파일을 import 해왔다.

```
(b.ts)

/// <reference path="./a.ts" />

let 이름 :MyNamespace.NameType = '민수';
let 나이 :MyNamespace.PersonInterface = { age : 10 };

type NameType = boolean; //사용 가능
interface PersonInterface {} //사용 가능

```

점 찍어서 사용하기 때문에, 다른 변수명을 오염시키지 않아서, 변수명 중복선언문제를 방지할 수 있어서 유용하다.
그러나, 자바스크립트 es6 버전이 나온 이후로 import as 키워드로 나름 namespace와 유사하게 중복문제를 해결가능해서 namespace는 많이 쓰이지 않는다.
