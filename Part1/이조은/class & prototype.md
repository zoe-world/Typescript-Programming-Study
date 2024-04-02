# class 문법

## class 문법을 쓰는 경우

비슷한 object 를 많이 만들어야하는 경우, class로 만들어쓰면 된다.

```

function 기계(구멍){
    this.q = 구멍;
    this.w = 'snowball';
}
var nunu = new 기계('consume');
var garen = new 기계('strike');

class Hero{
    constructor(구멍){
        this.q = 구멍;
        this.w = 'snowball';
    }
}

```

this 란? 기계로부터 생성되는 object(instance)

# prototype

## prototype란

clas로 함수를 만들면, prototype 이라는 속성을 추가해줌
