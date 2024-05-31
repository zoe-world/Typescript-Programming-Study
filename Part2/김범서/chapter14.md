# object index signatures

object 자료에 타입을 미리 만들어주고 싶은데

1. object 자료에 어떤 속성들이 들어올 수 있는지 아직 모르는 경우
2. 타입지정할 속성이 너무 많은 경우

index signatures 를 사용할 수 있다.

```ts
interface StringOnly {
  [key: string]: string;
}

let obj: StringOnly = {
  name: "kim",
  age: "20",
  location: "seoul",
};
```

`[key: string]: string` 는  
" `string` 타입으로 들어오는 모든 key값에 할당되는 value는 `string` 타입이다. "  
라는 뜻이 된다.

## index signature와 중복되는 속성이 있는 경우

```ts
interface StringOnly {
  age: number;
  [key: string]: string;
}
```

모든 속성이 `string` 타입이면서 `age` 의 타입은 `number` 일수 없기 때문에 에러가 발생한다.  
따라서 아래와 같이 수정할 경우 에러가 발생하지 않는다.

```ts
interface StringOnly {
  age: number;
  [key: string]: string | number;
}
```

## Recursive Index Signatures

만들어둔 타입을 타입안에서도 사용 할 수 있다.  
이렇게 만든 타입들을 Recursive한 타입이라고 부른다.

```ts
interface MyType {
  "font-size": MyType | number;
}

let obj: MyType = {
  "font-size": {
    "font-size": {
      "font-size": 14,
    },
  },
};
```

`'font-size': MyType;` 는 `'font-size'` 속성 안에 `MyType` 타입을 가지고 있다.  
즉, `'font-size': 'font-size': MyType;` 의 형태가 무한히 반복되게 된다.
