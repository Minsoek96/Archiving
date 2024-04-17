# QueryClient

## queryClient.fetchQuery

> `fetchQuery`는 비동기 메서드로 쿼리를 가져오고 캐싱하는데 사용된다. 결과가 필요하지 않고 쿼리를 가져오기만 하려면 `prefetchQuery` 메서드를 사용하자

### 캐싱 및 업데이트 

- 기존 쿼리가 있고 데이터가 유효하며 지정된 `staleTime`보다 오래되지 않았다면 캐싱된 데이터가 반환된다.
- 그렇지 않은 경우 최신 데이터를 가져온다.

### `fetchQuery` vs `setQueryData`

- `fetchQuery`는 비동기이며 동일한 쿼리에 대한 중복 요청을 생성하지 않도록 보장한다. 캐싱된 데이터를 직접 업데이트하는 `setQueryData`와 대조된다.
- `fetchQuery`는 에러 처리를 위한 `try...catch`블럭을 사용해야 한다. `setQueryData`는 에러를 발생시키지 않는다.

### 사용예시

```tsx
try {
  const data = await queryClient.fetchQuery({ queryKey, queryFn })
  // 가져온 데이터 사용
} catch (error) {
  console.log(error) // 에러 처리
}
```

`staleTime`옵션  
지정된 시간보다 오래된 경우에만 데이터를 다시 가져온다.

```tsx
try {
  const data = await queryClient.fetchQuery({
    queryKey,
    queryFn,
    staleTime: 10000, // 10초
  })
} catch (error) {
  console.log(error)
}
```

### Option

| 옵션 | 설명 | 기본값 |
| --- | --- | --- |
| queryKey | 쿼리 키 | 필수 |
| queryFn | 쿼리 함수 | 필수 |
| staleTime | 데이터 유효 시간 (밀리초) | 무한대 |
| variables | 쿼리 변수 | 없음 |
| meta | 메타 데이터 | 없음 |
| initialData | 초기 데이터 | 없음 |
| onSuccess | 성공 콜백 | 없음 |
| onError | 에러 콜백 | 없음 |
| refetchOnMount | 컴포넌트 렌더링 시 다시 가져오기 | false |
| refetchInterval | 다시 가져오기 간격 (밀리초) | 무한대 |
| refetchIntervalInBackground | 백그라운드에서 다시 가져오기 | true |
| refetchOnWindowFocus | 윈도우 포커스 시 다시 가져오기 | false |
| refetchOnReconnect | 네트워크 연결 복구 시 다시 가져오기 | true |
| keepPreviousData | 이전 데이터 유지 | false |
| first | 첫 번째 페이지만 가져오기 | false |
| pageSize | 페이지 크기 | 없음 |
| useInfiniteCache | 무한 캐싱 사용 | false |
| batchSize | 배치 크기 (무한 캐싱 시 사용) | 없음 |
| retry | 재시도 옵션 | { maxRetries: 0, delay: 0 } |
| retryOnMount | 컴포넌트 렌더링 시 재시도 | false |
| retryOnWindowFocus | 윈도우 포커스 시 재시도 | false |
| retryOnReconnect | 네트워크 연결 복구 시 재시도 | false |

### useQuery와 동일한 옵션 

`fetchQuery`는 `useQuery`와 동일한 옵션을 대부분 지원한다. 다만, 다음 옵션은 사용할 수 없다.

- `enabled`
- `notifyOnChangeProps`
- `select`
- `suspense`
- `placeholderData`

**반환값**  

`Promise<TData>`: 쿼리 결과 데이터를 포함하는 약속 객체


## queryClient.prefetchQuery

`prefetchQuery` 메서드는 `useQuery`와 같은 훅을 이용하여 데이터를 가져올 필요가 생기기 전에 미리 쿼리를 페치하는 데 사용된다. 이 메서드는 `fetchQuery`와 동일하게 작동하지만 예외를 발생시키거나 데이터를 반환하지 않는다.

### 사용 방법

```jsx
import { queryClient } from '@tanstack/react-query';

// 쿼리 키와 쿼리 함수를 제공
await queryClient.prefetchQuery({ queryKey, queryFn });

```

- `queryKey`는 고유한 쿼리 식별자
- `queryFn` 는 실제 데이터를 가져오는 함수

### 옵션

`prefetchQuery` 옵션은 `fetchQuery` 옵션이랑 정확히 동일하다.

**반환 값**  
`Promise<void>`
    - 페치가 필요 없는 경우 즉시 해결되거나 쿼리 실행 후 해결되는 Promise 객체를 반환한다. 데이터를 반환하거나 에러를 발생시키지 않는다.