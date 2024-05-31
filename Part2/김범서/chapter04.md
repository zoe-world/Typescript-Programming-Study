# public, private 쓰는거 보니까 타입스크립트 귀여운편

## `public` 키워드

클래스 내부의 원하는 속성 왼쪽에 붙이면 그 속성은 어디서든 수정이 가능하다.

```ts
class User {
  public name: string;

  constructor() {
    this.name = "kim";
  }
}

let user1 = new User();
user1.name = "park";
```

`public` 이 붙은 속성은 자식 object들이 마음대로 사용하고 수정가능하다.
필드값 앞에 `public` 을 선언하지 않아도 생략된 상태로 동일하게 동작한다.

### public, private 키워드 쓰면 이런 것도 가능

`constructor` 파라미터에 `public` 을 붙이면 `this.name = name` 과 같은 코드를 생략가능하다.  
`Person` 으로부터 새로 생산되는 object들은 `name` 속성을 가질 수 있다.

```ts
class Person {
  name;
  constructor(name: string) {
    this.name = name;
  }
}
let Person1 = new Person("john");
```

위와 아래의 코드는 동일하게 동작한다.

```ts
class Person {
  constructor(public name: string) {}
}
let Person1 = new Person("john");
```

## `private` 키워드

`private` 키워드를 붙이면 `class` 내부에서만 수정 및 사용이 가능하다.

그러므로 class로부터 생산된 자식 object에서도 `private` 가 붙은 값은 사용이 불가능하다.

```ts
class User {
  public name: string;
  private familyName: string = "kim";

  constructor() {
    this.name = "어쩌구";
    let hello = this.familyName + "안뇽"; // 가능
  }
}

let user1 = new User();
user1.name = "bumseo"; // 가능
user1.familyName = "park"; // 'familyName' 속성은 private이며 'User' 클래스 내에서만 액세스할 수 있습니다.
```

이렇게 속성을 외부에서 숨기고 싶을 때 `private` 키워드를 이용한다.  
바닐라JS 문법에서도 `#` 을 속성옆에 붙이면 `private` 속성이 된다.

`private` 키워드는 class내의 함수에도 붙일 수 있다.

### private 부여된 속성을 class 밖에서 수정해야할 경우?

private 속성을 수정하는 함수를 class 안에 만들어 외부에서 실행시킨다.

```ts
class User {
  public name: string;
  private familyName: string = "kim";

  constructor() {
    this.name = "어쩌구";
    let hello = this.familyName + "안뇽"; // 가능
  }
  changeSecret() {
    this.familyName = "park";
  }
}

let user1 = new User();
user1.changeSecret();
```
