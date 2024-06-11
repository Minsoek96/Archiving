# 인자강 리팩토링 이야기

본 리팩토링 사항은 인프런 인턴십을 통해 많은 배움을 깨우쳤던 2023년 10월을 기준으로 작성된 글을 옮겼습니다. 현재 2024년 6월은 이 글을 작성한 시점보다 더 발전했다고 자부할 수 있습니다.


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

## Toast 알림 추가

기존에는 API요청에 대한 알림이 없어 단순한 처리만 하였습니다. 하지만 이런 방식은 사용자의 경험에 좋지 못하다는 판단이 들어 알림을 설정 하고자 하였습니다.

프로젝트 내에는 모달이 구현되어있었지만 모달은 사용자에게 경고창이나 확실히 알려야하는 메시지를 전달하는 역할에는 좋지만 사용자의 경험을 방해한다.

## API 요청

### 기존 문제

> 기존의 API요청 로직은 하드 코딩 방식을 사용하여 API를 모듈화 하지 않았습니다. 이 방식은 여러가지 문제를 유발 하였습니다.

- URL을 잘못 입력하거나 변경될 경우, 수동으로 모든 링크를 수정해야하는 번거로움
- 비동기 함수 로직의 관심사 분리가 어려워 URL의 의도를 파악하기 어려움
- 단순히 기능 구현에 초점을 맞춘 유지보수가 어려운 상태

```jsx
export const getProfile =
  () =>
  async (dispatch: Dispatch<authDispatchType>): Promise<void> => {
    try {
      // console.log({ headers });
      const response = await fetcher(METHOD.GET, "/info", {
        headers: { Authorization: Cookies.get("accessToken") },
      });
      if (response) {
        dispatch({
          type: PROFILE_SUCCESS,
          payload: {
            nickname: response.data.nickname,
            role: response.data.role,
          },
        });
      }
    } catch (error: any) {
      dispatch({ type: AUTHENTICATE_FAILURE, payload: { error } });
    }
  };
```

### 개선 방안

**Axios 인스턴스 활용**  
: Axios의 기능을 활용하지 못하였습니다.  
주로, JSON 문자열 처리의 간편한 목적에 사용하였습니다.

문제점 : 매 요청마다 쿠키에 인증에 필요한 정보를 직접 추출하여 입력해야 하여 코드의 가독성을 저하시키며 유지보수를 어렵게 만들었습니다.

해결법 : Axios 인스턴스를 생성하고 기본 헤더 설정 로직을 추가하여 매 요청 시 존재하는 토큰이 자동으로 헤더에 추가 될 수 있게 구현하였습니다.

```jsx
import axios from "axios";
import { SERVER } from "./config";
import Cookies from "js-cookie";
import { error } from "console";
export const API = axios.create({
  baseURL: SERVER,
  headers: {
    "Content-Type": "application/json",
  },
});

API.interceptors.request.use(
  config => {
    if (Cookies.get("accessToken")) {
      config.headers.Authorization = Cookies.get("accessToken");
    }
    return config;
  },
  error => {
    return Promise.reject(error);
  },
);

export enum METHOD {
  GET = "get",
  POST = "post",
  PUT = "put",
  PATCH = "patch",
  DELETE = "delete",
}

/**AXIOS를 좀더 오류 없이 사용하기위한 역할 */
export const fetcher = async (
  method: METHOD,
  url: string,
  ...rest: { [key: string]: any }[]
) => {
  const res = await API[method](url, ...rest);
  return res;
};
```

#### 도메인별 API 엔트리 포인트

: 각 도메인별 API엔트리포인트를 객체화 하였습니다.

```jsx
export const SERVER = process.env.BACKEND_SERVER_API;

export const AUTH_APIS = {
  INFO_API: `${SERVER}/info`,
  SIGNUP_API: `${SERVER}/signup`,
  SIGNIN_API: `${SERVER}/login`,
  LOGOUT_API: `${SERVER}/logout`,
  TOKKEN_REISSUE_API: `${SERVER}/reissue`,
  NICK_CHNAGE_API: `${SERVER}/nicknameChange`,
  PASSWORD_CHAGNE_API: `${SERVER}/passwordChange`,
};
```

#### 비동기 처리 함수를 모듈화

