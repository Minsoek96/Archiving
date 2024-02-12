# msw v1 -> v2

## 변경 사항

### 최소환경

> Node.js 버전 18.0.0이상  
> 타입스크립트 버전 4.7이상

### 엔트리 포인트 변경

> 브라우저 측 통합과 관련된 모든 것이 msx/browser 엔트리포인트에서 내보내진다.

**Before:**

```jsx
import { setupWorker } from "msw";
```

**After:**

```jsx
import { setupWorker } from "msw/browser";
```

### Response resolver arguments

> 더 이상 req, res 및 ctx 인수를 받지 않습니다.

**Before:**

```jsx
rest.get("/resource", (req, res, ctx) => {});
```

**After:**

```jsx
http.get("/resource", (info) => {});
```

### Request Changes

> URL 인스턴스로 작업하려면 먼저 요청 url 문자열에서 인스턴스를 만들어야 한다.

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

> 경로 매개 변수에 액세스하려면 응답 확인자에서 매개 변수 개체를 사용한다.

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

### Request cookies

> 요청 쿠키에 액세스하려면 응답 resolver의 쿠키 객체를 사용한다.

**Before:**

```jsx
rest.get("/resource", (req) => {
  const { token } = req.cookies;
});
```

**After:**

```jsx
import { http } from "msw";

http.get("/resource", ({ cookies }) => {
  const { token } = cookies;
});
```

### Request body

> `.text(), json(), .arrayBuffer()` 등과 같은 표준 요청 메서드를 사용하여 원하는 대로 요청 본문을 읽어야 한다.

**Before:**

```jsx
rest.post("/resource", (req) => {
  // The library would assume a JSON request body
  // based on the request's "Content-Type" header.
  const { id } = req.body;
});
```

**After:**

```jsx
import { http } from "msw";

http.post("/user", async ({ request }) => {
  // Read the request body as JSON.
  const user = await request.json();
  const { id } = user;
});
```

### Response declaration

> 모의 응답을 선언하려면 Fetch API응답 인스턴스를 생성하고 응답 resolver에서 반환한다.

**Before:**

```jsx
rest.get("/resource", (req, res, ctx) => {
  return res(ctx.json({ id: "abc-123" }));
});
```

**After:**

```jsx
import { http } from "msw";

http.get("/resource", () => {
  return new Response(JSON.stringify({ id: "abc-123" }), {
    headers: {
      "Content-Type": "application/json",
    },
  });
});
```

> Response클래스를 대체하는 드롭인 방식으로 사용할 수 있는 사용자 정의 HttpResponse클래스를 제공한다.

```jsx
import { http, HttpResponse } from "msw";

export const handlers = [
  http.get("/resource", () => {
    return HttpResponse.json({ id: "abc-123" });
  }),
];
```

### req.passthrough()

**Before:**

```jsx
rest.get("/resource", (req, res, ctx) => {
  return req.passthrough();
});
```

**After:**

```jsx
import { http, passthrough } from "msw";

export const handlers = [
  http.get("/resource", () => {
    return passthrough();
  }),
];
```

### res.once()

> 일회성 요청 핸들러를 선언하려면 세 번째 인수로 객체를 제공하고 해당 객체의 일회성 속성을 true로 설정한다.

**Before:**

```jsx
rest.get("/resource", (req, res, ctx) => {
  return res.once(ctx.text("Hello world!"));
});
```

**After:**

```jsx
import { http, HttpResponse } from "msw";

http.get(
  "/resource",
  () => {
    return new HttpResponse("Hello world!");
  },
  { once: true }
);
```

### res.networkError()

> 네트워크 오류를 모의하려면 HttpResponse.error()정적 메서드를 호출하고 응답 확인자에서 반환한다.

**Before:**

```jsx
rest.get("/resource", (req, res, ctx) => {
  return res.networkError("Custom error message");
});
```

**After:**

```jsx
import { http, HttpResponse } from "msw";

http.get("/resource", () => {
  return HttpResponse.error();
});
```

## Context utilities

### ctx.status()

> ctx 유틸리티 객체가 더 이상 사용되지 않는다. 대신 HttpResponse 클래스를 사용하자

**Before:**

```jsx
rest.get("/resource", (req, res, ctx) => {
  return res(ctx.status(201));
});
```

**After:**

```jsx
import { http, HttpResponse } from "msw";

http.get("/resource", () => {
  return new HttpResponse(null, {
    status: 201,
  });
});
```

### ctx.set()

**Before:**

```jsx
rest.get("/resource", (req, res, ctx) => {
  return res(ctx.set("X-Custom-Header", "foo"));
});
```

**After:**

```jsx
import { http, HttpResponse } from "msw";

http.get("/resource", () => {
  return new HttpResponse(null, {
    headers: {
      "X-Custom-Header": "foo",
    },
  });
});
```

### ctx.cookie()

**Before:**

```jsx
rest.get("/resource", (req, res, ctx) => {
  return res(ctx.cookie("token", "abc-123"));
});
```

**After:**

```jsx
import { http, HttpResponse } from "msw";

http.get("/resource", () => {
  return new HttpResponse(null, {
    headers: {
      "Set-Cookie": "token=abc-123",
    },
  });
});
```
