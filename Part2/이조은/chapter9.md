# React + TypeScript 사용할 때 알아야할 점 2 : Redux toolkit

> **이번 강의 내용**  
> redux 공식 toolkit 라이브러리를 써서 이쁘게 코드짜는 - 신규방식 redux의 타입지정
> 예전처럼 if문 switch문 그런거 쓰는 - 전통방식 redux의 타입지정

## Redux 쓰는 이유

1. state를 한 곳에서 관리할 수 있고, 컴포넌트들이 props없이 state 다루기 쉽다.
2. 수정방법을 미리 reducer라는 함수로 정의해놔서 state 수정시 발생하는 버그를 줄일 수 있다.

## (전통방식 redux) state와 reducer 만들 때 타입지정 필요

- 예제로 `<button> ` 버튼을 누르면 state가 +1, -1 되는 예제를 만들어보자.

```
// index.ts
import { Provider } from 'react-redux';
import { createStore } from 'redux';

interface Counter {
  count : number
}

const 초기값 :Counter  = { count: 0 };

function reducer(state = 초기값, action : { type : string, payload : number } ) {
  if (action.type === '증가') {
    return { count : state.count + 1 }
  } else if (action.type === '감소'){
    return { count : state.count - 1 }
  } else {
    return initialState
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

1. initialState = {count:0} 이렇게 생긴 state 초기값을 만듬
2. function reducer를 만들어서 state가 변경되는 방법을 미리 정의해둠. 변경방법은 1) 증가 2) 감소 두 개이다.
3. createStore 기본 셋팅 문법

위에서 변수와 함수에 타입 지정을 하고 싶으면 하면 된다.

(1) 초기값 변수 오른쪽에 타입지정
(2) reducer 함수는 state, action 이 이름의 파라미터 2개 타입지정
action 은 나중에 dispatch 날릴 때, object 자료를 집어넣게 되는데, 그거랑 똑같이 넣어줘야한다.

## (전통방식 redux) state를 꺼낼 때

redux 에 있던 state를 가져오려면?
useSelector 훅을 쓰면 되고, state를 변경하려면, useDispatch 훅을 쓰면 dispatch를 간단히 날릴 수 있다.

```
import React from 'react';
import { useDispatch, useSelector } from 'react-redux'
import { Dispatch } from 'redux'
import {RootState} from './index'

function App() {
  const 꺼내온거 = useSelector( (state :RootState) => state );
  const dispatch :Dispatch = useDispatch();

  return (
    <div className="App">
      { 꺼내온거.count }
      <button onClick={()=>{dispatch({type : '증가'})}}>버튼</button>
      <Profile name="kim"></Profile>
    </div>
  );
}
```

1. useSelector를 쓰면 state 빼오기가 쉽다. 안에 콜백함수 넣으면 거기 있던 파라미터가 그대로 state 이다.
2. useDispatch를 쓰면 redux로 수정요청을 날릴 수 있다. type을 잘 기입하시면 미리 정의해뒀던 수정방법이 동작함

**타입지정**
(1) useSelector() 안에 파라미터 있는 곳에 하기
index.ts 에 있던 export type RootState = ReturnType<typeof store.getState> 이 코드가
store의 타입을 미리 export 해둔 걸 가져오면 된다.

(2) useDispatch 타입지정
import {Dispatch} from 'redux' 이렇게 타입을 가져오셔서 const dispatch :Dispatch 이렇게 쓰면 된다.

## (신규방식 redux) state와 reducer 만들 때 타입지정 필요

```
import { createSlice, configureStore, PayloadAction } from '@reduxjs/toolkit';
import { Provider } from 'react-redux';

const 초기값 = { count: 0, user : 'kim' };

const counterSlice = createSlice({
  name: 'counter',
  initialState : 초기값,
  reducers: {
    increment (state){
      state.count += 1
    },
    decrement (state){
      state.count -= 1
    },
    incrementByAmount (state, action :PayloadAction<number>){
      state.count += action.payload
    }
  }
})

let store = configureStore({
  reducer: {
    counter1 : counterSlice.reducer
  }
})

//state 타입을 export 해두는건데 나중에 쓸 데가 있음
export type RootState = ReturnType<typeof store.getState>
export type AppDispatch = typeof store.dispatch

//수정방법 만든거 export
export let {increment, decrement, incrementByAmount} = counterSlice.actions
```

1. createSlice() 로 slice 를 만들어주는데, slice는 state와 reducer를 합쳐놓는 것이다.
2. slice 안에는 slice 이름, state 초기값, reducer가 정확한 이름으로 들어가야한다.
3. state는 원하는데로 만들고, reducer는 함수 형태로 만들어주면 되는데, 첫 파라미터는 state, 둘쨰는 actions 이 자동으로 부여된다.
4. 만든 것들은 configureStore 안에 등록하면된다.
5. 만들어둔 reducer를 쓰고 싶으면, reducer안의 함수명을 export 해주면 된다.

**타입지정**
(1) state 초기값 타입지정하기
(2) reducer 안의 action 파라미터의 타입지정

## (신규방식 redux) state를 꺼낼 때

```
import { useDispatch, useSelector } from 'react-redux'
import {RootState, increment} from './index'

function App() {

  const 꺼내온거 = useSelector( (state :RootState) => state);
  const dispatch = useDispatch<AppDispatch>();

  return (
    <div className="App">
      {꺼내온거.counter1.count}
      <button onClick={()=>{dispatch(increment())}}>버튼</button>
    </div>
  );
}
```

1. useSelector 함수를 써서 state 를 쉽게 꺼내기 안에 콜백함수 ()=>{} 하나를 집어넣으면 되는데, 첫 파라미터는 항상 state 가 된다.
   2.useDispatch 쓰자.

**타입지정**

(1) RootState : reducer 만든 곳(=store)에서 미리 RootState라는 타입을 export 해두면, import해서 쉽게 타입 지정 가능
(2) AppDispatch : dispatch 타입 가져옴