:API/도메인/도메인명API 구조로 관리하였습니다.

```jsx
export const authInfoAPI = async () => {
  return fetcher(METHOD.GET, AUTH_APIS.INFO_API);
};
```

## 결과

- 함수의 책임이 명확히 분리되어 API에 대한 책임에만 집중하게 되었습니다.
- 세부 사항은 authInfoAPI(도메인API)에 위임하여 유지보수 효율이 향상되고, 하드코딩으로 인한 오류를 방지할 수 있었습니다.

```jsx
export const getProfile =
  () =>
  async (dispatch: Dispatch<authDispatchType>): Promise<void> => {
    try {
      const response = await authInfoAPI();
      if (response) {
        dispatch({
          type: PROFILE_SUCCESS,
          payload: {
            nickname: response.data.nickname,
            role: response.data.role,
          },
        });
      }
    } catch (error: any) {
      dispatch({ type: AUTHENTICATE_FAILURE, payload: { error } });
    }
  };
```

## 타입 분리

### 문제점

- 타입스크립트를 처음 사용할 때 타입스크립트에 대한 이해가 부족했습니다.
- 단순히 기업에서 선호하는 기술 스택이라는 이유로 선택해 사용하였습니다.
- 타입스크립트의 오류메시지에 따라 타입을 유연하게 조정하였습니다.
- 타입의 규칙도 없이 때로는 `Type`을 사용하거나 `interface`를 정의하거나, 파일 최상단 혹은 함수 바로 위에 타입을 배치하는 등 일관성 없이 사용했습니다.

: 그럼에도 불구하고 타입스크립트는 자바스크립트보다 오류 대응에는 더 나아 만족했습니다.

인턴십 수업을 통한 변화

- 타입스크립트는 단순히 타입 오류를 미리 방지하는 것 이상의 가치가 있다.
- 인터페이스를 통해 로직의 기능을 추상화하며 표현력을 높일 수 있다.

프로젝트 반영

> 규칙없이 작성한 타입들을 모아 `types`라는 폴더안에 `도메인별`로 정리하였습니다.

- 유지보수가 쉬워졌고, 코드의 가독성이 향상되었습니다.
- 각 카테고리별로 인터페이스만 확인해도 문서를 효과를 가졌다.

```bash
📦types
 ┣ 📂auth
 ┃ ┗ 📜AuthType.tsx
 ┣ 📂essay
 ┃ ┗ 📜EssayType.ts
 ┣ 📂feedback
 ┃ ┗ 📜FeedBackType.ts
 ┣ 📂InterViewQuestion
 ┃ ┗ 📜InterViewQuestionType.ts
 ┣ 📂qnaBoard
 ┃ ┗ 📜QnaBoardType.ts
 ┗ 📂template
 ┃ ┗ 📜TemplateType.ts
```

## 테마 분리

### 계획

#### 문제점

- 모든 컴포넌트는 레이아웃 컴포넌트를 통해 테마 관련 상태를 공통적으로 관리하고 있었습니다.
- 기존에는 데이터를 하드코드 방식으로 저장

#### 해결법

- `useThemeToggler` 커스텀 훅을 생성하여 테마 상태 관리를 처리 하여 가독성을 높이고 유지보수성을 향상 시켰습니다.
- `LocalStorageManager(key)` 클래스를 구현 하여 스토리지의 관심사를 분리하고 활용성을 향상 시켰습니다.

### 테마 리팩토링 이전 코드

-`Layout컴포넌트`는 레이아웃 렌더링와 테마 토글링의 두 가지 주요 관심사를 가졌습니다.

- 테마 관련 상태관리에 대한 절차적인 과정이 포함되어 있었습니다.

