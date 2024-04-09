# class에서 사용가능한 protected, static 키워드

## (JS 문법)class는 extends 로 복사가능하다.

class는 extends 라는 문법을 쓰면 다른 class 만들 때 기존 class에 있던 걸 전부 복사붙여넣기가 가능하다.

```
class NewUser extends User {
  ~~ 어쩌구
}
```

새로운 NewUser class 를 만들고 싶으면, User 앞에다가 extends 를 사용하면 되는데, 기존과 비슷한 class를 많이 만들어야할 때 주로 사용한다.

## class 안에서 쓰는 protected 키워드

private와 유사하게 동작하지만, 다른 점은 private 는 클래스 외부에서 접근불가능 하지만, protected는 파생클래스에서 접근 가능하다.

User 라는 class 의 x 속성은 protected 이다.
그럼 private와 동일하게 class 안에서만 사용이 가능해지며, User의 자식들도 함부로 사용이 불가능하다.

```
class User {
  protected x = 10;
}
```

아래는 User를 extends 하는 NewUser class를 만들었다. NewUser가 갑자기 this.x 이런 식으로 x를 가져다가 쓰려고 하면 x가 private 속성일 경우, 에러가 나지만 protected 속성일 경우 에러가 나지 않는다.

```
class User {
  protected x = 10;
}

class NewUser extends User {
  doThis(){
    this.x = 20;
  }
}
```

> 따라서, class를 여러개 만들 때 class 끼리 공유할 수 있는 속성을 만들고 싶으면? protected
> class 하나 안에서만 쓸 수 있는 속성을 만들고 싶으면? private 를 쓰도록 하자.

## class 안에서 쓰는 static 키워드

class {} 안에 집어넣는 변수, 함수 이런 건 전부 class 로부터 새로 생성되는 object (일명 instance)에 부여된다. 근데 class에 직접 변수나 함수를 부여하고 싶으면? static 키워드를 왼쪽에 붙여주면 된다.

```
class User {
  x = 10;
  y = 20;
}

let john = new User();
john.x // x와 y같은 변수들은 User로 부터 생성된 object들만 사용가능
User.x //class는 사용 불가능
```

```
class User {
  static x = 10;
  y = 20;
}

let john = new User();
john.x //불가능
User.x //가능
```

함수에도 static 붙이기가 가능하고, extends 로 class를 복사할 경우 static 붙은 것들도 따라와서 사용가능하다.

```
class User {
  private static x = 10;
}
```

(참고) static은 private, protected, public 키워드와 동시 사용이 가능하다.

Q. static 이런 걸 언제 쓸까요?

- 주로 class 안에 간단한 메모를 하거나, 기본 설정값을 입력하거나 class로부터 생성되는 object가 사용할 필요가 없는 변수들을 만들어놓고 싶을 때 사용한다.

```
class User {
  static skill = 'js';
  intro = User.skill + '전문가입니다'
}
var 철수 = new User();
console.log(철수)

```

User class 를 만들었는데, 자식들에게 { intro : 'js 전문가입니다' } 를 복사해주고 싶다.
근데 js 라는 단어는 중요할 거 같아서, static skill 이곳에다가 메모해놓고 그걸 사용했다.

그럼 자식들은 철수.intro 를 사용할 때마다 'js 전문가입니다~' 를 출력해준다.

만약 여기서, skill을 변경하고 싶으면 어떻게 해야할까?

```
class User {
  static skill = 'js';
  intro = User.skill + '전문가입니다'
}
var 철수 = new User();

User.skill = 'python';

var 민수 = new User();

console.log(민수); // User {intro: 'python전문가입니다'}

```

User.skill 을 수정해버리면, 새로 생성되는 민수 부터는 'python전문가입니다' 로 나오게 된다.

---

(숙제1) 다음 x,y,z 속성의 특징을 설명해봐라.

```
class User {
  private static x = 10;
  public static y = 20;
  protected z = 30;
}
```

1. 필드값은 원래 모든 User 자식들에게 물려주는 속성이지만, static이 붙은 x,y 는 this.x,this.y 사용 불가능하고, User.x 이런 식으로 접근해서 쓸 수 있다.
2. private static x는 class 내부에서만 수정가능
3. public static y class 내부, 외부 전부 수정 가능
4. protected z 는 private 키워드와 유사하게, class 내부에서만 사용이 가능하지만 extends로 복사한 class 내부에서는 사용이 가능하다.

(숙제2) x 속성에 숫자를 더해주는 함수가 필요합니다.

```
class Adduser {
  private static x = 10;
  public static y = 20;

  static addOne(z: number) {
    Adduser.x += z;
  }
  static printX() {
    console.log(Adduser.x);
  }
}
Adduser.addOne(30);
Adduser.addOne(10);
Adduser.printX();
```

(숙제3)

```
let 네모 = new Square(30, 30, 'red');
네모.draw()
네모.draw()
네모.draw()
네모.draw()
```
