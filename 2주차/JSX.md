# JSX

JSX는 XML와 유사한 자바스크립트 확장문법으로  
컴포넌트 UI를 생성하는데 도움을 준다.

- JSX는 XML처럼 작성된 부분을 `React.createElement`을 쓰는 JavaScript코드로 변환한다.
- 중괄호를 써서 JavaScript 코드를 그대로 쓸 수 있고, 결국은 JavaScript코드와 1:1로 매칭된다.

## SynctaticSugar

이해 하고 표현하기 쉽게 디자인된 프로그래밍 언어 문법

- 더 직관적이고 간결한 방식으로 코드를 작성할 수 있도록 돕는다.

## React.createElement

```jsx
createElement(type, props, ...children);
```

`createElement`를 사용하면 `React element`트리에 노드를 추가 할 수 있다.
`JSX`는 `React.createElement()`함수에 대한 문법적 설탕이다.

트랜스파일러를 이용하면 :  
`JSX` → `React.createElement()`함수로 변환된다.

[Babel](https://babeljs.io/repl) 로 확인 가능

>→ “Presets”에서 “react”를 체크하거나, “Plugins”에서 “@babel/plugin-transform-react-jsx”를 추가하면 JSX를 실험할 수 있다.

JSX

```jsx
<div>test</div>
```

변환된 JS코드

```jsx
React.createElement("div", null, "test");
```

JSX

```jsx
<div type="test">test</div>
```

변환된 JS코드

```jsx
React.createElement("div", { type: "test" }, "test");
```

JSX

```jsx
<div>
  <p>test</p>
</div>
```

변환된 JS코드

```jsx
React.createElement("div", null, React.createElement("p", null, "test"));
```

JSX

```jsx
<div>
  <p>test</p>
  <p>test2</p>
</div>
```

변환된 JS코드

```jsx
React.createElement(
  "div",
  null,
  React.createElement("p", null, "test"),
  React.createElement("p", null, "test2")
);
```

JSX

```jsx
const App = (pageName) => <div>{pageName}</div>;
<App pageName={"App"} />;
```

변환된 JS코드

```jsx
const App = (pageName) => React.createElement("div",null,pageName)
React.createElement(App,{pageName="App"});
```

>💡JSX 파일 /_ @jsx 어쩌고 _/ 주석을 추가하면 React.createElement 대신 “ 어쩌고”를 쓰게된다.

```jsx

```

## React Element

React에서 UI를 구성하는 가장 기본적인 단위  
React 화면에 무엇이 표시될지 설명하는 객체

**특징:**  
불변성 : 한번 생성된 element는 속성을 변경할 수 없다.  
→새로운 element를 생성하고 갱신 요청 해야함

가상DOM : React의 요소는 실제 DOM요소가 아니라 가상DOM의 일부이다.

**생성방법** : `React.createElement()` , `JSX`

## React StrictMode

리액트 내의 잠재적인 문제를 알아내기 위한 도구이다.  
`Fragement` 와 같이 UI를 렌더링하지 않으며, 자식들에 대한 부가적인 검사와 경고를 활성화한다.

>💡Strict모드는 개발모드에서만 활성화 된다.

### Virtual DOM

리액트는 돔보다 빠른게 중요한게 아니라 유지보수가 좋은 앱이지만 충분히 빨라

## Reconciliation(재조정) 과정 ???