```tsx
import { useState } from "react";

import Head from "next/head";
import WithAuth from "./WithAuth";
import NavBar from "../components/Nav/NavBar";

/**컴포넌트와 navbar의 영역을 분할하고, 전체 컴포넌트의 렌더링을 통제하기 위한 역할*/
const Layout = ({ children }: LayoutProps) => {
  const [isDarkMode, setIsDarkMode] = useState(false);
  const router = useRouter();
  const routes = ["/join", "/login"];
  const isShowNav = routes.includes(router.asPath);

  const handleToggleTheme = () => {
    const newMode = !isDarkMode;
    setIsDarkMode(newMode);
    localStorage.setItem("theme", JSON.stringify(newMode));
  };

  //유저의 브라우저에 저장된 테마를 반영하기위한 역할
  useEffect(() => {
    const storageTheme = localStorage.getItem("theme");
    if (storageTheme) {
      const isThemeMode = JSON.parse(storageTheme);
      if (isThemeMode !== isDarkMode) {
        setIsDarkMode(isThemeMode);
      }
    }
  }, []);

  return (
    <ThemeProvider theme={isDarkMode ? darkTheme : lightTheme}>
	@@ -68,10 +53,7 @@ const Layout = ({ children }: LayoutProps) => {
      </Head>
      <LayoutStyle>
        <Sidebar>
          <NavBar
            toggleTheme={handleToggleTheme}
            mode={isDarkMode}
          />
        </Sidebar>
        <Content>{children}</Content>
      </LayoutStyle>
```

#### 테마 리팩토링 이후

- `theme`에 대한 책임을 분리 하기 위해 `useThemeToggler` 커스텀훅을 추가 하여 책임을 분리함과 동시에 절차적인 코드를 숨겼습니다.

```jsx
import { useState } from "react";

import Head from "next/head";
import NavBar from "../components/Nav/NavBar";
import useThemeToggler from "@/hooks/useThemeMode";

/**컴포넌트와 navbar의 영역을 분할하고, 전체 컴포넌트의 렌더링을 통제하기 위한 역할*/
const Layout = ({ children }: LayoutProps) => {
  const [isDarkMode, ChangeDarkMode] = useThemeToggler(false);
  const router = useRouter();
  const routes = ["/join", "/login"];
  const isShowNav = routes.includes(router.asPath);

  return (
    <ThemeProvider theme={isDarkMode ? darkTheme : lightTheme}>
	@@ -68,10 +53,7 @@ const Layout = ({ children }: LayoutProps) => {
      </Head>
      <LayoutStyle>
        <Sidebar>
          <NavBar toggleTheme={ChangeDarkMode} mode={isDarkMode} />
        </Sidebar>
        <Content>{children}</Content>
      </LayoutStyle>
      )
```

### 테마 리팩토링 결과

이러한 리팩토링을 통해 테마 상태 관리의 책임을 useThemeToggler 훅으로 분리하고, 레이아웃 컴포넌트는 단순히 테마 상태를 전달하고 토글하는 역할에 집중하게 되었습니다. 이로 인해 코드의 가독성과 유지보수성이 향상되었습니다.

## 템플릿

### 문제점

템플릿 관련 코드를 리팩토링하기 위해 현재 코드 상태를 평가해 보았습니다.

- 컴포넌트의 명칭이 직관적이지 못했습니다.
- 컴포넌트를 분리하는 기준이 명확하지 않았습니다.
- 전체적으로 주석을 봐도 알아보기 힘든 구조였습니다.
- 리덕스를 사용하고 있음에도 Props 드릴링 문제가 있었습니다.

### 리팩토링 적용 사항

1. **서버 상태와 클라이언트 상태를 분리하여 관리**

   - **이전**: template/redux
   - **이후**: template/server - user 디렉토리 구조로 분리

2. **템플릿 리스트 컴포넌트의 복잡함 해결**

   - **해결방안**: 리덕스의 디스패치를 커스텀 훅으로 분리하여 라이브러리에 대한 의존성 문제를 해소하고, 컴포넌트는 단순히 데이터를 가져오고 필요한 인자를 전달하는 역할에 충실하게 하였습니다.
   - `useTemplateManager` (서버 관련 상태 디스패치 전담)
   - `useUserTemplateManager` (유저 관련 상태 디스패치 전담)

3. **복잡한 추상화의 컴포넌트**

   - **기존 구조**: Template/TemplateList - TemplateQuestionAdd
   - **변경된 구조**: Template/TemplateList - TemplateTitleItem - TemplateQuestionAdd - TemplateDetail
   - `TemplateList`: 템플릿 리스트를 가져오며, 유저의 행동에 따라 렌더링을 결정합니다.
   - `TemplateTitleItem`: 템플릿 아이템을 렌더링하며, 유저 관리 상태를 전달합니다.
   - `TemplateQuestionAdd`: 템플릿 추가 기능을 담당합니다.
   - `TemplateDetail`: 유저가 선택한 템플릿을 렌더링하며, 선택한 템플릿 삭제 기능을 관리합니다.

