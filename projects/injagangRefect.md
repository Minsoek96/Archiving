# 인자강 리팩토링 이야기

본 리팩토링 사항은 인프런 인턴십을 통해 많은 배움을 깨우쳤던 2023년 10월을 기준으로 작성된 글을 옮겼습니다. 

## 프리온보딩 인터십의 배움을 적용

프리온보딩 인터십 과정을 통해 개발자로서 중요한 깨달음을 얻었습니다.
초기에는 독학한 지식을 바탕으로 프로젝트를 진행하며, 나름대로 기준을 정하여 컴포넌트를 단위별로 나누고, 네이밍에도 신경을 쓰며 프로젝트를 진행했다고 생각했습니다. 하지만, 프리온보딩 인턴십 과정에서 다른 프론트 개발자들과 협업을 통해 기업과제를 진행하면서 그동안 잘못된 방향으로 코드를 작성했다는것을 깨달을수있었습니다.
매주 진행된 멘토님의 코드 리뷰에서 나쁜 코드의 예시를 볼 때마다, 지난날 작성했던 코드들이 떠올랐습니다. 특히, 멘토님이 추후에 리팩토링을 기약하며 기능 구현에만 초점을 둔 채 코드를 생각 없이 작성하는 것은 개발자로서 정말 좋지 못한 가치관이라고 강조 하셨고, 저 또한 그 의견에 깊이 공감 하였습니다. 과거에는 미숙함으로 잘못된 방향으로 코드를 작성했을지 모르나, 지금은 과거 보다 더 나은 코드를 작성할 수 있는 능력과 지식을 갖췄다 자신하기에 지난 실수를 바로잡기 위해 리팩토링에 대한 결심을 하게 되었습니다.

## 프로젝트 초기 구현 전체적 공통 문제

1. 관심사 분리 X : 기본적으로 비즈니스 로직와 View가 분리가 안되었습니다.

2. 추상화 복잡성 : 컴포넌트 분할은 되어있지만 그 기준이 명확하지 못하여 하나의 컴포넌트가 많은 기능을 담당하여 추상화 자체가 불가능한 상황입니다.

3. Props드릴링 : 리덕스를 사용하고 있음에도 서버에 관련된 상태만 관리하고, 대부분의 컴포넌트 내에서는 Props드릴링 구조가 눈에 띄었습니다.

4. 매직 넘버 사용 : 코드 내에서 무엇을 의미하는지 한눈에 알 수 없는 매직넘버가 코드의 가독성을 저하시키고 유지 보수를 어렵게 만들었습니다.

## 해결방안

관심사 분리 : 컴포넌트는 단순히 UI를 표현하는 부분만을 담당하게 초점을 맞춘다.

- 모든 비즈니스 로직은 Hooks 또는 유틸함수 등으로 분리한다.
- 비즈니스 로직을 담당하는 커스텀훅은 `use도메인명Logic`의 형태로 작성한다.
- 리덕스 관련 로직은 `use도메인명Manager`의 형태로 관리하여 리덕스에 대한 컴포넌트의 의존도를 최소화 한다.

컴포넌트 추상화 : 컴포넌트의 재사용성과 관심사의 분리를 위해 컴포넌트를 최대한 단순하게 만들고, 특정 기능만을 담당하게 한다.

Props드릴링 문제 해결 : 프로젝트 내에서 리덕스를 채택하고 있기 때문에 2단계의 과정을 거치는 Props는 리덕스를 통해 관리한다.

- server와 관련된 상태는 `도메인명/server`에서 관리한다.
- client와 관련된 상태는 `도메인명/user`에서 관리한다.

## 코드 구조 및 디렉토리 규칙 :

```shell
**리덕스**

📦Template
┣ 📂server
┃ ┣ 📜actions.tsx
┃ ┣ 📜reducer.tsx
┃ ┗ 📜types.tsx
┗ 📂user
┃ ┣ 📜actions.ts
┃ ┣ 📜reducer.ts
┃ ┗ 📜types.ts

**컴포넌트**

📦MyProfile
┣ 📂hooks
┃ ┣ 📜useMyProfileLogic.ts
┃ ┗ 📜useMyProfileManager.ts
┣ 📜CheckMyInFo.tsx
┣ 📜PassWordSetting.tsx
┗ 📜UserInfoSetting.tsx
```

## 기능별 리팩토링 상세 계획

- [API](./injagangRefects/apiRefect.md)
- [Board](./injagangRefects/boardRefect.md)
- [Edit](./injagangRefects/editRefect.md)
- [Template](./injagangRefects/templateRefect.md)
- [Theme](./injagangRefects/themeRefect.md)
- [Type](./injagangRefects/typeRefect.md)
- [interView](./injagangRefects/interView.md)

## Toast 알림 추가

기존에는 API요청에 대한 알림이 없어 단순한 처리만 하였습니다. 하지만 이런 방식은 사용자의 경험에 좋지 못하다는 판단이 들어 알림을 설정 하고자 하였습니다.

프로젝트 내에는 모달이 구현되어있었지만 모달은 사용자에게 경고창이나 확실히 알려야하는 메시지를 전달하는 역할에는 좋지만 사용자의 경험을 방해합니다.

