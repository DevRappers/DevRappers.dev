---
title: 우아한 테크러닝 2회차
date: 2020-09-04 12:00:39
category: 우아한테크러닝
thumbnail: "./images/tech_running.png"
draft: false
---

![tech](./images/tech_running.png)

> 우아한 테크러닝 3기 React&TypeScript 2회차 

오늘은 어제 수강한 우아한 테크러닝 3기 React&TypeScript 2회차에 대한 정리 해보려 한다.

## 2회차에 배운 내용 
2회차에서는 우아한 테크러닝 3기의 참여자들의 수준을 맞추고, 주니어 프론트엔드 개발자들에게 
가장 필요한 개념인 자바스크립트 A~Z까지의 흐름을 파악하고, 직접 자바스크립트를 이용해 상태관리 도구인 Redux를 만들어보는 것을 배웠다.

## JavaScript

### JavaScript 타입, 변수 선언 
JavaScript 타입은 원시, 객체 타입으로 구분되며 변수는 `var`, `let`, `const` 키워드를 사용해 선언할 수 있다.
```javascript
var x = 10;
let y = 20;
const z = 30;
```
위 코드에서 x, y, z는 변수이며 10, 20, 30은 값에 해당한다. 

JavaScript 에서 값이라고 정의한 것들은 변수에 넣을 수 있는 규칙이 존재한다. 
이 값이라고 하는 것은 우리가 흔히 아는 숫자, 문자 뿐만 아니라 객체, 함수 등이 존재한다.

### JavaScript 함수
> JavaScript 의 모든 함수는 값을 반환한다.
> 
> 함수는 값으로 취급하여 변수에 넣을 수 있다.

이렇게 함수를 정의하는 방법에는 함수정의문과 함수정의식이 있다.

<strong>1.함수정의문</strong>
```javascript
function foo() {}
```

<strong>2.함수 정의식</strong>
```javascript
const bar = function bar() {};
bar();

// 함수를 값으로 취급할 땐 함수이름이 생략 가능하다.
const boo = function(){};
```

여기서 궁금증이 생긴다면 과연 식과 문의 차이점은 무엇일까? 생각하게 될 것이다.
식이란 실행결과가 값이 되는 것을 식이라 하며 그외의 모든 것은 문이라고 한다.

문에는 반복문인 while, for 조건문인 if 같은 것이 해당된다.
이것들의 특징으로 식에는 ;(세미콜론)이 붙고 문에는 ;(세미콜론)을 붙이지 않는다.

<strong>3.즉시 호출 함수</strong>
```javascript
(function(){})();
```
익명 함수를 값으로 만들어 사용할 때에는 ()를 사용해 즉시 호출해주면 된다. 이는
예전에 많이 사용된 방법으로 단 한번만 함수를 실행할 때 주로 사용된다고 한다.

<strong>4.함수의 매개변수</strong>
```javascript
function foo(x) {
  x();
  return function() {
    
  }
}

const y = foo(function() {});
```
여기서 foo안에 파라미터로 함수를 넣을 수 있는 것은 아까 말했듯 함수 또한 값으로 취급하기 때문이다. 
함수는 꼭 값을 반환한다는 규칙을 가지고 있기 때문에 함수를 반환해도 된다. 

위와 같이 함수를 파라미터로 넘겨주는 것을 콜백함수라고 하며 호출을 위임하는 역할을 한다. (해당 함수에서 끝맺음을 지을 수 없고 추가적인 작업이 필요할 경우 사용)
그리고 위와 같이 함수를 인자로 받고 함수를 반환하는 foo같은 함수를 1급 함수(High Order Function)라고 한다.

<strong>5.재귀 호출 함수</strong>
```javascript
const foo = function foo(x){
    foo(x);    
}
```
재귀 호출 함수란 자기 자신을 호출하는 것으로 자기 자신을 호출해야 하기 때문에 함수의 이름을 반드시 적어주어야 한다.

<strong>6.ES6 이후의 함수 (Arrow Function)</strong>
```javascript
const foo = () => {};
```
ES6이후 함수식을 만들때는 `=>`를 이용해 만들 수 있다. 화살표를 사용하기 때문에 화살표 함수라고 하며, 강의자이신 민태님은
화살표함수의 이름이 촌스러워 별로라고 하신다.. 😂 

화살표 함수는 한줄 함수라고도 불리며 하나의 식을 반환할 때 자주 사용된다.

