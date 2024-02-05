# Setting

## TypeScript + React + Jest + Parcel 셋팅

### 1. npm 패키지 준비하기

```tsx
npm init -y
```

### 2. `.gitignore`파일 작성

```tsx
touch .gitignore
------
//.gitignore
/node_modules/
/dist/
/.parcel-cache/
```

### 3. 타입스크립트 설치 및 설정

```bash
npm i -D typescript
npx tsc --init
```

### 4. `tsconfig.json` 파일의 jsx 속성 변경

```bash
{
  "compilerOptions": {
    // ...(전략)...
    "jsx": "react-jsx",
    // ...(후략)...
  }
}
```

### 5. ESLint 설치 및 설정

```tsx
npm i -D eslint
npx eslint --init
```

<figure><img src="../dev/1/setting.png" alt=""><figcaption></figcaption></figure>

### 6. `.eslintrc.js` 파일 수정

```tsx
module.exports = {
  env: {
    browser: true,
    es2021: true,
    jest: true,
  },
  // ...(후략)...
};
```

**7. `.eslintignore` 파일 작성**

```tsx
touch .eslintignore
---
//.eslintignore
/node_modules/
/dist/
/.parcel-cache/
```

### 8. 리액트 설치

```tsx
npm i react react-dom
npm i -D @types/react @types/react-dom
```

### 9. 테스팅 도구 설치

```tsx
npm i -D jest @types/jest @swc/core @swc/jest \
    jest-environment-jsdom \
    @testing-library/react @testing-library/jest-dom@5.16.4
```

### 10. jest.config.js 파일 작성

SWC를 이용하여 타입스크립트 코드를 변환 해주어야한다.

```tsx
module.exports = {
  testEnvironment: "jsdom",
  setupFilesAfterEnv: [
    "@testing-library/jest-dom/extend-expect",
    "./jest.setup",
  ],
  transform: {
    "^.+\\.(t|j)sx?$": [
      "@swc/jest",
      {
        jsc: {
          parser: {
            syntax: "typescript",
            jsx: true,
            decorators: true,
          },
          transform: {
            react: {
              runtime: "automatic",
            },
          },
        },
      },
    ],
  },
  testPathIgnorePatterns: ["<rootDir>/node_modules/", "<rootDir>/dist/"],
};
```

### 오류 대비

```tsx
npx eslint --fix .
Warning: React version not specified in eslint-plugin-react settings.
See https://github.com/jsx-eslint/eslint-plugin-react#configuration .
```

eslint-plugin-react 설정에서 React버전이 지정되지 않았음을 나타낸다. \
간단하다. `eslintrc.js`버전을 설정해주면 해결된다.

```tsx
settings: {
    react: {
    version: 'detect', // 사용자가 설치한 버전을 자동으로 선택
    },
  },
```

### 11. Parcel 설치

```tsx
npm i -D parcel
```

### 12. `package.json` - Scripts 설정

```tsx
"source": "./index.html",
"scripts": {
    "start": "parcel --port 8080",
    "build": "parcel build",
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
    "test": "jest",
    "coverage": "jest --coverage --coverage-reporters html",
    "watch:test": "jest --watchAll"
  },
```

### 13. `index.html`

```tsx
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="./src/main.tsx"></script>
  </body>
</html>
```

- `src/main.tsx`

```tsx
import ReactDOM from "react-dom/client";
import App from "./App";

function main() {
  const element = document.getElementById("root");

  if (!element) {
    return;
  }

  const root = ReactDOM.createRoot(element);
  root.render(<App />);
}

main();
```

- `src/App.tsx`

```tsx
import React from "react";
import Greeting from "./components/Greeting";

const App = () => (
  <div>
    <Greeting name={"Hi"}></Greeting>
  </div>
);

export default App;
```

- `src/App.test.tsx`

```tsx
function add(x: number, y: number): number {
  return 1 + 2;
}

test("숫자더하기", () => {
  expect(add(1, 3)).toBe(3);
});
```

- `src/components/Greeting.test.tsx`

```tsx
import { render, screen } from "@testing-library/react";
import Greeting from "./Greeting";

test("Greeting", () => {
  render(<Greeting name="world"></Greeting>);
  screen.getByText(/world/);
});
```

- `src/components/Greeting.tsx`

```tsx
import React from "react";

const Greeting = ({ name }: { name: string }) => <div>Hellow,{name}</div>;

export default Greeting;
```

### 14. Parcel 정적빌드 도구 설치

```tsx
npm install -D parcel-reporter-static-files-copy
```

### 15. Parcel 설정

```tsx
touch .parcelrc
-------
mkdir static/images
```

### 16. ESLint플러그 인 설정

```tsx
mkdir .vscode
touch .vscode/settings.json
-----
//settings.json
{
    "editor.rulers": [
    80
    ],
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": "explicit"
    },
    "trailing-spaces.trimOnSave": true
    }
```
