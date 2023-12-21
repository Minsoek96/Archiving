# JSX

JSX는 XML와 유사한 자바스크립트 확장문법으로\
컴포넌트 UI를 생성하는데 도움을 준다.

* JSX는 XML처럼 작성된 부분을 `React.createElement`을 쓰는 JavaScript코드로 변환한다.
* 중괄호를 써서 JavaScript 코드를 그대로 쓸 수 있고, 결국은 JavaScript코드와 1:1로 매칭된다.

## SynctaticSugar

***

이해 하고 표현하기 쉽게 디자인된 프로그래밍 언어 문법

* 더 직관적이고 간결한 방식으로 코드를 작성할 수 있도록 돕는다.

## React.createElement

***

`createElement`를 사용하면 `React element`트리에 노드를 추가 할 수 있다.

```jsx
createElement(type, props, ...children);
```

### 특징

`JSX`는 `React.createElement()`함수에 대한 문법적 설탕이다.\
트랜스파일러를 이용하면 :\
`JSX` → `React.createElement()`함수로 변환된다.

### Parameters

**type**: `type`의 인자는 태그 이름 문자열(`div`, `span`)등 또는 React컴포넌트(함수, 클래스 또는 `Fragment`)\
**props**: `props`인자는 객체이거나 null이어야 한다.\
**...children?**: 0개 이상의 자식 노드

[Babel](https://babeljs.io/repl) 로 확인 가능

> → “Presets”에서 “react”를 체크하거나, “Plugins”에서 “@babel/plugin-transform-react-jsx”를 추가하면 JSX를 실험할 수 있다.

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

> 💡JSX 파일 /\_ @jsx 어쩌고 \_/ 주석을 추가하면 React.createElement 대신 “ 어쩌고”를 쓰게된다.

## React Element

***

React에서 UI를 구성하는 가장 기본적인 단위\
React 화면에 무엇이 표시될지 설명하는 객체

### 특이사항

**불변성** : 한번 생성된 element는 속성을 변경할 수 없다.\
→새로운 element를 생성하고 갱신 요청 해야함\
**가상DOM** : React의 요소는 실제 DOM요소가 아니라 가상DOM의 일부이다.

**생성방법** : `React.createElement()` , `JSX`

## jsx-runtime(New JSX Transform)

***

React17 이전 버전에서는 React 컴포넌트를 컴파일할 때 `React.createElement` 를 호출 해야하기 때문에 모든 JSX파일 상단에 import React from ‘react’ 구문이 필요했다.\
하지만 React17이후 React와 Babel이 `jsx-runtime` 이라는 새로운 패키지를 도입했다. JSX코드가 `jsx`또는 `jsxs` 함수를 사용하여 `React.createElement`의 호출 없이 직접 변환된다. 더이상 React를 명시적으로 불러올 필요가 없게 되었다.

> 모든 파일에 React를 임포트 할 필요 없어졌다.\
> 필요한 기능만 임포트하여 최종 번들의 크기가 줄어들었다.\
> 새로운 JSX변환은 런타임 성능을 조금이나마 향상시켰다.

## React StrictMode

***

리액트 내의 잠재적인 문제를 알아내기 위한 도구이다.\
`Fragement` 와 같이 UI를 렌더링하지 않으며, 자식들에 대한 부가적인 검사와 경고를 활성화한다.

> 💡Strict모드는 개발모드에서만 활성화 된다.\
> 에상치 못한 사이드 이펙트를 감지하기 위해 의도적으로 이중으로 호출한다.

## Virtual DOM(DOM복사본)

***

Virtual DOM은 UI를 DOM을 추상화한 javaScript객체를 메모리에 저장하고\
실제 DOM와 동기화하는 프로그래밍 개념

## DOM

***

`HTML`, `XML`문서의 프로그래밍 `interface`이다. DOM은 문서의 구조화된 표현을 제공하며 프로그래밍 언어가 DOM구조에 접근할 수 있는 방법을 제공하여 문서를 읽고, 변경할 수 있게 돕는다. DOM은 `nodes`와 `objet`로 문서를 표현한다.

## 브라우저 렌더링 과정

1. HTML문서를 파싱한다.
2. DOM트리를 생성한다.
3. CSSOM트리를 생성한다.
4. Render트리를 생성한다.
5. Layout 단계 ( 스타일 요소의 좌표를 설정하는 단계 )
6. Paint 단계 ( 스타일 요소를 그리는 단계 )
7. Compose (Paint를 쌓아 합치는 단계)

### 트리 자식 노드에 변화가 발생하는 경우

전체의 트리를 다시 그리는게 아니라 변화가 일어난 해당 노드와 관련된 트리 부분만 업데이트 한다.

1.  DOM조작 (DOM Manipulation)

    > DOM트리에 변화를 주거나 새 요소를 추가, 변경 등의 동작을 수행\
    > EX:) `document.createElement`, `appendChild`, `removeChild`, `innerHTML` 등
