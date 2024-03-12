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

## mutate

> 데이터를 강제로 변경하고 재검증(revalidation)없이 캐시를 업데이트할 수 있는 기능  

> 모든 키를 변경할 수 있는 `global mutate API`와 해당 SWR hook의 데이터만 변경할 수 있는 `bound mutate API`가 있다.

### Global Mutate

> `useSWRConfig` 훅은 SWR의 글로벌 설정와 유틸리티 함수에 접근할 수 있게 해준다.  
`useSWRConfig`를 사용하면 컴포넌트 내에서 글로벌 `mutate`함수를 가져와서 사용할 수 있다.

```jsx
import { useSWRConfig } from 'swr'

function Component() {
  const { mutate } = useSWRConfig()

  // 글로벌 mutate 사용 예시
  const updateGlobalData = async () => {
    const newData = { /* 새로운 데이터 */ }
    await mutate('/api/data', newData, true)
  }

  return (
    // UI 구성
  )
}

```

`mutate` 함수가 SWR의 글로벌 캐시와 연동되어 작동하게 된다. 어느 곳에서든 동일한 키로 패칭된 데이터를 업데이트할 수 있게 해주며, 데이터의 일관성와 동기화를 보다 쉽게 관리할 수 있게된다.

```jsx
import { mutate } from "swr"
 
function App() {
  mutate(key, data, options)
}
// 전역으로 가져올 수도 있음
```

### Bound Mutate

> `Bound mutate`는 현재 키를 기반으로 데이터로 변경하는 빠른 방법이다.  
`Global mutate`함수와 기능적으로 동일하지만 `key` 매개변수가 필요하지 않다.

```jsx
import useSWR from 'swr'
 
function Profile () {
  const { data, mutate } = useSWR('/api/user', fetcher)
 
  return (
    <div>
      <h1>My name is {data.name}.</h1>
      <button onClick={async () => {
        const newName = data.name.toUpperCase()
        // API에 대한 요청을 종료하여 데이터를 업데이트 합니다.
        await requestUpdateUsername(newName)
        // 로컬 데이터를 즉시 업데이트 하고 다시 유효성 검사(refetch)를 합니다.
        // NOTE: key는 미리 바인딩되어 있으므로 useSWR의 mutate를 사용할 때 필요하지 않습니다.
        mutate({ ...data, name: newName })
      }}>Uppercase my name!</button>
    </div>
  )
}
```