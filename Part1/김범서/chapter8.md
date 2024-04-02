# 타입스크립트로 HTML 변경과 조작할 때 주의점

## HTML 찾고 변경해보기

" 'title'은(는) 'null'일 수 있습니다. " 라는 에러가 발생한다.  
이유는 셀렉터로 html을 찾으면 타입이 `Element | null` 이기 때문이다.

```
let title = document.querySelector('#title');
title.innerHTML = '반갑소'; // 'title'은(는) 'null'일 수 있습니다.
```

### 방법 1. type narrowing

```
if (title != null) {
  title.innerHTML = '반갑소';
}
```

### 방법 2. `instanceof` 를 사용한 type narrowing

```
if (title instanceof HTMLElement) {
  title.innerHTML = '반갑소';
}
```

`instanceof` 는 왼쪽의 object가 오른쪽의 클래스의 하위요소인지를 판별해 `boolean` 타입으로 리턴을 해준다.

### 방법 3. type assertion

```
let title = document.querySelector('#title') as HTMLElement;
title.innerHTML = '반갑소';
```

as를 쓰면 타입실드가 해제되기 때문에 가급적 지양한다.

### 방법 4. optional chaining

```
if (title?.innerHTML != undefined) {
  title.innerHTML = '반갑소'
}
```

`title` 에 `innerHTML` 이 있으면 출력해주고 없으면 `undefined` 를 리턴해준다.

## html 태그의 타입명칭

`a` 태그는 `HTMLAnchorElement`  
`img` 태그는 `HTMLImageElement`  
`h4` 태그는 `HTMLHeadingElement`

등등 html 태그는 종류별로 정확한 타입명칭이 있다.

```
let link = document.querySelector('#link');
if (link instanceof HTMLElement) {
  link.href = 'https://kakao.com'; // error
}
```

아래와 같이 정확한 타입명칭으로 narrowing 해준다.

```

let link = document.querySelector('#link');
if (link instanceof HTMLAnchorElement) {
  link.href = 'https://kakao.com';
}
```

## 이벤트리스너 부착해보기

```
let btn = document.getElementById('button');
btn?.addEventListener('click', function() {
  console.log('안녕');
});
```

버튼이라는 변수가 없을 경우 그 자리에 undefined를 내보내고,  
HTMLElement로 잘 있으면 addEventListener() 잘 부착해주기 때문에 이것도 일종의 narrowing 이다.

## TS로 html 조작 문제

html 안에 test.jpg를 보여주고 있는 이미지 태그가 있다고 칩시다.

1. 이미지를 new.jpg 라는 이미지로 바꾸고 싶으면 자바스크립트 코드를 어떻게 짜야할까요?
2. 3개의 링크가 있는데 이 요소들의 href 속성을 전부 https://kakao.com으로 바꾸고 싶은 겁니다.

```
const btn = document.querySelector(".btn");
const img = document.querySelector("#image");

const linkNodes = document.querySelectorAll(".naver");

btn?.addEventListener("click", () => {
    if (img instanceof HTMLImageElement) img.src = "new.jpg";   // 1

    linkNodes.forEach((nodes) => {                              // 2
    if (nodes instanceof HTMLAnchorElement) {
      nodes.href = "https://kakao.com";
    }
  });
});
```

### 그 외 narrowing
```
const img = document.querySelector<HTMLImageElement>("#image"); 
const img: HTMLImageElement = document.querySelector("#image")!;
const linkNodes: NodeListOf<HTMLAnchorElement> = document.querySelectorAll(".naver");
```