# msw v1 -> v2

## 정리

### 최소환경

Node.js 버전 18.0.0이상  
타입스크립트 버전 4.7이상

### 엔트리 포인트 변경

**Before:**

```jsx
import { setupWorker } from "msw";
```

**After:**

```jsx
import { setupWorker } from "msw/browser";
```

### Response resolver arguments

더 이상 req, res 및 ctx 인수를 받지 않습니다.

**Before:**

```jsx
rest.get("/resource", (req, res, ctx) => {});
```

**After:**

```jsx
http.get("/resource", (info) => {});
```

### Request Changes

URL 인스턴스로 작업하려면 먼저 요청 url 문자열에서 인스턴스를 만들어야 한다.

**Before:**

```jsx
rest.get("/resource", (req) => {
  const productId = req.url.searchParams.get("id");
});
```

**After:**

```jsx
import { http } from "msw";

http.get("/resource", ({ request }) => {
  const url = new URL(request.url);
  const productId = url.searchParams.get("id");
});
```

### Request params

경로 매개 변수에 액세스하려면 응답 확인자에서 매개 변수 개체를 사용한다.

**Before:**

```jsx
rest.get("/post/:id", (req) => {
  const { id } = req.params;
});
```

**After:**

```jsx
import { http } from "msw";

http.get("/post/:id", ({ params }) => {
  const { id } = params;
});
```

