# PublicAPI

## Public API의 요구사항

1.  접근 제어

    - **잘못된 경우:** 모듈의 내부 부분에 직접 접근하여 공용 접근 인터페이스를 우회하는 경우 - 이는 모듈을 리팩토링할 때 특히 위험하다.
      - `import { Form } from "features/auth-form/components/view/form"`
    - **올바른 경우:** API는 미리 필요한 것과 허용된 것만을 내보낸다. 이제 모듈 개발자는 리팩토링할 때 Public API를 깨뜨리지 않도록 생각하기만 하면 된다.
      - `import { AuthForm } from "features/auth-form"`

2.  변경에 대한 지속 가능성

    - **잘못된 예:** 이 컴포넌트를 기능 내부에서 이동하거나 이름을 변경하면 해당 컴포넌트를 사용하는 모든 위치에서 import를 리팩토링해야 한다.
      - `import { Form } from "features/auth-form/ui/form"`
    - **올바른 예:** 기능의 인터페이스가 내부 구조를 노출하지 않기 때문에, 기능의 외부 "사용자"는 내부에서 컴포넌트를 이동하거나 이름을 변경해도 영향을 받지 않는다.
      - `import { AuthForm } from "features/auth-form"`

3.  통합성

    - 잘못된 예: 이름 충돌이 발생한다.

      ```tsx
      export { Form } from "./ui";
      export _ as model from "./model";
      export { Form } from "./ui";
      export _ as model from "./model";
      // 사용 시
      import { Form, model } from "features/auth-form";
      import { Form, model } from "features/post-form";
      ```

    - 올바른 예: 인터페이스 수준에서 충돌이 해결된다.

      ```tsx
      export { Form as AuthForm } from "./ui";
      export * as authFormModel from "./model";
      export { Form as PostForm } from "./ui";
      export * as postFormModel from "./model";
      // 사용 시
      import { AuthForm, authFormModel } from "features/auth-form";
      import { PostForm, postFormModel } from "features/post-form";
      ```

4. 유연한 사용

    - 잘못된 예: 작성하기 불편하고 읽기 불편하며, 기능의 "사용자"가 불편을 겪는다.

      ```tsx
        import { storeActionUpdateUserDetails } from "features/auth-form"
        dispatch(storeActionUpdateUserDetails(...))
      ```

    - 올바른 예: 기능의 "사용자"는 필요한 것에 접근할 때 반복적이고 유연하게 접근할 수 있다.

      ```tsx
        import { authFormModel } from "features/auth-form"
        dispatch(authFormModel.effects.updateUserDetails(...)) // redux
        authFormModel.updateUserDetailsFx(...) // effector
      ```

4. 충돌 해결
    
    - 이름 충돌은 구현 수준이 아니라 `Public` 인터페이스 수준에서 해결되어야 한다.
    - 잘못된 예: 이름 충돌이 구현 수준에서 해결된다.

      ```tsx
        export { AuthForm } from "./ui"
        export { authFormActions, authFormReducer } from "model"
        ---
        export { PostForm } from "./ui"
        export { postFormActions, postFormReducer } from "model"
      ```

    - 올바른 예: 이름 충돌은 인터페이스 수준에서 해결된다.

      ```tsx
        export { actions, reducer }
        export { Form as AuthForm } from "./ui"
        export * as authFormModel from "./model"
        ---
        export { actions, reducer }
        export { Form as PostForm } from "./ui"
        export * as postFormModel from "./model"
      ```

##  모듈의 re-exports

JavaScript에서 모듈의 Public 인터페이스는 index 파일에서 모듈 내부의 엔티티를 re-export 하여 생성된다

```tsx
export { Form as AuthForm } from "./ui"
export * as authModel from "./model"
```

### 단점 

인기 있는 번들러들에서는 `re-exports`로 인해 코드 분할이 제대로 작동하지 않는다. 이 방법을 사용하면 트리 셰이킹이 전체 모듈을 제거하는 것은 안전하지만, 모듈의 일부만 제거하는 것은 안전하지 않기 때문이다.

예를 들어, `authModel`을 페이지 모델에 가져오면, 해당 페이지에서 `AuthForm` 컴포넌트를 사용하지 않더라도 이 컴포넌트가 해당 페이지의 청크에 포함된다.

결과적으로 청크의 초기화가 더 비싸지게 되며, 브라우저는 번들 안에 포함된 모든 모듈을 처리해야 하기 때문이다. 이는 번들에 "함께 따라온" 모듈들까지 처리하는 것을 의미한다.

### 가능한 해결책

`webpack`은 재내보내기 파일을 **사이드 이펙트가 없는 파일(side effects free)** 으로 표시할 수 있다. 이를 통해 `webpack`은 해당 파일을 사용할 때 더 공격적인 최적화를 사용할 수 있다.