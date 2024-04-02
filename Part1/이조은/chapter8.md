# 타입스크립트로 HTML 변경과 조작할 때 주의점

## HTML 요소 찾고 변경해보기

```
let 제목 = document.querySelector('#title');
제목.innerHTML = '반갑소'
```

위와 같이 자바스크립트에서 적으면, 에러가 나지 않지만 타입스크립트는 에러를 내준다.

<span style="color:red">"제목이라는 변수가 null 일 수 있습니다."</span>
라고 에러가 뜨는데, querySelector를 쓰게 되면, 타입이 Element | null 이기 떄문이다. (html을 몾찾을 경우 null이 된다)

그래서 TS 에서는 확실치 않기 때문에, 에러를 내준다.

> 따라서 제목이라는 변수가 union type이기 때문에 if문으로 type narrowing 하거나 as 문법으로 assertion 해도 된다.

### 해결책 1. narrowing 하기

```
let 제목 = document.querySelector('#title');
if (제목 != null) {
  제목.innerHTML = '반갑소'
}
```

### 해결책 2. instanceof 사용하기

```
let 제목 = document.querySelector('#title');
if (제목 instanceof HTMLElement) {
  제목.innerHTML = '반갑소'
}
```

instanceof 라는 연산자를 쓰는 방법인데, HTMLElement 을 입력하면, 그 타입인지 체크를 해준다.

### 해결책 3. assertion 쓰기

```
let 제목 = document.querySelector('#title') as HTMLElement;
제목.innerHTML = '반갑소'
```

as 키워드를 쓰면 임시타입을 설정할 수 있기 때문에, HTMLElement 혹은 Element 로 바꿀 수 있다.

### 해결책 4. optional chaining 연산자 쓰기

```
let 제목 = document.querySelector('#title');
if (제목?.innerHTML != undefined) {
  제목.innerHTML = '반갑소'
}
```

? 연산자는 객체 속성에 접근하여 호출 할때, 자료 안에 해당 속성값이 null 이거나 unfinded 라면, undefined 를 남겨, 에러가 발생되지 않는다.

### 해결책 5. 그냥 strict 설정 false 로 끄지

null 체크가 귀찮다면, tsconfig.json 파일에서 strictNullChecks 옵션을 false 로 바꾸면 되지만, 그렇다면 굳이 Ts 를 쓸 필요가 없다고 생각한다.

```
{
    "compilerOptions": {
        ...
        "strictNullChecks": true
    }
}
```

위에 해결책 중 가장 좋은 해결책은 2 번 instanceof 연산자를 사용하는 것인데 이 연산자를 써야 조작가능한 부분이 있기 때문이다.

instanceof 연산자를 써서 html 요소를 변경해보자

## a 태그의 href 속성을 바꿔보기

html 파일에 <a href="naver.com"></a> 이런 태그가 있다.

이 태그의 href 속성을 바꾸고 싶으면 셀렉터로찾고 .href = 'https://kakao.com' 이렇게 쓰면 됩니다.

```
let 링크 = document.querySelector('#link');
if (링크 instanceof HTMLElement) {
  링크.href = 'https://kakao.com' //에러남 ㅅㄱ
}
```

위와 같이 그냥 쓰면 에러가 난다. 이유는?
html 태그 종류별로 정확한 타입명칭이 있기 때문이다.

a 태그는 HTMLAnchorElement  
img 태그는 HTMLImageElement  
h4 태그는 HTMLHeadingElement

이런식으로 매우 많은 종류가 있는데, 정확한 타입으로 narrowing 을 해줘야, html 속성 수정을 제대로 할 수 있다.

그럼 왜 정확한 타입을 설정해줘야할까?

> 타입스크립트에서 쓸 수 있는 수 HTML 타입들은
> Element, HTMLElement, HTMLAnchorElement 등이 있다.
> Element에 들어있는 걸 복사 후 추가해서 HTMLElement 타입을 만들고, HTMLElement에 들어 있는 걸 복사해서 몇개 추가해서 HTMLAnchorElement 타입을 만들어놨다.

그래서 셀렉터로 대충 찾으면 Element 타입이라는게 부여가 되기 때문에, 일반 html 태그의 특징만 들어있어, .href, .src 이런 속성은 찾기 어렵다.

## 이벤트리스너 부착하기

```
let 버튼 = document.getElementById('button');
버튼.addEventListener('click', function(){
  console.log('안녕')
})
```

위와 같이 적으면 에러가 난다. 이유는 버튼이이라는 변수가 null 값일 수도 있기 때문에 narrowing을 해줘야한다.

근데, narrowing을 하기 싫다면, optional chaining 문법을 사용할 수 있다.

```
let 버튼 = document.getElementById('button');
버튼?.addEventListener('click', function(){
  console.log('안녕')
})
```

버튼? 이렇게 사용하며느 버튼이라는 변수가 없을 경우, 그 자리에 undefined를 내보내고, HTMLElement로 잘 있으면,
addEventListener()를 잘 부착해주기 떄문에, 일종의 narrowing이라고 볼 수 있다.