```javascript
// 한줄 일 경우 {}를 생략해도 된다. return 생략
const x = (a) => a*2; 

// parameter가 한개일 경우 ()를 생략할 수 있다.
const y = a => a*2;
```

<strong>7.new 연산자 함수와 인스턴스 객체</strong>
```javascript
function foo() {
  this.name = "DevRappers"
}

const obj = new foo();

console.log(obj) // foo {name: "DevRappers", constructor: Object}

if(obj instanceof foo){
  console.log(true);    
}
```
`new` 연산자를 통해 `this`를 생성하게 되고 인스턴스 객체는 생성된 this를 반환하게 된다.
그래서 obj를 출력할 경우 저러한 결과 값이 나오게 되는 것을 알 수 있다.

JavaScript 에서 생성된 객체의 타입을 확인하기 위해서는 `instanceof`를 사용 할 수 있다.

<strong>8.클래스</strong>

ES6이전에는 `new` 연산자를 통해 인스턴스를 생성 할 수 있었지만 ES6에서는 클래스를 통해 인스턴스가 생성이 가능해졌다.
```javascript
class bar(){
  constructor(){
   this.name = "DevRappers";
  }   
}

console.log(new bar()); // bar {name: "DevRappers", constructor: Object}
```

결과적으로 같아 보이지만 `class`를 사용하게 되면 훨씬 명시적인 코드가 되게 된다.
그리고 함수로만든 foo같은 경우는 `new`연산자 없이 호출이 가능하지만,
`class`로 만들게 되면 `new` 호출을 강제해 사용가능하다.

<strong>9.this</strong>

```javascript
const person = {
    name: "DevRappers",
    getName() {
        return this.name;
    },
};

// DevRappers
console.log(person.getName());
```
이 코드에서 this.name은 DevRappers를 가르킨다.
이 과정을 설명하자면 getName을 실행하게 되면 자바스크립트 엔진이 getName의 소유자를 확인하게 된다.
소유자는 person임을 확인하고 DevRappers를 출력하게 된다.

```javascript
const man = person.getName();
console.log(man());
```
이렇게 실행하게 된다면 man을 실행하는 순간 호출자를 찾을 수 없기 때문에 전역객체(window)에서 찾게 된다.
window에서 찾을 수 없기 때문에 에러가 발생하게 된다. 그럼 이렇게 소유자를 찾을 수 없을 때 어떻게 해야할까?

```javascript
// 1. bind를 통한 소유자 고정
const man = person.getName.bind(person);

// 2. call, apply 같은 함수 호출 시 소유자 정해주기 
person.getName.call(person);
```

<strong>10.클로저</strong>

```javascript
function foo(x){
  return function bar(){
    return x; 
  } 
}

const f = foo(10);

console.log(f()); //10
```
foo함수는 bar함수를 반환하는 함수이다. 즉 bar함수를 가지고 있는 f는 f()로 호출 가능하며 10이라는 값을 반환한다.
foo함수가 호출이 완료된 시점에서 foo의 스코프는 사라지지만 반환된 bar에서는 x를 기억하게 된다.

이처럼 반환된 함수가 외부의 스코프를 기억하고 있는 상태를 `클로저`라 한다.

```javascript
const person = { age: 23};
person.age = 500;   // 값을 변조 가능하다.

function makePerson() {
 let age = 10;
 return {
  getAge() {return age;},
  setAge(x) {
    age = x>1 && x< 130 ? x: age;    
  }  
 }  
}

let p = makePerson();
console.log(p.getAge());
```
`클로저`를 사용하지 않는다면 값을 변조할 수 있는 문제가 생긴다. 이를 해결하기 위해
`클로저`를 사용해서 값을 `캡슐화`해 보호할 수 있다.

<strong>11.비동기</strong>
```javascript
setTimeout(function(){
  console.log("foo");
  setTimeout(function() {
    console.log("bar");
  },2000);
},1000);
```
초창기의 자바스크립트에서는 이런식으로 콜백을 이용해서 비동기를 사용하였다. 
위의 코드는 2개의 depth를 갖고 있지만 더 깊어질 경우 콜백지옥의 문제가 생긴다.

이문제를 해결하기 위해 등장한 것이 바로 `Promise`이다.

