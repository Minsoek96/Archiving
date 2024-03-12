# SWR

`SWR`이라는 이름은 HTTP 캐시 무효 전략인 stale-while-revalidate에서 유래되었다.

## stale-while-revalidate????

> HTTP 캐싱 전략중 하나로, 캐시된 데이터를 사용자에게 즉시 제공하면서 백그라운드에서 최신 데이터로 업데이트 하는 캐싱 전략

### 처리 과정

1. 사용자가 데이터를 요청하면 최신 캐싱 데이터를 반환한다.
2. 동시에, 백그라운드에서 최신 데이터를 서버에 요청한다.
3. 이전 캐시된 데이터를 최신 데이터로 교체한다.
4. 새로운 최신화된 캐싱 데이터가 제공된다.

## SWR 상세

> SWR을 사용하면 컴포넌트는 지속적이며 자동으로 데이터 업데이트 스트림을 받게 된다.  
> 그리고 UI는 항상 빠르고 반응적이다.

```jsx
import useSWR from "swr";

function Profile() {
  const { data, error, isLoading } = useSWR("/api/user", fetcher);

  if (error) return <div>failed to load</div>;
  if (isLoading) return <div>loading...</div>;
  return <div>hello {data.name}!</div>;
}
```

`useSWR` hook은 key 문자열과 fetcher 함수를 받는다.

- key는 데이터의 고유한 식별자(일반적으로 API URL)이며 `fetcher`로 전달될 것
- hook은 `data`, `isLoading` ,`error`를 반환한다.

## SWR 주요 기능

- 내장된 캐시 및 요청 중복 제거 
- 실시간 데이터 
- SSR/ISR/SSG 지원 
- 포커스시 재검증
- 네트워크 회복시 재검증
- 뮤테이션
- 에러 재시도 