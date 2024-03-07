# React-Hook-Form

---

> 폼을 쉽고 효율적으로 다루기 위한 라이브러리

- 사용하기 쉬운 유효성 검사 기능을 갖춘 고성능의 유연하고 확장 가능한 폼
- 불필요한 리렌더링을 제거하면서 작성해야 하는 코드의 양을 줄여준다.
- 컴포넌트 리렌더를 분리하여 페이지 또는 앱의 성능을 개선할 수 있다.
- 성능은 폼작성에 있어 사용자 경험의 중요한 측면이다.
- 전체 폼을 다시 렌더링하지 않고도 개별 입력 및 양식 상태 업데이트를 구독할 수 있다.

## register

---

> `useForm` 훅으로 부터 반환되며, 폼 필드(`<input/>`, `<select/>`, `<textarea/>`)을 React Hook Form에 등록하는 역할을 한다. 필드를 등록함으로써 값의 변화를 추적하고, 유효성을 검증할 수 있다.

**RefRef를 이용한 등록** : `ref` 속성을 통해 직접 필드를 등록한다.

```jsx
<input {...register("example")} />
```

**옵션을 포함한 등록**: 필드에 검증 규칙이나 값을 초기화하는 등 추가적인 옵션을 설정해야 할 때 사용한다.

```jsx
<input {...register("example", { required: true, maxLength: 20 })} />
```

### required

- 타입 : `boolean | string`
- 필드가 필수적인지를 지정한다. `true`로 설정하면, 해당 필드가 비어 있을 경우 유효성 검사에 실패한다.
- 문자열을 전달하면 문자열이 오류 메시지로 사용된다.

---

### maxLength

- 타입 : `number`
- 필드 값의 최대 길이를 지정한다.

### minLength

- 타입 : `number`
- 필드 값의 최소 길이를 지정한다.

---

### **max**

- **타입**: **`number`**
- **설명**: 입력 가능한 최대 값을 지정한다.
- 주로 `<input type="number">` 또는 `<input type="date">` 같은 필드에 사용된다.

### **min**

- **타입**: `number`
- **설명**: 입력 가능한 최소 값을 지정합니다.
- **`max`** 옵션과 마찬가지로 주로 숫자나 날짜 입력 필드에 사용된다.

---

### **pattern**

- **타입**: `RegExp`
- **설명**: 필드 값이 일치해야 하는 정규 표현식을 지정한다.
- 주어진 패턴과 일치하지 않는 입력은 유효성 검사에 실패한다.

---

### **validate**

- **타입**: `Function | Object`
- **설명**: 커스텀 검증 로직을 제공한다.
- 함수를 전달하면, 이 함수는 필드 값이 유효한지를 검증하기 위해 호출된다.
- 객체를 전달하는 경우, 각 키는 오류 이름이 되고, 해당 키에 해당하는 함수가 유효성 검증 로직으로 사용된다. 각 함수는 `true`를 반환하거나, 유효성 검증에 실패할 경우 오류 메시지를 반환해야 한다.

---

## Unregister

> `register`를 통해 등록된 필드를 React Hook Form의 관리 대상에서 제외시킬 수 있다.

**단일 필드 등록 해제** : 필드 이름을 인자로 전달하여 단일 필드를 등록 해제할 수 있다.

```jsx
unregister("fieldName");
```

**여러 필드 등록 해제** : 필드 이름들을 배열로 전달하여 여러 필드를 동시에 등록 해제 할 수 있다.

## formState

> `useForm` 훅을 통해 접근할 수 있으며, 폼의 상태 변화를 실시간으로 반영한다. 이 객체는 폼의 유효성 검증 상태, 사용자의 입력 상태, 폼의 제출 상태 등 다양한 정보를 포함하고 있다.

### 주요 속성

`isDirty` : 사용자가 어떤 입력을 변경했는지 여부를 나타낸다. 초기값과 비교하여 변경된 값이 하나라도 있으면 `true`가 된다.

`isValidating` : 폼의 유효성 검사가 진행 중일 때 `true`가 된다.

`isSubmitted` : 폼이 제출되고 있는지 여부를 나타낸다. 폼 제출 로직이 실행되는 동안 `true`로 설정된다.

`submitCount` : 폼이 제출된 횟수를 나타낸다.

`touchedFields` : 사용자가 접근하여 변경했거나 선택했던 필드를 추적한다. 필드에 포커스가 있었다가 빠져나온 경우 해당 필드는 `touched`로 간주 된다.

`error` : 현재 폼에 있는 모든 필드의 유효성 검사 오류를 담고 있는 객체

```jsx
import React from "react";
import { useForm } from "react-hook-form";

function Form() {
  const {
    register,
    handleSubmit,
    formState: { isDirty, isValid, isSubmitting, submitCount },
  } = useForm({
    mode: "onChange",
  });

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("example", { required: true })} />
      <button type="submit" disabled={!isDirty || !isValid || isSubmitting}>
        Submit
      </button>

      <div>
        {isDirty ? <p>Your form is dirty.</p> : <p>Your form is clean.</p>}
        {isValid ? <p>Your form is valid.</p> : <p>Your form is invalid.</p>}
        <p>Submit count: {submitCount}</p>
      </div>
    </form>
  );
}
```