4. **매직 넘버 제거**
   - `selectedTemplateList.questions.length < 1` 와 같은 의도 파악이 어려운 조건문을 `isTemplateSelected` 와 같은 변수를 만들어 처리합니다.

### 템플릿 리팩토링 적용후 :

```bash
📦Template
 ┣ 📂server
 ┃ ┣ 📜actions.tsx
 ┃ ┣ 📜reducer.tsx
 ┃ ┗ 📜types.tsx
 ┗ 📂user
 ┃ ┣ 📜actions.ts
 ┃ ┣ 📜reducer.ts
 ┃ ┗ 📜types.ts
```

```jsx
const TemplateList = () => {
  const { templateList, loading, error, getTemplateList } =
    useTemplateManager();
  const { isAddTemplate, setIsAddTemplate } = useUserTemplateManager();

  useEffect(() => {
    getTemplateList();
  }, []);

  if (error) return <p>Error발생</p>;

  return (
    <TemplateStlyed>
      <Card>
        <TemplateTtileList>
          {templateList.map((item) => (
            <TemplateTitleItem key={item.templateId} list={item} />
          ))}
          {!isAddTemplate && (
            <BiPlus onClick={() => setIsAddTemplate(true)}></BiPlus>
          )}
        </TemplateTtileList>
        <TemplateViewController>
          {loading && <p>로딩중</p>}
          {isAddTemplate ? <AddTemplate /> : <TemplateDetail />}
        </TemplateViewController>
      </Card>
    </TemplateStlyed>
  );
};
```

- 최대한 코드 내에서 절차적인 성향을 지우기 위해 커스텀훅을 활용하여 로직을 분리하였습니다.
- 리덕스에 대한 컴포넌트의 의존도를 최소화 하기 위해 리덕스에 대한 내용을 커스텀훅에 감췄습니다.
- `useTemplateManger()` 서버 상태 관련 dispatch 관리
- `useUserTemplateManager()` 클라이언트 상태 관련 dispatch 관리

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

## EDIT? : 자기소개서 작성 & 편집

해당 컴포넌트는 유저의 자소서 리스트를 관리하기 위한 목적으로 작성되었습니다.

- EDIT라는 명칭은 의미를 파악하기 힘들었습니다.
- 이전 문제와 동일하게 컴포넌트의 관심사 분리 & 매직넘버 문제가 있었습니다.
- Props드릴링 문제도 발생했습니다.

### 상세 문제점 파악

- 표현이 애매한 도메인명
  - EDIT : 편집 무엇을 편집?
- 복잡한 페이지 라우팅 구조 & 명칭
  - myEssay : 자소서 확인 페이지
  - edit : 자소서 작성 페이지 라우팅
  - edit essay=2 : 자소서 수정 페이지
- idx로 이루어진 map 키값

### EDIT 해결 방안

- 대표 도메인명 수정
  - coverLetter: 자기소개서
- 복잡한 라우팅 & 명칭 개선
  - coverLetter : 자소서 확인 페이지
  - coverLetter/new : 자소서 작성 페이지
  - coverLetter/[id]/edit: 자소서 편집 페이지
- 컴포넌트 관심사 분류
- UUID4()를 키값으로 사용하기
- 트리가 깊은 경우 리덕스 활용

### 자기소개서 리팩토링 적용전

#### 유저 자소서 리스트 관리

- 유저가 선택한 자소서 넘버 상태를 관리 하기 위한 로직이 Props드릴링 구조의 주요 원인이라 판단되었습니다.

#### 유저 자소서 생성 & 편집

> 생성과 편집의 스타일이나 로직이 비슷하다는 이유로 하나의 컴포넌트에서 이를 처리하려 했던 안일한 접근이 문제였습니다.

- `isEdit` 상태에 대한 의존도가 높아져 가독성이 떨어지고 유지보수 측면에서 비효율적이었습니다.
- 삼항 연산자를 이용하여 기능을 분류하는 방식은 절차적인 성향의 코드에 가까워 컴포넌트 간의 의존도를 높이고 유지보수 측면에서 매우 비효율적이며 가독성을 떨어트렸습니다.

