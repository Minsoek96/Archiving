# Using Styled-Components with Next.js v13 (TypeScript)

## Install Next.js (with TypeScript)

```bash
npm install styled-components @types/styled-components
```

---

## Add SSR support

Next.js는 서버에서 컴포넌트를 렌더링하고 클라이언트에서 `hydrates`(인터랙티브한 부분을 추가)한다. Next.js와 `styled-component`를 사용할 때, 스타일은 클라이언트에서 적용되기 때문에 서버에서 첫 렌더링은 어떠한 스타일도 없이 이루어지며 스타일이 적용되기 전에 눈에 띄는 지연이 발생할 것이다.

Next.js와 `styled-compoent`에 SSR지원을 추가하는 몇 가지 방법이 있다.

### A. next.config.mjs에서 styled-component 활성화하기

> next.config.mjs파일을 편집하고 다음을 추가하기만 하면 된다.

```jsx
const nextConfig = {
  compiler: {
    styledComponents: true,
  ...
  },
};
```

---

### B. Global Style registry

> Next.js는 모든 스타일을 수집하고 이를 `<head>`태그에 적용하는 전역 스타일 레지스트리 컴포넌트를 구현할 것을 제안한다.

`/lib/registry.tsx` 파일을 생성하고 다음을 추가

```jsx
"use client";

import React, { useState } from "react";
import { useServerInsertedHTML } from "next/navigation";
import { ServerStyleSheet, StyleSheetManager } from "styled-components";

export default function StyledComponentsRegistry({
  children,
}: {
  children: React.ReactNode,
}) {
  // Only create stylesheet once with lazy initial state
  // x-ref: https://reactjs.org/docs/hooks-reference.html#lazy-initial-state
  const [styledComponentsStyleSheet] = useState(() => new ServerStyleSheet());

  useServerInsertedHTML(() => {
    const styles = styledComponentsStyleSheet.getStyleElement();
    styledComponentsStyleSheet.instance.clearTag();
    return <>{styles}</>;
  });

  if (typeof window !== "undefined") return <>{children}</>;

  return (
    <StyleSheetManager sheet={styledComponentsStyleSheet.instance}>
      {children}
    </StyleSheetManager>
  );
}
```

루트 layout.tsx 파일에서 styledCompoentsRegistry를 import하기

```tsx
import StyledComponentsRegistry from "./lib/registry";

export default function RootLayout(props: React.PropsWithChildren) {
  return (
    <html>
      <body>
        <StyledComponentsRegistry>{props.children}</StyledComponentsRegistry>
      </body>
    </html>
  );
}
```

---

### C. The `Styled-compoents` Babel plugin

> `styled-components`는 `SSR` 지원을 추가하기 위해 사용할 수 있는 Babel 플러그인을 제공한다. 이를 사용하려면:

- **Install it using the following command:**

```jsx
npm i -D babel-plugin-styled-components
```

- **Create the `.babelrc` file in the root of your project and add the following to it:**

{
"presets": ["next/babel"],
"plugins": ["babel-plugin-styled-components"]
}

이제 사용자 지정 설정이 있으므로 Next.js는 SWC대신 Babel을 사용해 코드를 컴파일 하게 된다.

그러나, 이 방법이 작동하지 않는 경우도 있다. 예를 들어 next/font는 SWC를 필요로 하므로 사용자 지정 .babelrc 설정 파일을 함께 사용할 경우 다음과 같은 오류 메시지가 나타날 수 있다.

```jsx
Syntax error: "next/font" requires SWC although Babel is being used
due to a custom babel config being present.
```

---

### D. The `Styled-components` SWC plugin

styled-components SWC 플러그인도 사용할 수 있다.

- **Install it using the following command:**  
  `npm i -D @swc/plugin-styled-components`

- **Create the `.swcrc` file in the root of your project and add the following to it:**

```jsx
{
  "jsc": {
    "experimental": {
      "plugins": [
        [
          "@swc/plugin-styled-components",
          {
            "ssr": true
          }
        ]
      ]
    }
  }
}

```
