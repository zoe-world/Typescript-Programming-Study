# React + TypeScript 사용할 때 알아야할 점 2 : Redux toolkit

## Redux의 `state` , `reducer` 의 타입지정

```
import { Provider } from 'react-redux';
import { createStore } from 'redux';

interface Counter {
  count : number
}

const 초기값 :Counter  = { count: 0 };

function reducer(state = 초기값, action :{type: string}): Counter {
  if (action.type === '증가') {
    return { count : state.count + 1 };
  } else if (action.type === '감소') {
    return { count : state.count - 1 };
  } else {
    return initialState;
  }
}

const store = createStore(reducer);

// store의 타입 미리 export 해두기
export type RootState = ReturnType<typeof store.getState>

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
)
```

### 1. 초기값 변수 타입지정

`state` 의 초기값으로 쓸 변수의 타입을 지정해준다.

### 2. reducer 함수 타입지정

- `state` 타입지정

  1번의 초기값의 타입을 잘 지정했다면 별도의 타입지정이 없어도 자동으로 타입이 할당된다.

- `action` 타입지정

  `action` 의 타입은 `dispatch()` 를 할때 파라미터로 넣을 object자료와 같은 타입이어야 한다.  
  대부분 `{ type : string, payload : number }` 의 형식과 같다.

### `state` 를 꺼낼 때

redux에 있던 `state` 를 가져오려면 `useSelector` 훅을 사용해 쉽게 가져올 수 있다.  
`state` 를 변경하려면 `useDispatch` 훅을 사용해 dispatch를 간단히 날릴 수 있다.

```
import React from 'react';
import { useDispatch, useSelector } from 'react-redux'
import { Dispatch } from 'redux'
import { RootState } from './index'

function App() {
  const 꺼내온거 = useSelector( (state: RootState) => state );
  const dispatch: Dispatch = useDispatch();

  return (
    <div className="App">
      { 꺼내온거.count }
      <button onClick={ () => { dispatch({type: '증가'}) } }>
        버튼
    	</button>
      <Profile name="kim"></Profile>
    </div>
  );
}
```

#### 1. `useSelector` 타입 지정

`useSelector` 안에 콜백함수를 넣으면 그 파라미터가 그대로 `state` 가 된다.

```
const 꺼내온거 = useSelector( (state: RootState) => state );
```

`state` 가 어떻게 생겼는지 파악한 다음 타입알아서 손수 지정할 수 있다.

또는

위 방법처럼 타입을 미리 선언한 후 ( `RootState` 로 선언 ) , `import` 해서 가져올 수 있다.  
그러기 위해서 `store` 를 선언한 곳에서 미리 타입을 `export` 해두어야 한다

```
// store의 타입 미리 export 해두기

export type RootState = ReturnType<typeof store.getState>;
```

#### 2. `useDispatch` 타입 지정

`useDispatch` 의 타입지정을 하면 `dispatch()` 명령을 할때 안에 파라미터가 빠지면 에러를 발생시켜준다.

```
import { Dispatch } from 'redux'

const dispatch: Dispatch = useDispatch();
```

## Redux-toolkit의 타입지정

1. state 초기값의 타입을 지정한다.

2. reducer 안의 `action` 파라미터의 타입을 지정한다.

   ```
   reducers: {
     incrementByAmount (state, action: PayloadAction<number>) {
       state.count += action.payload;
     }
   }
   ```

   `action` 타입지정은 방법이 따로 있는데 위와 같은 방법을 권장한다.  
   나중에 dispatch할 때 보내는 데이터가 있으면 그걸 payload 라고 부르는데,  
   그 자료의 타입을 `<>` 안에 집어넣어서 타입지정을 한다는 뜻이다.

### `createSlice` , reducer의 `store` 등록 및 `export`

```
import { createSlice, configureStore } from '@reduxjs/toolkit';
import { Provider } from 'react-redux';

const 초기값 = { count: 0, user : 'kim' };

const counterSlice = createSlice({
  name: 'counter',
  initialState : 초기값,
  reducers: {
    increment (state) {
      state.count += 1;
    },
    decrement (state) {
      state.count -= 1;
    },
    incrementByAmount (state, action: PayloadAction<number>) {
      state.count += action.payload;
    }
  }
});

let store = configureStore({
  reducer: {
    counter1 : counterSlice.reducer
  }
});

//state 타입을 export
export type RootState = ReturnType<typeof store.getState>;

//수정방법 만든거 export
export let {increment, decrement, incrementByAmount} = counterSlice.actions;
```

1. `createSlice()` 로 `slice` 를 생성한다. `slice` 는 `state` 와 `reducer` 를 결합한 새로운 변수다.

2. `slice` 안에는 slice이름, state초기값, reducer가 정확한 이름으로 들어가야 한다.

   ```
   name: 'counter',
   initialState : 초기값,
   reducers: {
   	...
   }
   ```

3. `reducer` 는 함수 형태로 만든다. 첫 파라미터는 `state` , 둘째는 `actions` 가 자동으로 부여된다.

   ```
   increment (state) {
   	state.count += 1;
   },
   decrement (state) {
   	state.count -= 1;
   },
   incrementByAmount (state, action :any) {
   	state.count += action.payload;
   }
   ```

4. 이렇게 만든 reducer를 `configureStore` 안에 등록해준다.

   ```
   let store = configureStore({
   	reducer: {
   		counter1 : counterSlice.reducer
   	}
   })
   ```

5. 내가 만들어둔 reducer를 쓰고 싶으면 `reducer` 안의 함수명을 `export` 해준다.

   ```
   export let {increment, decrement, incrementByAmount} = counterSlice.actions;
   ```

### `state` 를 꺼낼때

```
import { useDispatch, useSelector } from 'react-redux'
import {RootState, increment} from './index'

function App() {

  const 꺼내온거 = useSelector( (state :RootState) => state);
  const dispatch = useDispatch();

  return (
    <div className="App">
      {꺼내온거.counter1.count}
      <button onClick={ () => { dispatch(increment()) } }>버튼</button>
    </div>
  );
}
```

#### 1. `useSelector` 타입 지정

기존의 redux의 타입 지정 방법 및 사용 방법이 동일하다.

#### 2. `useDispatch` 타입지정

기존 방식과 다르게 `dispatch()` 의 파라미터로 `import` 해온 수정방법 함수 ( `increment()` ) 를 넘겨준다.

```
<button onClick={ () => { dispatch(increment()) } }>
  버튼
</button>
```

타입지정은 위와 같이 예전 방식과 동일하게 하거나 아래와 같이 사용할 수 있다.

```
// index.ts

export type AppDispatch = typeof store.dispatch;
```

```
// App.tsx

import { AppDispatch } from './index'

const dispatch = useDispatch<AppDispatch>();
```
