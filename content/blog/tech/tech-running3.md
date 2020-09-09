---
title: 우아한 테크러닝 3회차
date: 2020-09-09 14:00:00
category: 우아한테크러닝
thumbnail: './images/tech_running.png'
draft: false
---

![tech](./images/tech_running.png)

> 우아한 테크러닝 3기 React&TypeScript 3회차

오늘은 어제 수강한 우아한 테크러닝 3기 React&TypeScript 3회차에 대한 정리 해보려 한다.

## 3회차에 배운 내용

3회차에서는 본 수업목표인 React를 직접 만들어보고, React는 어떤식으로 생겼으며 왜 그렇게 만드는지 이해해보는 시간을 가졌다.
그리고 마지막으로 짧지만 React의 컴포넌트와 상태관리에 대해서 배우게 되었다.

## React를 만들어 보는 이유

강의에서 저번 회차에서는 Redux를 만들어보고, 이번 회차에서는 React를 만들어보게 되었는데 이렇게 직접 만들어 보는 이유는
개발자들이 무장정 사용하지만 말고 이해하기를 바래서라고 하셨다.

무작정 사용한 개발자와 이해하면서 사용하는 개발자의 깊이는 계속 차이가 날 것이고 연봉차이도 날거라고 하셨다.😂
이번에는 React를 개발한 사람들이 무슨생각으로 만들었지 확인해가면서 직접 React를 만들어보는 시간을 가졌다.

## React를 사용하지 않았을 때 화면에 그리기

```javascript
const list = [
  { title: 'React에 대해 알아봅시다.' },
  { title: 'Redux에 대해 알아봅시다.' },
  { title: 'Typescript에 대해 알아봅시다.' },
];

const rootElement = document.getElementById("root");

function App(items){
  rootElement.innerHTML = `
    <ul>
      ${items.map((item) => `<li>${item.title}</li>`).join("")}
    </ul>
  `;
}

App(list);
```

이 코드에서는 rootElement의 innerHTML을 이용해서 Element를 생성해 직접 넣어주었다.
App함수는 items를 받아 리스트를 만들어주는 순수함수로 만들었다.

하지만 위에 App함수는 innerHTML을 이용해서 DOM에 직접 접근해서 구조를 변경하고 있다. 
이런식으로 DOM에 직접 접근하는 것은 규모가 커지게 되면 리팩토링이 힘들며 성능저하의 문제가 있으므로 사용하지 않는 것이 좋다. 

이런 이유로 `Virtual DOM` 형태의 프레임워크들이 개발되었고 그중 하나가 이제 배울 `React`이다.

## React와 Virtual DOM 이해
간단하게 React 코드를 짜보도록 하겠다.
```jsx
import React from "react";
import ReactDOM from "react-dom";

function App(){
  return(
    <div>
      <h1>Hello?</h1>
      <ul>
        <li>React</li>
        <li>Redux</li>
        <li>MobX</li>
        <li>TypeScript</li>
      </ul>
    </div>
  )
}

ReactDOM.render(<App/>, document.getElementById("root"));
```
App 함수의 반환값은 JSX이며 HTML과 비슷하게 컴포넌트를 만들었다.
1회차 수업에서 진행했듯 ReacDOM은 render라는 정적메서드를 가지며 2개의 인자를 받는데, 첫번째는 렌더링할 컴포넌트를 받게되고 두번째는 렌더링할 요소를 받는다.
"root"가 아닌 다른 것으로 설정해도 똑같이 렌더링 된다.

위 코드는 현재 App이라는 컴포넌트 하나기 때문에 App이 무슨의미지? 라는 생각이 든다. 코드를 한번 바꿔보겠다.

```jsx
import React from "react";
import ReactDOM from "react-dom";

function StudyList(){
  return (
    <ul>
        <li>React</li>
        <li>Redux</li>
        <li>MobX</li>
        <li>TypeScript</li>
    </ul>
  )
}

function App(){
  return(
    <div>
      <h1>Hello?</h1>
      <StudyList />
    </div>
  )
}

ReactDOM.render(<App/>, document.getElementById("root"));
```
App안에서 리스트를 만들어주는 것을 StudyList라는 함수형 컴포넌트를 만들어 분리했다. 
리액트의 장점 중 하나로 코드를 사용성에 따라서 분리할 수 있으며 컴포넌트의 이름을 줄 수 있다.
이렇게 이름을 줄 수 있는 것은 정말 강력하고 좋은 기능이라고 하셨다. 이름을 줌으로 인해 가독성이 높아지고, 
결국 가독성이 높아지기 때문에 나중에 코드를 수정하거나, 관리할 때 편하다고 하셨다.