2.  DOM트리 업데이트 (Update DOM Tree)

    > 변경된 부분은 브라우저에서 감지된다.\
    > 변경 내용은 DOM트리에 즉시 반영되고, 트리의 구조가 변경된다.
3.  CSSOM 재계산 (Recalculate CSSOM)

    > 변경된 DOM에 대한 CSSOM을 다시 계산한다.\
    > 스타일 규칙이 재평가되고 스타일 정보가 업데이트 된다.
4.  레이아웃 재계산 (Reflow)

    > DOM의 변경으로 인해 레이아웃에 영향을 미칠 수 있는 경우, 레이아웃 단계가 재실행된다.\
    > \->DOM 요소의 크기 또는 위치 변경될 때 발생
5.  페인팅 재수행 (Repaint)

    > 변경된 요소와 그 주변의 요소들은 새로 그려져야 한다.\
    > \->색상, 배경, 그림자 및 투명도 같은 스타일 속성만 변경하면 Repaint 단계만 발생
6.  합성 (Compositing)

    > 필요한 경우 발생 한다.

## VDOM와 DOM의 차이점

***

DOM은 웹페이지의 실제 구조를 나타내지만 VDOM은 메모리상 DOM의 경량화된 표현 이다.

DOM은 업데이트가 발생할 때마다 실제 DOM에 반영하지만, `React는 배치 업데이트 방식`으로 처리하기 위해 상태변화를 비동기 처리하고, 여러 변경 사항을 모아 단일 업데이트로 처리한다. 이후 가상 DOM트리가 생성되고 이전의 DOM트리와 비교하여 변경된 부분만 실제 DOM에 동기화되어 `Reflow`와`Repaint`과정을 최소화할 수 있다.

> 💡리액트는 돔보다 빠른게 중요한게 아니라 유지보수가 좋은 앱이지만 충분히 빨라

## Reconciliation(재조정) 과정 ???

***

어떤 부분을 새롭게 렌더링해야 하는지 가상 DOM과 실제 DOM을 비교하는 작업\
`render()` 함수는 `React Eelement Tree`를 만드는 것이다.

> 원칙 1. 서로 다른 타입의 두 엘리먼트는 서로 다른 트리를 만들어낸다.\
> 원칙 2. 개발자가 key prop을 통해, 여러 렌더링 사이에서 어떤 자식 엘리먼트가 변경 되지 않아야 할지 표시해 줄 수 있다.

### 동작과정

> 1. state나 props가 변경됨을 감지
> 2. render() 함수가 호출이 된다.
> 3. 가상DOM트리를 생성
> 4. 이전 DOM와 가상 DOM을 비교(diffing Algorithm)
> 5. 변경이 확인된 부분을 실제 DOM와 동기화

**Batch Update**가 핵심\
실제DOM을 여러번 반복하여 조작하는게 아니라 변경된 내용을 한번에 반영한다.



***

> [Understanding Reflow and Repaint in the browser](https://dev.to/gopal1996/understanding-reflow-and-repaint-in-the-browser-1jbg)
>
> [React 공식문서의 JSX 소개](https://ko.reactjs.org/docs/introducing-jsx.html)
>
> [JSX 이해하기](https://ko.reactjs.org/docs/jsx-in-depth.html)
>
> [JSX 없이 사용하는 Reac](https://ko.reactjs.org/docs/react-without-jsx.html)[t](https://ko.reactjs.org/docs/react-without-jsx.html)
>
> [VDOM (Virtual DOM)](https://ko.reactjs.org/docs/faq-internals.html)
>
> [재조정 (Reconciliation)](https://ko.reactjs.org/docs/reconciliation.html)\
> [Strict Mode](https://ko.reactjs.org/docs/strict-mode.html)
