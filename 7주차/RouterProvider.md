# React Router

## RouterProvider

> 라우텅 구성을 위한 컴포넌트

### 주요기능

라우팅 컨텍스트 제공 :
`RouterProvider`는 자식 컴포넌트에 라우팅 관련 컨텍스트를 제공한다.  
-> 자식 컴포넌트들이 현재의 라우트, 히스토리, 위치등에 접근할 수 있다.

**라우트 설정** :  
애플리케이션에서 사용할 라우트(route)를 정의한다.  
-> 해당 라우트는 다양한 URL에 매칭하는 컴포넌트를 렌더링한다.

**히스토리 관리** :  
브라워즈이 히스토리 API와 연동하여 페이지 네비게이션을 관리한다.

**라우터 컨피규레이션** :  
`RouterProvider`는 `createBrowserRouter`나 `createMemoryRouter` 등을 통해 생성된 라우터 객체를 받아, 라우팅을 처리한다.

### 코드

```jsx
import { RouterProvider, createBrowserRouter } from "react-router-dom";

import routes from "./routes";

const router = createBrowserRouter(routes);

const App = () => (
  <div>
    <RouterProvider router={router} />
  </div>
);

export default App;
```

```jsx
import Home from "./components/Home";
import About from "./components/About";
import Layout from "./components/Layout";

const routes = [
  {
    element: <Layout />,
    children: [
      { path: "/", element: <Home /> },
      { path: "/about", element: <About /> },
    ],
  },
];

export default routes;
```