그리고 HTML에서는 태그에 class 속성을 사용하지만 React는 class대신에 className을 사용한다. 
이 이유는 class는 javascript의 키워드이고 JSX는 javascript의 확장이기 때문이다. 
javascript의 예약어 "class"와 구별되게 만들려고 className으로 사용하는 것이다.

```jsx
function StudyList(){
  return (
    <ul>
        <li className="studyItem">React</li>
        <li className="studyItem">Redux</li>
        <li className="studyItem">MobX</li>
        <li className="studyItem">TypeScript</li>
    </ul>
  )
}
```
위의 JSX 엘리먼트들을 객체로 바꾸어보자.

```jsx
const vdom = {
  type: "ul",
  props: {},
  children: [
      { type: "li", props: { className: "item" }, children: "React" },
      { type: "li", props: { className: "item" }, children: "Redux" },
      { type: "li", props: { className: "item" }, children: "Typescript" },
      { type: "li", props: { className: "item" }, children: "MobX" },
  ]
}
```
아마도 이런식으로 생겼을 것이다. 한번 속성을 넘기면 어떻게 되는지 유추해보자.(props)

```jsx
import React from "react";
import ReactDOM from "react-dom";

const vdom = {
  type: "ul",
  props: { item: "item", id: "study list"},
  children: [
      { type: "li", props: { className: "item" }, children: "React" },
      { type: "li", props: { className: "item" }, children: "Redux" },
      { type: "li", props: { className: "item" }, children: "Typescript" },
      { type: "li", props: { className: "item" }, children: "MobX" },
  ]
}

function StudyList(){
  return (
    <ul>
        <li>React</li>
        <li>Redux</li>
        <li>MobX</li>
        <li>TypeScript</li>
    </ul>
  )
}

function App(){
  return(
    <div>
      <h1>Hello?</h1>
      <StudyList item="item" id="study list"/>
    </div>
  )
}

ReactDOM.render(<App/>, document.getElementById("root"));
```
여기서 속성(props)를 넘겨주는 것은 객체 형태로 밖에 넘어갈 수 없다. 이유는 객체형태가 아니라면 컴포넌트간 값을 넘기기란 불가능하기 때문이다.

밑에서 만든 코드에 따라 vdom즉 VirtualDOM은 저런 모양일 것이다. 저러한 형태로 생성된 객체를 render메소드가 만나 HTML태그로 변경시켜서 
화면에 그려주는 방식인 것이다. 결국 하나의 부모에서 트리형태로 자식을 가지는 것이다.

## React를 직접 만들어보기 
먼저 React로 구현되어 있는 createElement는 어떻게 생겼는지 확인해보자
```jsx
function createElement(type, props ={}, ...children){
  return { type, props, children};
}
```
이런식으로 함수가 구성되었다. type은 위에서 봤듯이 태그의 타입, props는 컴포넌트의 속성, children은 가변인자로 자식들이라고 보면 된다.

createElement를 사용하면 위에서 만들었던 vdom형태가 생성되는 것이라고 보면 된다.

### 1. createElement만들어보기 
```jsx
/* @jsx createElement */
function createElement(type, props={}, ...children){
  return {type, props, children};
}

const App = () => <div>App!</div>

console.log(App);
```
위에 @jsx createElement는 babel로 트랜스파일링 되는 vdom의 역할을 줄 수 있는 것이다. 
이런식으로 하면 됬을까? 안될 것이다. 왜냐하면 App은 함수이기 때문이다. 안에 반환되는 것이 있긴하지만 App자체가 함수이기 때문에
type자체가 function이기 때문이다. 이를 해결하기 위해 방어코드를 만들어 주어야 한다.

```jsx
function createElement(type, props={}, ...children){
  if(typeof type === "function"){
    return type.apply(null, [props, ...children]);
  }
  return {type, props, children};
}

const App = () => <div>App!</div>

console.log(App);
```
`typeof`를 이용해서 type이 function일 경우에는 apply을 이요해 기존의 매개변수를 그대로 넣어 호출해 준 것이다. 이런식으로 만들어주게 되면
위에서 만든 vdom과 같은 형태가 출력되는 것을 확인할 수 있을 것이다. 

그럼 이제 Vdom을 만들어 줬으니 이를 그려줘야 할 것이다. 그려주는 함수를 직접 구현해보자.