---

[ `해결방안`]

- **컴포넌트 분리**: 생성 기능과 편집 기능을 별도의 컴포넌트로 분리하였습니다.
  커스텀 훅 활용: 비슷하고 겹치는 비즈니스 로직은 커스텀 훅으로 분리하여 상태 관리를 효율적으로 하였습니다.
- **명칭 변경**: `handle ~~` 형태의 불명확한 명칭을 최대한 직관적으로 변경하였습니다.
- **고유 키 값 사용**: 서버에서 id 값을 지정해주기 때문에 key 값에 크게 신경 쓰지 않았으나, CRUD 관련 컴포넌트의 맵핑에서는 key 값을 유니크한 값으로 지정하는 것이 옳다고 판단하여 `uuid4()`를 채택하였습니다.

### EDIT 리팩토링 적용전 코드

```jsx
type QnAEditorProps = {
  isEdit: boolean;
};

const QnAEditor = ({ isEdit }: QnAEditorProps) => {
  {
    /**리덕스에 대한 선언 */
  }
  const router = useRouter();
  const dispatch = useDispatch();
  /**나의 자소서를 호출중 판단여부 */
  const readEssayLoading = useSelector(
    (state: RootReducerType) => state.essay.loading,
  );
  /**나의 자소서 리스트 */
  const readEssayList = useSelector(
    (state: RootReducerType) => state.essay.readEssayList,
  );
  /**관리자의 샘플 리스트 */
  const templateReducer: InitiaState = useSelector(
    (state: RootReducerType) => state.template,
  );

  const [qnaLists, setQnALists] = useState<QnAList[]>([]);
  const [templateTitle, setTemplateTitle] = useState<string>("커스텀자소서");
  const [mainTitle, setMainTitle] = useState<string>("");
  const [qnaContent, setQnAContent] = useState<qnaList>([]);
  const [isOpenModal, setIsOpenModal] = useState<boolean>(false);
  const [modalMsg, setModalMsg] = useState<string>("");
  const essayId = isEdit
    ? (JSON.parse(router.query.essayId as string) as number)
    : 0;

  /** ESSAY의 로딩이 완료가되면 useState에 반영한다. */
  useEffect(() => {
    if (!readEssayLoading) {
      if (readEssayList[0].title === "") {
        return;
      }
      setQnALists(readEssayList);
      setTemplateTitle(readEssayList[0]?.title);
    }
  }, [readEssayLoading]);

  /** 템플릿의 로딩이 완료가되면 useState에 반영한다. */
  useEffect(() => {
    if (!templateReducer.loading) {
      const customTemplate = {
        templateId: 10000,
        title: "커스텀자소서",
        qnaList: [],
      };
      setQnALists(cur => [customTemplate, ...templateReducer.templateList]);
    }
  }, [templateReducer.templateList]);
  /** 수정모드에서는 기존의 리스트를 반환, 작성모드에서는 선택된 템플릿 리스트를 반환 */
  const getQuestionItem = useCallback(() => {
    if (isEdit) {
      return qnaLists;
    }
    const filteItem = qnaLists.filter(list => list.title === templateTitle);
    return filteItem;
  }, [templateTitle, qnaLists]);

  const handleChangeMainTitle = (title: string) => {
    setMainTitle(title);
  };

  /**질문FORM 추가 함수 */
  const handleAddQnA = () => {
    if (templateTitle === "") {
      return;
    }
    setQnALists(prevLists => {
      const newLists = [...prevLists];
      const filterIndex = newLists.findIndex(a => a.title === templateTitle);
      const newContent = [...newLists[filterIndex].qnaList, " "];
      newLists[filterIndex].qnaList = newContent;
      return newLists;
    });
  };

  /**현재 질문리스트 수정 */
  const handleChangeQnA = (index: number, question: string, answer: string) => {
    const newList = [...qnaContent];
    newList[index].question = question;
    newList[index].answer = answer;
    setQnAContent(newList);
  };

  /**현재 질문리스트의 정보를 전달 */
  const handleQnASelect = (index: number, question: string, answer: string) => {
    const filterList = qnaContent[index];
    if (filterList) {
      return;
    } else {
      setQnAContent(curList => [
        ...curList,
        { qnaId: index, question, answer: answer },
      ]);
    }
  };

  /**필터링후 자소서 작성 반영 */
  const handleSubmit = () => {
    const filterJudge = qnaContent.filter(
      a => a.answer === "" || a.question === "",
    ).length;
    if (!isEdit && mainTitle === "") {
      setIsOpenModal(true);
      setModalMsg("메인 제목을 입력해주세요");
      return;
    }
    if (qnaContent.length < 1) {
      setIsOpenModal(true);
      setModalMsg("질문과 답변은 1개이상 작성해주세요.");
      return;
    }
    if (filterJudge > 0) {
      setIsOpenModal(true);
      setModalMsg("질문 답변의 내용을 채워주세요");
      return;
    }
    const data = {
      title: isEdit
        ? mainTitle === ""
          ? templateTitle
          : mainTitle
        : mainTitle,
      qnaList: qnaContent,
    };
    if (isEdit) {
      dispatch(updateEssay(data, essayId));
      router.replace("/myEssay");
      return;
    }
    dispatch(addEssay(data, Number(Cookies.get("useId"))));
    router.replace("/myEssay");
  };


  /**모드1은 액션이 없는 모달, 모드2 삭제액션이 담긴 모달 */
  const handleModal = (mode: number) => {
    if (mode === 1) {
      setIsOpenModal(false);
    }
    if (mode === 2) {
      setModalMsg(`${essayId}번 자소서를 정말 삭제하시겠습니까?`);
      setIsOpenModal(true);
    }
  };

  /**자소서 삭제*/
  const handleDeleteEssay = () => {
    dispatch(deleteEssayList(essayId));
    router.push("/myEssay");
  };

  return (
    <QnAEditorStyle>
      <h2>{isEdit ? "자소서 수정하기" : "자소서 작성하기"}</h2>

      <TopContainer>
        <QnAListTitle onChange={handleChangeMainTitle} />
        <ControlMenu
          Size={{ width: `${v.lgItemWidth}`, height: "40px" }}
          value={templateTitle}
          optionList={qnaLists}
          onChange={setTemplateTitle}
        />
        {qnaLists &&
          getQuestionItem().map(list => (
            <QuestionItmesContainer
              key={isEdit ? list.essayId : list.templateId}
            >
              {list.qnaList &&
                list.qnaList.map((qna, idx) => (
                  <QuestionItem
                    key={idx}
                    content={qna}
                    onChange={handleChangeQnA}
                    curInfo={handleQnASelect}
                    index={idx}
                  ></QuestionItem>
                ))}
            </QuestionItmesContainer>
          ))}
      </TopContainer>

      <BottomContainer>
        <BiPlus onClick={handleAddQnA}></BiPlus>
        <ControlButtonContainer>
          <div>
            <CustomButton
              Size={{ width: "150px", font: "20px" }}
              onClick={() => router.push("/myEssay")}
              text="뒤로가기"
            />
          </div>
          <div>
            {isEdit ? (
              <CustomButton
                Size={{ width: "150px", font: "20px" }}
                onClick={() => handleModal(2)}
                text="삭제하기"
              />
            ) : (
              <></>
            )}
            <CustomButton
              Size={{ width: "150px", font: "20px" }}
              onClick={handleSubmit}
              text={isEdit ? "수정완료" : "작성완료"}
            />
          </div>
        </ControlButtonContainer>
      </BottomContainer>

      {isOpenModal && (
        <Modal
          isOpen={isOpenModal}
          onClose={() => handleModal(1)}
          onAction={isEdit ? handleDeleteEssay : () => handleModal(1)}
          contents={{
            title: "경고",
            content: modalMsg,
          }}
        ></Modal>
      )}
    </QnAEditorStyle>
  );
};

export default QnAEditor;
```

