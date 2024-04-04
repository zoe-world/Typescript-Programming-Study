# public, private

## public, private 키워드로 사용제한두기

### public

public이 붙은 속성은 자식 object들이 마음대로 사용하고, 수정이 가능하다.

- (참고) public 키워드는 class 내의 prototype 함수에도 붙일 수 있습니다.

```
class User {
  public name: string;

  constructor(){
    this.name = 'kim';
  }
}

let 유저1 = new User();
유저1.name = 'park';  //가능
```

### private

private이 붙은 속성은 아무데서나 수정이 불가능하고, <span style="color:red">오직 class {} 안에서만 수정이 가능하다.</span>

개발시에 중요한 변수나 속성들에 사용한다. 외부에서 실수로 수정하지 않도록 private 를 붙여준다.

- (참고) private 키워드는 class 내의 함수에도 붙일 수 있다.

```
class User {
  public name :string;
  private familyName :string;

  constructor(){
    this.name = 'kim';
    let hello = this.familyName + '안뇽'; //가능
  }
}

let 유저1 = new User();
유저1.name = 'park';  //가능
유저1.familyName = 456; //에러남
```

## public, private 키워드의 쓰임

```
class Person {
  name;
  constructor ( name :string ){
    this.name = name;
  }
}
let 사람1 = new Person('john')


class Person {
  constructor ( public name :string ){

  }
}
let 사람1 = new Person('john')
```

- 위 두개의 코드는 같은 역할을 하는 코드이다. constructor 파라미터에 public을 붙이면 this.name = name이 생략이 가능하고, 이제 Person으로부터 새로 생산되는 object들은 name 속성을 가질 수 있다.
