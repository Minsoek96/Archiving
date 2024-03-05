# Redux

## Redux란?

---
> 자바스크립트 상태관리 라이브러리 

Redux는 Flux 아키텍처의 영감을 받아 `Store`, `Action`, `Reducer`라는 3가지의 특징을 가지고 있다. 하지만 기존의 Flux Architecture와 다르게 store를 싱글턴 패턴으로 디자인하여 단일 진실 공급원으로 사용된다는 점이다.  

구조는 좀더 단순화 되었고 하나의 스토어에서 상태를 관리하여 더욱 효율적이다.  

리액트의 PropsDrilling 문제를 해소하기 위한 도구 중 하나로 사용된다. 

**Store**: 애플리케이션의 상태를 저장하는 곳으로 Redux에서는 단 하나만 존재한다.  
**Action**: 상태를 변경하려는 의도를 설명하는 객체  
**Reducer**: 액션을 받아 이전 상태를 새로운 상태로 변환하는 순수 함수이다.  

### Redux Middleware  

또 하나의 핵심 기능으로는 미들웨어가 있다.  
액션과 리듀서 사이에서 작동한다. 미들웨어는 액션을 가로채고, 필요에 따라 액션을 수정하거나, 무시하고, 새로운 액션을 조작한다.  
단연 최대 장점은 미들웨어를 통해 비동기 작업을 수행할 수 있다는 점이다. 

`Redux DevTool` 이라는 도구를 이용하여 상태변화를 시각적으로 추적하고 디버깅하는데 매우 유용하다.  

```jsx
// 커스텀 미들웨어 함수 작성
const customMiddleware = store => next => action => {
  // action을 전달하기 전에 원하는 작업을 수행할 수 있다.
  console.log('액션 타입:', action.type);
  
  // 다음 미들웨어로 액션을 전달한다.
  const result = next(action);

  // 액션이 처리된 후에 추가적인 작업을 수행할 수 있다.
  console.log('다음 상태:', store.getState());

  return result;
};

// 미들웨어를 적용하여 스토어 생성
const store = createStore(
  rootReducer,
  applyMiddleware(customMiddleware)
);
```

### 단점

리덕스의 단점은 보일러플레이트 코드의 복잡성와 비동기 작업 처리의 불편함이다.  
최근에는 보일러 플레이트의 문제를 해결하기 위해 Redux-Toolkit이 출시되었다.  
비동기 작업의 해결은 미들웨어를 통해 해결 할 수 있지만 여전히 불편하다.  

## Reflect 

---  

> ES6에서 추가된 내장 객체이며, 주로 메타 프로그래밍에 사용된다.  

메서드의 종류는 프록시 처리기와 동일하여 객체의 기본 작업을 가로채고 조작할 수 있는 기능을 제공한다.  

### 특징  

- Reflect는 함수객체가 아니므로 생성자로 사용할 수 없다.
- Reflect는 정적 메서드만을 제공하는 객체이다.  
- Reflect 메서드는 일관된 반환값을 제공한다.  
-> 성공시 true, 실패시 false를 반환한다는 의미  

Reflect 메서드들은 첫 번째 인자로 대상 객체를 받는다.  
Reflect API는 Proxy객체와 밀접하게 연관되어 Proxy를 사용할 때 Reflect 메서드를 활용하면 좀더 쉽게 사용할 수 있다.  

### 문법 

- **`Reflect.apply()`**: 함수를 주어진 인자와 함께 호출한다.
- **`Reflect.construct()`**: 새 객체를 생성자로서 호출한다.
- **`Reflect.get()`**: 대상 객체의 속성을 가져온다.
- **`Reflect.set()`**: 대상 객체의 속성을 설정한다.
- **`Reflect.defineProperty()`**: 객체에 새 속성을 정의하거나 수정한다.
- **`Reflect.deleteProperty()`**: 객체의 속성을 삭제한다.
- **`Reflect.has()`**: 객체가 특정 속성을 가지고 있는지 여부를 확인한다.