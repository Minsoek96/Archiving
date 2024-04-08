# useQuery 

> useQuery는 리액트 쿼리 라이브러리의 핵심 훅 중 하나이며, 데이터를 패치하고 캐싱하고 업데이하는 데 사용된다. 이 훅을 사용하면 컴포넌트에서 직접 데이터 패치를 구현하는 것보다 코드를 더욱 간결하고 효율적으로 만들 수 있다.

## useQuery 옵션

> 다양한 옵션을 설정하여 쿼리 동작을 커스터마이징할 수 있다.

### 필수 옵션

- `queryKey` : 유니크한 쿼리 키, 이 키를 사용하여 캐싱된 데이터를 식별한다.
- `queryFn` : 실제 데이터를 페치하는 함수, 이 함수는 `QueryFunctionContext` 객체를 받으며, `Promise`를 반환해야한다.

### 선택 옵션 

- `enabled` : 쿼리 실행 여부를 설정한다. `false`로 설정하면 쿼리가 자동으로 실행되지 않는다.  

- `networkMode` : 데이터 패치 시 네트워크 연결 상태를 고려하는 방식을 설정한다.
(`online`, `always`, `offlineFirst` 중에서 선택)  

#### Retry

- `retry` : 쿼리 실패 시 재시도 여부와 횟수를 설정한다.
- `retryDelay` : 쿼리 재시도 시 지연 시간을 설정한다.  

#### Fresh

- `staleTime` : 캐싱된 데이터 유효 시간을 설정한다. 이 시간을 초과하면 데이터가 `stale`(오래됨) 상태가 되어 업데이트 된다.
- `gcTime` : 캐싱된 데이터가 사용되지 않을 때 메모리에서 제거되는 시간을 설정한다.  

#### Refetch

- `refetchInterval` : 특정 간격으로 데이터를 자동으로 재요청한다.
- `refetchOnMount` : 컴포넌트 마운트 시 데이터가 `stale` 상태일 경우 재요청 한다.
- `refetchOnWindowFocus` : 윈도우가 활성화될 때 데이터가 `stale` 상태일 경우 재요청 한다.
- `refetchOnReconnect` : 네트워크 연결이 복귀될 때 데이터가 `stale` 상태일 경우 재요청한다.

#### 그외 

- `notifyOnChangeProps` : 컴포넌트가 리렌더링되는 조건을 설정한다. 특정 프로퍼티만 변경되었을 때만 리렌더링하도록 설정할 수 있다.  
- `select` : 캐싱된 데이터를 변환 또는 필터링하는 함수이다.
- `initialData` : 쿼리 실행 전 초기 데이터를 설정한다.
- `initialDataupdatedAt` : 초기 데이터의 업데이트 시간을 설정한다.
- `placeholderData` : 쿼리가 로딩 중일 때 표시할 `placeholder` 데이터를 설정한다.
- `structralSharing` : 쿼리 결과 데이터의 객체 구조를 공유하는 방식을 설정한다.
- `throwOnError` : 쿼리 오류 발생 시 에러 바운더리에게 전파하도록 설정한다.  
- `meta` : 쿼리 캐시 항목에서 저장할 메타 정보이다.  
  
---

### useQuery 반환값 

> useQuery 훅은 객체를 반환하며, 다음과 같은 프로퍼티를 포함한다.

- `status`: 쿼리 상태 (pending, error, success)
- `isPending`: 쿼리가 데이터를 페치하는 중인지 여부 (boolean)
- `isSuccess`: 쿼리가 성공적으로 데이터를 페치했는지 여부 (boolean)
- `isError`: 쿼리에서 오류가 발생 했는지 여부 (boolean)
- `data`: 성공적으로 페치된 데이터
- `dataUpdatedAt`: 데이터가 마지막 업데이트된 시간
- `error`: 쿼리 오류 (실패 시)
- `errorUpdatedAt`: 오류가 발생한 시간
- `isStale`: 캐싱된 데이터가 유효하지 않은지 여부 (boolean)
- `isPlaceholderData`: 현재 표시되고 있는 데이터가 placeholder 데이터인지 여부 (boolean)
- `isFetching`: 데이터를 페치하는 중인지 여부 (boolean)
- `isLoading`: 최초 데이터 페치가 진행 중인지 여부 (boolean)
- `refetch`: 쿼리를 수동으로 재요청하는 함수
  
---

### 사용예시 

```jsx
const { data, isLoading, error } = useQuery('todos', () => {
  return axios.get('/api/todos');
});

if (isLoading) {
  return <div>Loading...</div>;
}

if (error) {
  return <div>Error: {error.message}</div>;
}

return (
  <ul>
    {data.map((todo) => (
      <li key={todo.id}>{todo.title}</li>
    ))}
  </ul>
);
```

## useQuery고급기능 

useQuery훅은 기본적인 기능외에도 고급 기능을 제공한다.

- 변수를 사용하여 쿼리 키를 동적으로 생성할 수 있다.  
(페이지네이션, 무한 스크롤링 등에 유용할듯)
- 폴링 기능을 사용하여 특정 간격으로 데이터를 자동으로 재요청할 수 있다.  
(하루를 기준으로 데이터를 새롭게 리셋하는 경우 유용할듯)
- 서버 측 렌더링 환경에서 `useQuery`훅을 사용할수있다. (SSR)