```javascript
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("응답1"); 
    }, 1000);

    reject();
});

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("응답2");
    }, 1000);

    reject();
});

p1.then(p2())
    .then(function (r) {
        console.log(r);
    })
    .catch(function () {});
```
`Promise`의 then의 콜백 함수는 resolve로 호출되며 catch로 넘겨준 콜백 함수는 reject로 호출된다.
쉽게 resolve는 성공시 이 resolve를 이용한 것은 then, 실패시에는 reject, reject를 잡아주기 위해선 catch를 쓴다고 이해하면 된다.

마지막으로 가장 최신의 문법으로 Promise또한 복잡해서 나온 것이 `async-await`이다.
```javascript
const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

async function main() {
    console.log("1");
    try {
        const result = await delay(3000);
    } catch (e) {
        console.error(e);
    }
    console.log("2");
}

main();
```
await은 async키워드가 붙은 함수에서만 사용 가능하며 Promise의 resolve된 값을 저장할 수 있다.
Promise의 reject부분은 try-catch 문법을 사용하여 catch로 잡아줄 수 있다.

## Redux 만들어보기 

Redux를 만들어보기전 `Flux패턴`을 설명해주셨다. Flux패턴이란 어플리케이션에서 여러 개의 컴포넌트에서 하나의 특정 상태가 필요한
경우가 생길때 하나의 store에서 상태에 관한 정보를 저장해 사용하는 패턴이다.

React의 가상돔 이전의 어플리케이션에서는 하나의 상태가 변경될 시에는 모든화면을 리렌더링 해주었기 때문에 불가능했다.
React에서는 가상돔을 이용해 기존의 DOM과 비교해서 바뀐부분만 DOM에 반영하기 때문에 이러한 패턴의 사용이 가능해졌다.

### Redux 만들기 
먼저 짠 코드를 보자.
```javascript
// index.js 
import { createStore, actionCreator } from "./tiny-redux";

function reducer(state = {}, { type, payload }) {
  switch (type) {
    case "init":
      return {
        ...state,
        count: payload.count
      };
    case "inc":
      return {
        ...state,
        count: state.count + 1
      };
    case "reset":
      return {
        ...state,
        count: 0
      };
    default:
      return { ...state };
  }
}

const store = createStore(reducer);

store.subscribe(() => {
  console.log(store.getState());
});

store.dispatch({
  type: "init",
  payload: {
    count: 1
  }
});

store.dispatch({
  type: "inc"
});

store.dispatch(actionCreator("reset"));

const Increment = () => store.dispatch(actionCreator("inc"));

Increment();
Increment();
Increment();
```

```javascript
// redux.js
export function createStore(reducer) {
  let state;
  const listeners = [];

  const publish = () => {
    listeners.forEach(({ subscriber, context }) => {
      subscriber.call(context);
    });
  };

  const dispatch = (action) => {
    state = reducer(state, action);
    publish();
  };

  const subscribe = (subscriber, context = null) => {
    listeners.push({
      subscriber,
      context
    });
  };
        
  const getState = () => ({ ...state });

  return {
    dispatch,
    getState,
    subscribe
  };
}

// 액션 생성 함수
export const actionCreator = (type, payload = {}) => ({
  type,
  payload: { ...payload }
});
```

코드는 이런 규칙들을 가지고 짜게 되었다.
1. redux의 store에 저장된 데이터는 컴포넌트가 바꿀수 없으며 컴포넌트는 저장된 데이터를 그려주기만 해야한다.
2. 상태를 직접적으로 바꿀 수 없도록 => 클로저를 이용해 state를 은닉화 하였다.
3. createStore라는 함수를 통해 store를 생성할 수 있으며 state는 getState함수를 통해 볼 수 있다.
4. store에 저장된 값을 수정하려면 reducer 함수를 호출하여 수정해야한다.
5. reducer는 반드시 action을 인자로 받는 dispatch 함수를 통해 호출해야한다.
6. redcuer는 호출될 때 기존 state와 실행할 액션 객체를 매개변수로 전달 받는다.
7. 액션 객체 타입에 type에 따라 기존 state의 값을 변경시켜 store에 저장한다.
8. subscribe함수는 store를 구독하는 listener배열에 받는 fn을 저장해 dispatch시 실행한다.

## 마무리 🚀 
감기약을 먹고 강의를 들어서 그런지 집중이 잘 안되는 회차였지만, 오랜만에 자바스크립트를 복습하고
현재 사용하고 있는 redux를 만들어봐서 그런지 재미있고, 의미있었던 회차였다.😄