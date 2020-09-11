---
title: 우아한 테크러닝 4회차
date: 2020-09-11 15:40:00
category: 우아한테크러닝
thumbnail: './images/tech_running.png'
draft: false
---

![tech](./images/tech_running.png)

> 우아한 테크러닝 3기 React&TypeScript 4회차

오늘은 어제 수강한 우아한 테크러닝 3기 React&TypeScript 4회차에 대한 정리 해보려 한다.

## 4회차에 배운 내용

4회차에서는 React의 기본적인 상태관리와 비동기에 대해서 배우게 되었다.

## 함수형 컴포넌트의 상태관리와 컴포넌트 분리 
먼저 간단하게 React예제를 작성해보자.

`index.js`
```jsx
import React from "react";
import ReactDOM from "react";
import App from "./App";

const root = document.getElementById("root");

ReactDOM.render(<App/>, root);
```

`App.js`
```jsx
import React from "react";

const App = () => {
    return (
        <div>
            <header>
                <h1>React and TypeScript</h1>
            </header>
            <ul>
                <li>1회차: Overview</li>
                <li>2회차: Redux 만들기</li>
                <li>3회차: React 만들기</li>
                <li>4회차: 컴포넌트 디자인 및 비동기</li>
            </ul>
        </div>
    );
};

export default App;
```

간단하게 현재 진도를 출력하는 것을 만들었다. 이를 index에서 App으로 상태를 넘겨주는 코드로 수정해보자.

`index.js`
```jsx
import React from "react";
import ReactDOM from "react";
import App from "./App";

const sessionList = [
    { title: "1회차: Overview" },
    { title: "2회차: Redux 만들기" },
    { title: "3회차: React 만들기" },
    { title: "4회차: 컴포넌트 디자인 및 비동기" },
];

const root = document.getElementById("root");

ReactDOM.render(<App store={{ sessionList }} />, root);
```

`App.js`
```jsx
import React from "react";

const App = (props) => {
    const {sessionList} = props.store;
    return (
        <div>
            <header>
                <h1>React and TypeScript</h1>
            </header>
            <ul>
              {sessionList.map((session) => (
                <li>{session.title}</li>
              ))}
            </ul>
        </div>
    );
};

export default App;
```

이런식으로 코드를 구현해보았다. 여기서 태그를 반환하는 코드가 많아질 경우 가독성을 저하시키고, 눈으로 봤을때 분리시킬 수 있는 부분이 있기 때문에 컴포넌트를 분리해보자.
이 설명을 해주시면서 민태님께서 안좋은 코드로 스노우볼이 굴러가기 전에 쪼갤 수 있는 부분이 보인다면 미리미리 코드를 분리하는 편이 좋다고 하셨다. 

한번 분리해보자.

`App.js`
```jsx
import React from "react";

const SessionItem = ({ title }) => <li>{title}</li>;

const App = (props) => {
    const {sessionList} = props.store;
    return (
        <div>
            <header>
                <h1>React and TypeScript</h1>
            </header>
            <ul>
              {sessionList.map(({title}) => (
                 <SessionItem title={title} />
              ))}
            </ul>
        </div>
    );
};

export default App;
```
이렇게 분리를 해보았다. 이제 App컴포넌트에 정렬을 하기 위한 상태를 줘보자. 저번 회차에서 설명한 내용이지만 App컴포넌트에서 변수를 선언하고 함수를
선언해서 정렬되는 것을 해보려고 하면 안될 것이다. 

이유는 안에서 선언된 변수의 값은 변경되지 않으며 항상 함수호출 시 초기화 되게 때문에 상태를 가질 수 없고 React 함수형 컴포넌트에서 상태를 가지려면
`useState`라는 Hook을 사용해서 만들 수 있다 한번 만들어보자.

먼저 index.js의 리스트들의 키값을 넣어주도록 하자.

`index.js`
```jsx
//...

const sessionList = [
  { id: 1, title: "1회차 : Overview" },
  { id: 2, title: "2회차 : Redux 만들기" },
  { id: 3, title: "3회차 : React 만들기" },
  { id: 4, title: "4회차 : 컴포넌트 디자인 및 비동기" }
];

//...
```

`App.js`
```jsx
import React from "react";

const SessionItem = ({ title }) => <li>{title}</li>;

const App = (props) => {
    const [displayOrder, toggleDisplayOrder] = React.useState("ASC");
    const { sessionList } = props.store;
    const orderedSessionList = sessionList.map((session, i) => ({
       ...session,
       order: i
    }));

    const onToggleDisplayOrder = () => {
      toggleDisplayOrder(displayOrder === "ASC" ? "DESC" : "ASC");
    };

    return (
        <div>
            <header>
                <h1>React and TypeScript</h1>
            </header>
            <p>
              전체 세션 갯수 : {orderedSessionList.length}개 {displayOrder}
            </p>
            <button onClick={onToggleDisplayOrder}>재정렬</button>
            <ul>
              {orderedSessionList.map(({title}) => (
                 <SessionItem title={title} />
              ))}
            </ul>
        </div>
    );
};

export default App;
```

