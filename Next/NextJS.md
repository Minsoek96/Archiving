# Next.js

## AppRouter

> Next.js @v13에서 추가된 라우팅 방식 

라우팅 제어를 세밀하게 제어 할 수 있어 페이지 전환시 사용자의 경험을 향상 시킬 수 있다.   
이전에는 `Page Router`방식

---

### Layout(`layout.tsx`)

> 여러페이지 또는 경로에서 공통적으로 사용되는 UI

```jsx
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return <section>{children}</section>;
}
```

레이아웃은 일반 page와 달리 URL의 쿼리 파라미터의 영향을 받아 페이지가 리렌더링 되지 않아야 하기 때문에 `searchParams`를 받지 않는다.

사용자가 다른 경로로 이동하더라도 공통 레이아웃 부분은 유지되며 리렌더링 되지 않는다.

---

### Template(`template.tsx`)

```jsx
export default function Template({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>;
}
```

`Layout`와 유사하지만 state를 보존하는 Layout와 달리 항상 새로운 인스턴스를 생성 한다.
페이지 조회수 기록 같은 기능에 효과적이다

---

### 계층 순위

**Layout** `⇒` **HomeLayout** `⇒` **HomePage**

---
