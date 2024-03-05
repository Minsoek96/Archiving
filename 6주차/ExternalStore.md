# ExternalStore

## 관심사 분리(separation of concerns, SOC)

---
> 책임과 역할에 따라 명확하게 코드를 분리하는 디자인 원칙  

각각의 코드가 특정한 책임이나 역할을 가짐으로써, 전체 시스템의 코드의 가독성와 유지보수의 편의성을 높이기 위한 목적으로 사용된다.  

관심사를 적절하게 분리한다면 코드는 서로 독립적으로 동작할 수 있다.

## Layered Architecture  

---
> 각 계층 마다 특정 역할과 관심사를 가지는 구조이다.  
구성되는 숫자에 따라 N계층 아키텍처 (N-tier Architecture) 라고도 한다.

**특정 역할과 관심사?**: ( 화면 표시, 비즈니스 로직 수행, DB 작업 등) 별로 구분된다.  
**관심사의 분리가 주요 특징이다.** 

사람 —> 스위치(UI) —> 모터 —> 바퀴  
사용자에게 가까운 것과 사용자에게서 먼 것으로 구분한다.

원칙적으로 정해져 있지는 않지만 일반적으로 3~4개의 계층으로 나뉘어 진다.  

### Presentation Layer

화면에 정보를 표시하는 것을 주관심사로 두는 계층입니다.  
사용자 인터페이스(UI)와 사용자 경험(UX)를 처리한다.  
사용자의 입력을 받아들이고, 데이터를 표시하는 역할을 한다.  
비즈니스 로직이 어떻게 수행 되는지 관심이 없다.

### Business Layer  

애플리케이션의 핵심 기능과 비즈니스 규칙을 담당하는 계층  
Persistence Layer에서 데이터를 가져와 비즈니스 로직을 수행하고 그 결과를  
Persistence Layer로 전달한다.  
사용자의 요구 사항을 처리하고, 데이터의 처리와 변환을 담당한다.

### Persistence Layer  

데이터 출처와 그 데이터를 가져오고 다루는 것을 주 관심사로 둔다.  

### Database Layer  

MySQL, MariaDB, PostgreSQL, MongoDB 등 데이터베이스가 위치한 계층을 의미한다.  

## Flux Architecture  

---
>React와 함께 사용되며 Facebook에서 MVC의 2-way-binding의  
패턴의 복잡성와 관리 문제를 해결하기 위해  
데이터 흐름을 단방향으로 관리하는 것을 목적으로 제작된 아키텍처

Dispatcher, Stores, Views, Action으로 구성되어 있다.

### Action  

상태 변경 요청을 담은 자바스크립트 객체  
Dispatcher에게 전달된다.  
이벤트/ 메시지 같은 객체  

### Dispatcher  

모든 데이터의 흐름을 관리한다.  
여러 Store에 Action을 전달하는 역할  
내부에 상태 변경 로직이 존재하지 않는다   
메시지 브로커와 유사하다.  

### Stores  

애플리케이션의 상태를 관리한다.  
Dispatchher에서 전달된 Action을 통해서만 상태 변경  
받은 Action에 따라 상태를 변경, 상태 변경을 알림  

### Views  

상태에 따라 화면을 출력하는 로봇  
Controller-View(React)  
-> Store가 통지하는 상태 변경을 수신,  
-> 받은 상태에 따라 View를 새로 렌더링  
Dispatcher에게 Action전달  

## useReducer  

---
> useReducer는 컴포넌트에 reducer를 추가할 수 있는 Hook  

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```

### Reducer

Reducer 함수는 상태(state)와 어떤 변화를 일으킬 `action`을 인자로 받아,  
새로운 상태를 반환한다.   
해당 함수는 순수함수여야 하며, 같은 인자가 주어졌을 때 항상 같은 결과가 반환해야 한다.  
`reduce()` 연산에서 영감을 얻었다.  

`state` 현재의 상태를 반환한다.  
`dsipatch` 함수를 사용하여 Reducer함수에 인자를 전달할 수 있다.  
`initalArg` 초기 상태 

```jsx 
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}
-------
const [state, dispatch] = useReducer(reducer, {count: 0});
-------
<button onClick={() => dispatch({type: 'increment'})}>증가</button>
<button onClick={() => dispatch({type: 'decrement'})}>감소</button>

```

액션을 통해서 state를 업데이트 하여 상태가 예측이 가능하고,  
복잡한 상태 로직을 쉽게 관리할 수 있다.

## useCallback  

---
> 리렌더링 사이에서 함수 정의를 캐시할 수 있게 해주는 React훅

```jsx
const cachedFn = useCallback(fn, dependencies)
```

`fn` : 캐시하려는 함수  
-> 마지막 렌더링 이후 `depdencies`가 변경되지 않았다면 동일한 함수를 다시 제공한다.  
 
`dependencies` : 의존성 배열  
-> React는 `object.io` 비교 알고리즘을 사용하여 각 의존성을 이전 값과 비교한다.