## 회원가입

초기 구현에서는 파일을 여러 개로 나누는 것이 작업 효율을 저하시킬 것이라고 판단했습니다. 그러나 이러한 접근법은 코드의 가독성을 해치고 유지보수의 어려움을 초래했습니다.

### 복잡한 구조 정리

- `signupPage`에서 주요한 라우팅 및 뷰 비즈니스 로직에만 집중하게 하기 위해 컴포넌트를 분리 하였습니다.
- `useSignUpManager` : 리덕스의 디스패치를 담당
- `useSignUpLogic` : 비즈니스 로직을 담당

### 절차적인 코드 정리

- 기존에는 유효성검사를 절차적인 방식의 코드로 작성하였습니다.

```jsx
if (hasEmptyFields(joinInfo)) {
  setMsg(ERROR_MESSAGES.FILL_BLANKS);
  return;
}

if (!isValid) {
  setMsg(errorMessage);
  return;
}

if (joinInfo.password !== joinInfo.confirmPassword) {
  setMsg(ERROR_MESSAGES.CHECK_PASSWORD);
  return;
}
```

이를 선언적인 코드로 변경했습니다.

```jsx
const validation = [
  {
    check: () => hasEmptyFields(joinInfo),
    message: ERROR_MESSAGES.FILL_BLANKS,
  },
  {
    check: () => !isValid,
    message: errorMessage,
  },
  {
    check: () => joinInfo.password !== joinInfo.confirmPassword,
    message: ERROR_MESSAGES.CHECK_PASSWORD,
  },
];

const runValidationChecks = () => {
  const isValidation = validation.find((item) => item.check());
  if (isValidation) {
    setUserMsg(isValidation.message);
    return false;
  }
  return true;
};

if (!runValidationChecks()) return;
```

- 이전보다 코드가 길어져 효율이 떨어진 것 처럼 보이지만 실제적으로 사용되는 코드는 if(!runValidationChecks()) return 한줄 입니다.
- 만약 추가 유효검사 로직을 추가해야 하는 상황이라면 `validation` 만 보수하면 유효검사 로직이 정상 작동하게됩니다.

## useToast

기존에 메시지 전달 방식은 모달을 이용하였습니다. 모달은 유저에게 명확한 메시지를 전달하는 데에는 장점이 있지만, 사용자 경험을 방해한다는 큰 단점도 가지고 있습니다. 경고창의 알림 등, 유저의 집중이 필요한 상황에서는 모달이 적합하지만, 성공 알림이나 간단한 경고 알림 등은 사용자의 경험을 방해할 정도로 알릴 필요는 없다고 생각하였습니다. 따라서, 일시적으로 사라지는 토스트 알림을 구현하였습니다.

### 고려사항

- 위치
- 일시적으로 사라져야한다.
- 알림의 형태에 따라 색상이 달라야한다.
- 메시지를 덮는 방식이 아닌 연쇄적인 흐름
- 타이머 조절 기능
- 비동기 로직의 연결 여부에 따라 메시지 알림 표현 가능

### 구현 과정

- 위치
  - 화면 하단 오른쪽 or 화면 상단 오른쪽
- **일시적으로 사라져야한다.**

  - setTimer를 설정하여 일정시간이 지나면 배열에서 해당 toast 제거
  - fadeIn, fadeOut 와 같은 애니메이션을 적용하여 시각적으로 표현

- **알림의 형태에 따라 색상이 달라야한다.**

  - SUCCESS, WARNING, ERROR, INFO 4가지의 케이스를 분류
  - 케이스별 색상 지정
  - Toast 객체 내에 mode타입 지정
  - mode타입별 색상 렌더링

- **메시지를 덮는 방식이 아닌 연쇄적인 흐름**

  - Toast 타입을 string 이 아닌 string[] 형태로 관리
  - 각 아이템별 맵핑

- **타이머 조절 기능**

  - useToast의 props 값으로 duration 지정
  - 각 애니메이션에 duration 적용

- **비동기 로직의 연결 여부에 따라 메시지 알림 표현 가능**
  - toast상태를 리덕스에서 관리
  - showToast에 dispatch 연결

### 토스트 코드

```jsx
export interface IToast {
  id: string;
  message: string;
  mode: TOAST_MODE;
  duration: number;
  startTime: number;
}

export type TOAST_MODE = (typeof TOAST_MODE)[keyof typeof TOAST_MODE];

const useToast = (duration: number = 3000) => {
  const { toastList } = useSelector((state: RootReducerType) => state.toast);
  const dispatch = useDispatch();

  const showToast = useCallback(
    (mode: TOAST_MODE = "Info", message: string) => {
      dispatch(showToastAction(mode, message, duration));
    },
    [],
  );

  const RenderToast: React.FC = () => (
    <ToastContainer>
      {toastList &&
        toastList.map(toast => <ToastItem key={toast.id} {...toast} />)}
    </ToastContainer>
  );
  return [showToast, RenderToast] as const;
};
export default useToast;

const ToastContainer = styled.div`
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 1000;
`;
```
