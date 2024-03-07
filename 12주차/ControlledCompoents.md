# 제어, 비제어 컴포넌트

> 제어 컴포넌트(Controlled Components)와 비제어 컴포넌트(UncontrolledComponents)는  
주로 React에서 폼 요소를 다룰 때 사용되는 개념이다.  
---

## 제어 컴포넌트 (Controlled Compoents)

제어 컴포넌트에서는 React 컴포넌트가 폼 데이터의 상태를 관리한다.  
사용자가 입력을 변경할 때마다 상태 업데이트 함수를 호출하여 React상태를 업데이트 한다.  
해당 방식은 React의 상태를 컴포넌트가 가지게 된다. 즉, 입력 값을 변경 하기 위해서는  
React의 상태를 해야 한다는 것을 의미한다.  

**장점** : 상태를 직접 관리 하기 때문에 유효성 검사, 입력 값 저장, 조건부 필드 업데이트 등을 쉽게 구현할 수 있다.  

**단점** : 가독성이 떨어지고 코드의 관리가 힘들어 질 수 있다.  

```jsx
import React, { useState } from 'react';

function ControlledComponent() {
  const [value, setValue] = useState('');

  function handleChange(event) {
    setValue(event.target.value);
  }

  function handleSubmit(event) {
    alert('A name was submitted: ' + value);
    event.preventDefault();
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Name:
        <input type="text" value={value} onChange={handleChange} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}

export default ControlledComponent;

```  

---

## 비제어 컴포넌트 (Uncontrolled Compoents) 

비제어 컴포넌트에서는 폼 데이터의 상태를 DOM자체가 관리한다.  
React 컴포넌트는 폼 데이터를 직접적으로 관리하지 않고, 대신 `ref`를 사용하여 폼 입력  
요소에 접근하여 값을 가져오거나 설정할 수 있다. 이 방식에서는 React상태를 관리하지 않고 사용자 입력을 처리할 수 있다.

장점 : 가독성이 좋아지고, 폼을 빠르게 구현할 수 있다.

단점 : 폼 데이터의 상태를 직접적으로 제어하기 어려워, 유효성 검사나 사용자 입력의 사후 처리가 복잡해질 수 있다. 또한 React의 선언적인 프로그래밍 패러다임과는 거리가 멀다.

```jsx
import React, { useRef } from 'react';

function UncontrolledComponent() {
  const inputRef = useRef();

  function handleSubmit(event) {
    alert('A name was submitted: ' + inputRef.current.value);
    event.preventDefault();
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Name:
        <input type="text" ref={inputRef} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}

export default UncontrolledComponent;

```

## 언제 제어, 비제어?

제어 컴포넌트를 사용해야할 때:

- 폼 데이터를 다른 UI요소와 동기화해야 할 때
- 폼 제출 시 폼 데이터의 검증이 필요할 때 
- 사용자 입력을 기반으로 동적인 폼 요소를 생성하거나 제어해야 할 때 

비제어 컴포넌트를 사용해야 할 때:

- React외부에서 폼 데이터를 관리해야 할 때(예: 직접 DOM을 사용해야 할 때)
- 폼 입력이 매우 단순하거나 한 번만 사용될 때
- 파일 입력처럼 제어 컴포넌트로 처리하기 어려운 폼 요소를 다룰 때 

> 제어 컴포넌트는 복잡한 로직을 요구하는 만큼 엄격한 규칙을 세워 필터링을 할 수 있는 반면 컴포넌트의 상태를 직접 제어 하기때문에 리렌더링 자주 발생하는 치명적인 단점이 존재한다. 반면 비제어 컴포넌트는 폼의 상태를 외부(DOM)으로 부터 반영받아 간단하게 폼처리를 할 수 있고 상태의 제어는 외부에 있기에 리렌더링이 발생하지 않는다. 