- 주석이 없으면 이해할수 없는 코드와 구조

#### 리팩토링 적용후 코드 일부

```jsx
const CoverLetterEdit = () => {
  const [coverLetterTitle, setCoverLetterTitle] = useState < string > "";
  const router = useRouter();
  const moveCoverLetterMainPage = "/coverLetter";
  const { id } = router.query;

  const { qnaList, setQnAList, deleteQnAList, changeQnAList, addQnAList } =
    useCoverLetterCreatorLogic();

  const {
    loading,
    targetQnAData,
    getDetailEssayList,
    coverLetterMainTitle,
    changeCoverLetter,
    deleteCoverLetter,
  } = useCoverLetterManager();

  useEffect(() => {
    getDetailEssayList(Number(id));
  }, []);

  useEffect(() => {
    if (!loading) {
      setQnAList(targetQnAData);
      setCoverLetterTitle(coverLetterMainTitle);
    }
  }, [targetQnAData]);

  if (loading) return <p>로딩중...</p>;

  return (
    <CoverLetterCreatorContainer>
      <MainTitle>자소서 수정하기</MainTitle>
      <CoverLetterTitle
        value={coverLetterTitle}
        onChange={(e) => setCoverLetterTitle(e.target.value)}
        placeholder="자소서 제목"
      ></CoverLetterTitle>
      {qnaList.map((qna, i) => (
        <CoverLetterQuestionItems
          key={qna.qnaId}
          item={qna}
          onDelete={deleteQnAList}
          onUpdate={changeQnAList}
        ></CoverLetterQuestionItems>
      ))}
      <BiPlusStyled onClick={addQnAList}></BiPlusStyled>
      <ControllerBtns>
        <CustomButton
          Size={{ width: "150px", font: "20px" }}
          onClick={() => router.push(moveCoverLetterMainPage)}
          text={"뒤로가기"}
        />
        <CustomButton
          Size={{ width: "150px", font: "20px" }}
          onClick={() => deleteCoverLetter(Number(id))}
          text={"삭제하기"}
        />
        <CustomButton
          Size={{ width: "150px", font: "20px" }}
          onClick={() =>
            changeCoverLetter(Number(id), coverLetterTitle, qnaList)
          }
          text={"수정완료"}
        />
      </ControllerBtns>
    </CoverLetterCreatorContainer>
  );
};

export default CoverLetterEdit;
```

