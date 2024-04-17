# Setting

## Install

### React Query 설치하기

```jsx
npm install @tanstack/react-query
# 또는
yarn add @tanstack/react-query
```

### React Query Devtools 설치하기

```jsx
npm install @tanstack/react-query-devtools
# 또는
yarn add @tanstack/react-query-devtools
```
  
---

## QueryClient

> React Query에서 데이터 캐싱, 동기화 및 업데이트 관리를 담당하는 객체 

`ReactQuery`의 모든 기능을 설정하고 실행할 수 있다.
`QueryClient` 인스턴스는 애플리케이션 내에서 서버 상태를 관리하는데 필수적인 역할을 한다.

```jsx
import { QueryClient } from "@tanstack/react-query";

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: Infinity,
      // ...
    },
  },
});
```
  
---

### DefaultOptions

> React Query가 데이터 처리하는 방식을 전역적으로 커스텀 마이징 할 수 있게 해준다.

### `Queries`

- `cacheTime` 캐시에 데이터가 유지되는 기본 시간이다. (밀리초)
- `staleTime` 데이터가 신선한 것으로 간주되는 시간이다.
- `queryKeyHashFn` 쿼리 키를 해시하는 함수를 정의할 수 있으며, 이를 통해 내부적으로 쿼리 키의 일관성을 관리한다.
- `refetchOnWindowFocus` 윈도우가 다시 포커스 될 때 자동으로 쿼리를 새로 고칠지 여부를 결정한다.
- `retry` 쿼리가 실패했을 때 재시도할 횟수이다.
- `retryDelay` 재시도 사이의 지연시간을 설정한다.

### `Mutations`

- `onMutate`  mutation이 실행되기 전에 호출될 함수
- `onSucess` mutation이 성공적으로 완료된 후에 호출될 함수
- `onError`  mutation이 실패했을 때 호출될 함수
- `onSettled` mutation이 성공하든 실패하든 최종적으로 호출될 함수
  
---

## QueryClientProvider

>ReactQuery의 기능을 전역적으로 사용할 수 있도록 해주는 역할  
`queryClient` 인스턴스를 컴포넌트 트리에 주입한다.

```jsx
'use client';

import React, { useState } from 'react';

import { QueryClientProvider, QueryClient } from '@tanstack/react-query';

// eslint-disable-next-line import/no-extraneous-dependencies
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

type Props = {
  children: React.ReactNode;
};

function RQProvider({ children }: Props) {
  const [client] = useState(
    new QueryClient({
      defaultOptions: { // react-query 전역 설정
        queries: {
          refetchOnWindowFocus: false,
          retryOnMount: true,
          refetchOnReconnect: false,
          retry: false,
        },
      },
    }),
  );

  return (
    <QueryClientProvider client={client}>
      {children}
      <ReactQueryDevtools initialIsOpen={process.env.NEXT_PUBLIC_MODE === 'local'} />
    </QueryClientProvider>
  );
}

export default RQProvider;
```
