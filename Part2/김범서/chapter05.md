# class에서 사용가능한 protected, static 키워드

## class의 `extends`

class는 `extends` 라는 문법을 쓰면 다른 class 만들 때 기존 class를 복사해 올 수 있다.

```
class NewUser extends User { }
```

## `protected`

기본적으로 `private` 와 비슷한 역할을 한다.
`protected` 를 사용하면 `extends` 된 class안에서도 사용이 가능해진다.

```
class User {
  protected x = 10;
}

class NewUser extends User {
  doThis() {
    this.x = 20;
  }
}
```

`x` 가 `private` 속성일 경우엔 에러가 나지만,  
`x` 가 `protected` 속성일 경우엔 에러가 나지 않는다.

그래서 class 여러개 만들 때 class 끼리 공유할 수 있는 속성을 만들고 싶으면 `protected`,  
class 하나 안에서만 쓸 수 있는 속성을 만들고 싶으면 `private` 를 사용 하면 된다.

## `static`

class { } 안에 집어넣는 변수, 함수는 전부 class로 부터 새로 생성되는 object (일명 instance) 에 부여된다.  
근데 class에 직접 변수나 함수를 부여하고 싶으면 `static` 키워드를 왼쪽에 붙여주면 됩니다.

```
class User {
  x = 10;
  y = 20;
}

let john = new User();
john.x; //가능
User.x; //불가능
```

`x` 와 `y` 같은 변수들은 `User` 로 부터 생성된 object들만 사용가능하다.

```
class User {
  static x = 10;
  y = 20;
}

let john = new User();
john.x; // 불가능
User.x; // 가능
```

하지만 `static` 키워드를 사용하면  
`john` 은 사용 불가능하고  
`User` 는 직접 사용이 가능하다.

- 함수도 `static` 같은 키워드를 사용할 수 있다.
- `extends` 로 class를 복사할 경우 `static` 도 함께 따라온다.
- `static` 은 `private` , `protected` , `public` 키워드와 동시에 사용가능하다. 