- 비즈니스 로직을 외부로 분리하여 가독성과 관리가 용이 해졌습니다.
- 비즈니스 로직을 커스텀훅으로 분리하여 재 사용할 수 있는 가능성이 생겼습니다.
- 유지보수 측면에서 효율적으로 변했습니다.
- 컴포넌트 자체는 UI렌더링과 간단한 상태관리에 집중되어 단일 책임 원칙을 지켰습니다.

### 리팩토링 후 구조

컴포넌트

```bash
📦CoverLetter
┣ 📂edit
┃ ┗ 📜CoverLetterEdit.tsx
┣ 📂hooks
┃ ┣ 📜useControlTemplate.ts
┃ ┣ 📜useCoverLetterCreatorLogic.ts
┃ ┗ 📜useCoverLetterManager.ts
┣ 📂new
┃ ┣ 📜CoverLetterCreator.tsx
┃ ┗ 📜CoverLetterQuestionItems.tsx
┣ 📜CoverLetter.tsx
┣ 📜CoverLetterItems.tsx
┣ 📜CoverLetterList.tsx
┗ 📜CoverLetterPreView.tsx
```

라우팅

```bash
📦coverLetter
 ┣ 📂new
 ┃ ┗ 📜index.tsx
 ┣ 📂[id]
 ┃ ┗ 📂edit
 ┃ ┃ ┗ 📜index.tsx
 ┗ 📜index.tsx
```

리덕스

```bash
📦Essay
 ┣ 📂server
 ┃ ┣ 📜actions.tsx
 ┃ ┣ 📜reducer.tsx
 ┃ ┗ 📜types.tsx
 ┗ 📂user
 ┃ ┣ 📜actions.ts
 ┃ ┣ 📜reducer.ts
 ┃ ┗ 📜types.ts
```

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
import React, { useCallback, useState } from "react";
import styled from "styled-components";
import { v4 as uuid4 } from "uuid";
import { TOAST_MODE } from "@/constants";
import ToastItem from "./ToastItem";
import { useSelector } from "react-redux";
import { RootReducerType } from "@/components/redux/store";
import { useDispatch } from "react-redux";
import {
  hideToastAction,
  showToastAction,
} from "@/components/redux/Toast/actions";

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