이런식으로 `useState`를 이용해 상태를 만들어보았다. useState는 인자로 초깃값을 넣어주며 사용하며 상태와 상태를 변경하는 함수를 반환한다.
이렇게 해서 React에 상태를 가지는 법에 대해서 간단히 알아보았다.

## 비동기 
`제너레이터`는 function* 키워드로 선언한다.
```js
function* foo() {}
```
`비동기 함수`는 function에 async를 붙여 사용한다.
```js
async function bar(){}
```
제너레이터와 비동기 함수는 둘다 `Promise`와 밀접한 관련이 있다.

다음으로 아래 코드를 한번 봐보자.
```js
const x = 10;
const y = x * 10;
```
이 두 변수 선언이 동시에 이루어 질 수 있을까? 정답은 당연히 없다 이다. 이유는 y변수에 x라는 의존성이 있으므로 x가 없이 y가 생길 수 없기 때문이다.
그럼 이를 해결하려면 어떻게 해야 할까?
```js
const x = () => 10;
const y = x() * 10;
```
이런식으로 구현해주면 된다. javascript의 함수를 반환할 수 있는 특성을 이용해 지연호출 한 것이다. 

이제 `제너레이터`에 대해 알아보자. 먼저 `제너레이터`는 코루틴이라는 함수의 구현체이다. 제너레이터라는 이름이 붙은 이유는 어떤 값을 계속 생산하기 때문이며 
`제너레이터`는 함수인데 리턴을 여러번 할 수 있게 하면 어떨까? 라는 컨셉으로 만들어졌다.

```js
function* makeNumber() {
  let num = 1;
  
  while(true){
    yield num++;
  }
}

const i = makeNumber();
console.log(i.next());
```
위 코드를 제너레이터를 모르는 상태로 보게 되면 무한루프가 돌아서 안좋은거 아니야? 라는 생각을 할 수 있다.

먼저 위 코드는 전혀 문제 없이 작동한다. makeNumber를 호출시에는 값이 반환되는 것이 아닌 제너레이터 객체가 반환된다. 
제너레이터 값을 가져오기 위해서는 제너레이터 객체의 next메서드를 이용해야 한다.
위 제너레이터 함수는 무한루프로 돌고 있는 것 같지만 next를 호출 시 yield를 만나게 되고 값을 반환해주고 대기상태가 된다. 

제너레이터는 value, done이라는 속성을 가지고 있으며 value는 yield로 반환된 값이며 done이 true가 되면 제너레이터가 종료된다.
done을 true로 만들고 싶을 경우에는 제너레이터함수에서 return 문을 만나게 하면 done이 true가 된다. 

다음 예제를 보자.
```js
function* makeNumber() {
    let num = 1;

    while (true) {
        const x = yield num++;
        console.log(x); // a
    }
}

const g = makeNumber();

console.log(g.next());
console.log(g.next("a"));
```
이런식으로 구현하게 되면 일반 함수와 다르게 함수가 어떤 상태를 가지고 있는 상태에서 함수 안쪽과 바깥쪽이 커뮤니케이션을 하는 느낌을 가지게 할 수 있다. 

마지막으로 제너레이터를 활용해 비동기적인 코드를 동기적으로 풀어내보자.

`generator`
```js
const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

function* main() {
  console.log("시작");
  yield delay(3000);
  console.log("3초 뒤");
}

const it = main();

const { value } = it.next();

value.then(() => {
  it.next();
});
```

`async-await`
```js
async function main2() {
  console.log("시작");
  await delay(3000);
  console.log("3초 뒤이다.");
}

main2();
```
이런식으로 delay함수를 통해 3초 뒤에 콘솔을 찍는 것을 구현해 보았다. 밑의 부분을 제외하고 delay와 main함수만 보게 되면 동기식처럼 보이게 된다.

밑에 async-await을 보게 되면 제너레이터로 구현한 것과 정말 비슷해 보인다. 정말 비슷하게 사용이 가능하긴 하지만 차이점이 존재한다. 
그 차이점은 async-await은 Promise에 최적화 되어 있다. 이말은 즉 Promise가 꼭 있어야 한다. 하지만 제너레이터는 Promise없어도 사용이 가능하다.

그리고 async-await도 제너레이터로 구현되어 있다.

## 마무리 🚀 
진도는 많이 나가지 않았지만 복습한다는 생각으로 배우는게 많아서 좋았고, 중간에 민태님이 사람들과 소통할 때 프로토콜이 맞아야 하고, 수준의 높낮이가 아닌 같은
선상에서 통해야지 말이 통하며, 가령 무언가 설명할 때나 들어줄 때 상대방이 알 수 있는 언어로 얘기를 해야지 서로간의 상호작용이 가능하다고 말씀해주셨고,
주니어 개발자들이 배워야 할 덕목 중 하나는 소통할 수 있는 언어로 말하기라고 하시면서 누군가를 가르치는게 좋은 공부방법이라고 말씀해주신게 기억에 남는다. 