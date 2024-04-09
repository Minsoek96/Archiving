# ForwardRef

> 부모컴포넌트로부터 자식 컴포넌트에 `ref`를 전달하기 위해 사용되는 기능이다.

기본적으로`ref`는 `props`로 전달되지 않기 때문에, 직접적으로 자식 컴포넌트로 `ref`를 내리거나 조회하는 것이 불가능하다.

`forwardRef`를 사용하면 이러한 제약을 우회할수 있다.

## 사용예시 

```jsx
import React, { forwardRef } from 'react';

// FancyInput 컴포넌트는 forwardRef를 사용하여 부모 컴포넌트로부터 ref를 전달받습니다.
const FancyInput = forwardRef((props, ref) => {
  return <input ref={ref} type="text" {...props} />;
});

function ParentComponent() {
  // 부모 컴포넌트에서 ref를 생성합니다.
  const inputRef = React.useRef();

  // 마운트된 후 input 요소에 포커스를 줍니다.
  React.useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <div>
      <FancyInput ref={inputRef} placeholder="여기에 입력하세요" />
    </div>
  );
}

export default ParentComponent;

```