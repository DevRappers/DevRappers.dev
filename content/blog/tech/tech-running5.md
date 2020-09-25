---
title: 우아한 테크러닝 5회차
date: 2020-09-16 17:10:00
category: 우아한테크러닝
thumbnail: './images/tech_running.png'
draft: false
---

![tech](./images/tech_running.png)

> 우아한 테크러닝 3기 React&TypeScript 5회차

오늘은 어제 수강한 우아한 테크러닝 3기 React&TypeScript 5회차에 대한 정리 해보려 한다.

## 5회차에 배운 내용

5회차에서는 Redux에 기본과 리덕스 미들웨어에 대해 배웠다.

## Redux

지난 회차에서 작성했었던 tiny redux를 이용해서 store와 reducer를 생성해 보았다.

```js
import { createStore } from 'redux'

function reducer(state = { counter: 0 }, action) {
  switch (action.type) {
    case 'INC':
      return {
        ...state,
        counter: state.counter + 1,
      }
    default:
      return { ...state }
  }
}

const store = createStore(reducer);

store.subscribe(() => {
  console.log(store.getState());
})
```
store와 reducer를 생성해 준 이후에 subscribe 메서드를 사용해서 변경사항을 확인하도록 하였다.

```js
store.dispatch({type: "INC"}); 
store.dispatch({type: "INC"});
```
이 로직은 따로 작동하는 것 같지만 동기적으로 진행된다. 동기적으로 작동하기 때문에 reducer는 반드시 순수 함수여야 한다.

redux를 사용하는 경우 실행할때마다 결과가 달라져야 하는 경우가 있다. (API통신) 이를 위해 먼저 커링이라는 개념을 배워보자.

### 커링
```js
const add1 = function (a, b) {
    return a + b;
};

const add2 = function (a) {
    return function (b) {
        return a + b;
    };
};
```
위 두함수의 차이는 무엇일까? 첫번째 함수의 경우는 1차 함수이며 호출시 바로 반환한다. 그와 다르게 
두번째 함수는 2차함수로 작성되어 있다. 이는 이런식으로 호출 가능한 것이다.
```js
const addTen = add2(10);
console.log(addTen(20)); // 30
console.log(addTen(40)); // 50
```
add2 함수는 위와 같이 함수를 한번 더 호출해야 함수가 지연 호출되게 된다. 이런 개념을 `커링`이라 한다.

### Redux의 비동기, 미들웨어
setTimeout을 통해서 비동기와 동일하게 동작하는 API 함수를 만들어보자.
```js
function api(url, cb) {
    setTimeout(() => {
        cb({ type: "응답", data: [] });
    }, 2000);
}

function reducer(state = { counter: 0 }, action) {
    switch (action.type) {
        case "INC":
            return {
                ...state,
                counter: state.counter + 1,
            };
        case "FETCH_USER":
            api("/api/users/1", (users) => {
                return { ...state, ...users };
            });
        default:
            return { ...state };
    }
}

store.dispatch({ type: "FETCH_USER" });
```
이렇게 작성하게 되면 2초뒤에 응답이 반환될 것이라고 생각할 것이다. 하지만 Redux에서는 모든 로직을 동기적으로 처리하기 때문에
값이 반환되는 것을 기다리지 않는다. 따라서 Redux에서 비동기 작업을 하기 위해서는 `미들웨어`를 사용해야 한다.

미들웨어란 데이터가 흘러가는 과정 중간에 거쳐가는 것을 미들웨어라고 한다.
```js
const indexMiddleware = store => dispatch => action => {
  dispatch(action);
}

/* 커링2 - indexMiddleware와 같음 */
function testMiddleware (store) {
  return function (dispatch) {
    return function (action) {
      dispatch(action);
    }
  }
}

function testMiddleware2 (store, dispatch, action) {
  dispatch(action);
}

indexMiddleware(store)(store.disapatch)({ type: 'inc' });
testMiddleware(store)(store.disapatch)({ type: 'inc' });
testMiddleware2(store, store.disapatch, { type: 'inc' });
```
Redux 자체를 수정하기엔 위험부담이 크고 추후 유지 보수에도 문제가 있으니 중간에 미들웨어를 통해서 
dispatch 전에 어떤 행동을 취하게 하도록 한다.

## 마무리 🚀 
수업이후에 바로 작성을 했어야 했는데 늦게 작성을 하는 바람에 중간중간 생략된 내용이 많다. 
다음부턴 바로바로 해야겠다.