# 테마 분리

## 테마 관련 문제점 분석

### 문제점

- 모든 컴포넌트는 레이아웃 컴포넌트를 통해 테마 관련 상태를 공통적으로 관리하고 있었습니다.
- 기존에는 데이터를 하드코드 방식으로 저장

### 해결법

- `useThemeToggler` 커스텀 훅을 생성하여 테마 상태 관리를 처리 하여 가독성을 높이고 유지보수성을 향상 시켰습니다.
- `LocalStorageManager(key)` 클래스를 구현 하여 스토리지의 관심사를 분리하고 활용성을 향상 시켰습니다.

## 리팩토링 결과 반영 

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

### 테마 리팩토링 이후

- `theme`에 대한 책임을 분리 하기 위해 `useThemeToggler` 커스텀훅을 추가 하여 책임을 분리함과 동시에 절차적인 코드를 숨겼습니다.

```jsx
const Layout = ({ children }: LayoutProps) => {
  const [sgowToast, RenderToast] = useToast();
  const [isDarkMode, ChangeDarkMode] = useThemeToggler(false);
  const theme = isDarkMode ? darkTheme : defaultTheme;

  return (
    <ThemeProvider theme={theme}>
      <LayoutStyle>
        <Sidebar>
          <NavBar toggleTheme={ChangeDarkMode} mode={isDarkMode} />
        </Sidebar>
        <Content>{children}</Content>
      </LayoutStyle>
      <RenderToast />
    </ThemeProvider>
  );
};

export default WithAuth(Layout);
```

### 테마 리팩토링 결과

이러한 리팩토링을 통해 테마 상태 관리의 책임을 useThemeToggler 훅으로 분리하고, 레이아웃 컴포넌트는 단순히 테마 상태를 전달하고 토글하는 역할에 집중하게 되었습니다. 이로 인해 코드의 가독성과 유지보수성이 향상되었습니다.
