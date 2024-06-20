# Suspense

>Suspense는 React 컴포넌트가 비동기 작업을 수행하는 동안 로딩 상태를 처리하는 데 
사용된다.   
컴포넌트가 비동기 작업을 완료할 때까지 대체 UI(fallback)을 보여준다.

- children : `Suspense` 컴포넌트의 자식으로 렌더링하려는 실제 UI이다.
- fallback : 로딩이 완료되지 않았을 때 보여줄 대체 UI (로딩 스피너, 스켈레톤)

## 주의사항 

### State 보존 :

React는 처음 마운트되기 전에 일시 중단된 렌더링의 state를 보존하지 않는다. 컴포넌트가 로드되면 React는 일시 중단된 트리를 처음부터 다시 렌더링한다.


```jsx
<Suspense fallback={<LoadingSpinner />}>
  <ComponentThatSuspends />
</Suspense>
```

### Suspense와 상태관리 

Suspense가 트리의 콘텐츠를 표시하다가 다시 일시 중단되면, 그 원인이 startTransition이나 useDeferredValue로 인한 것이 아니라면 fallback이 다시 표시된다.

```jsx
import { useDeferredValue } from 'react';

function MyComponent({ value }) {
  const deferredValue = useDeferredValue(value);
  // deferredValue 사용
}
```