### 2. render
```jsx
function renderElement(node){
  if(typeof node === "string"){
    return document.createTextNode(node);
  }
  const item = document.createElement(node.type);
  
  node.children.map((renderElement).forEach((element) => {
    item.appendChild(element);
  }));
  
  return container;
}

function render(vdom, container){
  container.appendChild(renderElement(vdom));
}
```
이런식으로 render부분을 구현해보았다. renderElement에서는 먼저 node의 type이 string인지 확인한다. 
이는 자식으로 내려가다 마지막에 문자열일 경우에는 createTextNode로 그려주도록 한 것이다.

그런 이후 재귀 함수를 이용해 node의 타입이 문자열이 될 때까지 반복해 생성하게 한다. 

이런식으로 React를 만들어보았다. 구현하고 나니 정말 쉽다! props를 넘겨주고 그려주는 것도 동일하게 작동한다.

실제 React의 경우에는 VirtualDOM과 비교하는 로직이 추가되어 있을 것이다. 민태님이 올려주신 정리된 완성본을 보도록 하자!

`App.js`
```js
/* @jsx createElement */
import { createElement, render } from "./tiny-react";

function Hello(props) {
  return <li className="item">{props.label}</li>;
}

function App() {
  return (
    <div>
      <h1>hello world</h1>
      <ul className="board" onClick={() => null}>
        <Hello label="Hello" />
        <Hello label="World" />
        <Hello label="React" />
      </ul>
    </div>
  );
}

render(<App />, document.getElementById("root"));
```
`tiny-react.js`
```js
function renderElement(node) {
  if (typeof node === "string") {
    return document.createTextNode(node);
  }

  if (node === undefined) return;

  const $el = document.createElement(node.type);

  node.children.map(renderElement).forEach((node) => {
    $el.appendChild(node);
  });

  return $el;
}

export function render(vdom, container) {
  container.appendChild(renderElement(vdom));
}

export function createElement(type, props, ...children) {
  if (typeof type === "function") {
    return type.apply(null, [props, ...children]);
  }
  return { type, props, children };
}
```

## React의 컴포넌트와 상태관리
React에서는 함수로 컴포넌트로 개발하는 방법과 클래스형태로 컴포넌트를 개발하는 방법이 있다.
먼저 나온 클래스형 컴포넌트부터 보자.
### 클래스형 컴포넌트
클래스형 컴포넌트에는 라이프사이클 API가 존재하며 상태를 생성자에서 초기화해서 사용할 수 있다.
```jsx
import React from "react";

class Hello extends React.Component {
    constructor(props) {
        super(props);

        this.state = {
            count: 1,
        };
    }

    componentDidMount() {
        this.setState({ count: this.state.count + 1 });
    }

    render() {
        return <p>안녕하세요!</p>;
    }
}
```
상태를 변경할 때는 this.state.count = 1 이런식으로 하면 안되고 React.component에 존재하는 setState를 사용해 바꾸어야 한다.
클래스형 컴포넌트의 상태는 객체의 생성자에 의해 초기화 되기 때문에 React가 지우지 않는 이상 상태가 유지된다.

### 함수형 컴포넌트 
원래 예전의 함수형 컴포넌트를 상태를 갖지 못하는 컴포넌트로 불려졌다. 함수안에 변수를 만들게 되면 함수가 호출될 때마다 변수가 초기화 되기 때문에
값이 유지될 수 없었기 때문이다. 

이에 함수형 컴포넌트에서는 `Hooks`가 등장하게 되면서 상태를 관리할 수 있게 되었다. class형 컴포넌트에서는 setState를 사용하지만 
함수형에서는 useState함수를 사용해 상태를 생성하고 관리한다.
```jsx
import React, { useState } from "react";

function App() {
    const [count, setCount] = useState(1);

    return (
        <div>
            <h1 onClick={() => setCount(count+1)}>상태 {count}</h1>
        </div>
    );
}
```
useState반환값은 첫번째는 상태값이며 두번째는 dispatcher로 사용되는 함수로 상태를 바꿀 수 있는 함수이다.

React에서는 useState로 만든 dispatcher함수를 호출하면 React에 알리게 된다.

Hooks는 전역 배열로 관리되며 생성되는 순서에 따라 컴포넌트를 key로 하여 인덱스로 관리한다. useState를 사용하는
컴포넌트가 관련 정보가 없을 경우에는 초기값을 저장하게 되고 관련정보가 있을 경우에는 기존의 상태를 반환해 사용하는 것이다.

Hooks를 사용할 때에는 리액트 공식문서에 있는 Hooks사용규칙을 보고 사용해야 한다. 규칙을 지키지 않을 경우 문제가 생길 수 있다.

## 마무리 🚀 
리액트를 현재 열심히 사용중이고 애용중이지만 React를 간단하게 만들어보긴 처음이라 정말
재밌고 흥미로웠던 회차인것 같다.