## Watch

> 지정된 필드의 입력을 감시하고 해당 값을 반환한다. 입력 값을 렌더링하고 조건에 따라 렌더링할 내용을 결정할 때 유용하다.

### 사용방법

**특정 필드 구독** : 특정 필드의 이름을 인자로 전달하여 그 필드의 현재 값을 반환받을 수 있다.

```jsx
const yourFieldName = watch("yourFieldName");
```

**여러 필드 동시 구독** : 필드 이름의 배열을 전달하여 여러 필드를 동시에 구독할 수 있다.

```jsx
const { fieldName1, fieldName2 } = watch(["fieldName1", "fieldName2"]);
```

**전체 폼 구독** : 인자 없이 `watch`를 호출하면, 폼에 있는 모든 필드를 구독할 수 있다.

```jsx
const allFields = watch();
const yourFieldName = watch("yourFieldName", "기본값");
```

```jsx
import React from 'react';
import { useForm } from 'react-hook-form';

function Form() {
  const { register, watch } = useForm();
  const showExtraField = watch("toggleExtraField", false);

  return (
    <form>
      <label>
        Show extra field?
        <input type="checkbox" {...register("toggleExtraField")} />
      </label>

      {showExtraField && (
        <label>
          Extra field
          <input {...register("extraField")} />
        </label>
      )}

      <input type="submit" />
    </form>
  );
}
```

## handleSubmit

> 폼 제출 이벤트를 처리하고, 모든 폼 필드의 유효성 검증을 수행한 후, 폼 데이터를 사용할 수 있게 해준다. 유효성 검사에 실패한 경우, 제출을 중단하고 오류 정보를 제공한다.

### 사용방법

```jsx
</> handleSubmit: ((data: Object, e?: Event) => Promise<void>, (errors: Object, e?: Event) => void) => Promise<void>
```

`onSubmit` : 폼의 유효성 검사가 성공적으로 완료되었을 때 호출될 함수이다.  
`onError`(선택적) : 폼의 유효성 검사가 실패했을 때 호출될 함수이다. 

```jsx
import React from 'react';
import { useForm } from 'react-hook-form';

function Form() {
  const { register, handleSubmit, formState: { errors } } = useForm();

  const onSubmit = data => {
    console.log(data);
  };

  const onError = errors => {
    console.error(errors);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit, onError)}>
      <input {...register("example", { required: true })} />
      {errors.example && <span>This field is required</span>}

      <input type="submit" />
    </form>
  );
}

```

### reset

> 사용자가 폼을 제출한 후 폼을 초기화하거나, 사용자가 입력한 값을 지우고 폼을 처음부터 다시 작성할 수 있도록 하기 위해 사용된다. 

## 사용방법 

**특정 값으로 재설정** : `reset` 함수에 객체를 전달하여 폼의 특정 필드를 원하는 값으로 설정할 수 있다. 

```jsx
reset({
    exampler: '새로운 값'
})
```

**폼 상태 옵션** : `reset`함수의 두번째 인자로는 폼의 상태 옵션을 설정할 수 있는 객체를 전달할 수 있다. 

```jsx
reset({}, {
  keepErrors: true, // 오류를 유지할 것인지
  keepDirty: true,  // 변경된 필드 상태를 유지할 것인지
  keepIsSubmitted: false, // 제출된 상태를 초기화할 것인지
  // 기타 폼 상태 옵션
});
```

## Controller 

> React Hook Form을 사용하여 제어 컴포넌트(Controlled Component)를 쉽게 통합하고 관리할 수 있도록 도와준다. React Hook Form은 기본적으로 비제어 컴포넌트를 사용하여 상태를 관리한다.

### 기본 속성 

`name` : 폼 필드의 이름을 지정한다. 이 이름을 통해 폼 데이터 접근하고, 폼 상태를 관리할 수 있다. 

`control` : useForm 훅에서 반환된 `control` 객체를 전달한다. 이 객체를 통해 React Hook Form이 해당 컴포넌트를 폼 필드로 관리할 수 있다. 

`render` : 실제 렌더링할 컴포넌트를 정의하는 함수 이 함수는 `field` 객체를 인자로 받으며, 이 객체에는 `onChange`, `onBlur`, `value` 같은 필드 상태를 관리하는 데 필요한 함수와 값이 포함된다.

`defaultValue`(선택) : 필드의 초기값을 지정한다. 


```jsx
import React from 'react';
import { useForm, Controller } from 'react-hook-form';
import { TextField } from '@material-ui/core'; // 예시로 Material-UI의 TextField를 사용

function Form() {
  const { control, handleSubmit } = useForm();

  const onSubmit = data => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="example"
        control={control}
        defaultValue=""
        render={({ field }) => <TextField {...field} label="Example" />}
      />
      <input type="submit" />
    </form>
  );
}

```