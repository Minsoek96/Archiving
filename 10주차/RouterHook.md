# 키워드 정리 

## RouterHooks

### useParams

- `useParams` 은 URL의 동적 파라미터를 가져오는 데 사용된다.
- 예를 들어, URL 경로가 `user/:userId`  형태일 때, `useParams` 를 사용하여 `userId` 값을 추출할 수 있다.

```jsx
import { useParams } from 'react-router-dom';

function UserProfile() {
  let { userId } = useParams();

  return <div>User ID: {userId}</div>;
}
```

---

### useSearchParams

- `useSearchParams` 훅은 URL의 쿼리스트링을 읽고 쓰는 데 사용된다.
- 이를 통해 `/search?query=react` 와 같은 쿼리 파라미터를 관리할 수 있다

```jsx
import { useSearchParams } from 'react-router-dom';

function Search() {
  let [searchParams, setSearchParams] = useSearchParams();
  let query = searchParams.get("query");

  function handleSubmit(newQuery) {
    setSearchParams({ query: newQuery }); // URL을 "/search?query=newQuery"로 변경
  }

  return (
    <div>
      <p>Search for: {query}</p> // "Search for: react" 출력
      <button onClick={() => handleSubmit("Redux")}>Search "Redux"</button>
    </div>
  );
}
```

---

### useLocation

- `useLocation` 훅은 현재 페이지의 URL정보에 접근할 때 사용된다.
- 이 훅을 통해 pathname, search, hash등을 포함한 현재 위치의 정보를 얻을 수 있다.
- URL 예시: **`/search?query=react`**

```jsx
import { useLocation } from 'react-router-dom';

function CurrentPageInfo() {
  let location = useLocation();
  return (
    <div>
      <p>Current path is {location.pathname}</p> // "/search" 출력
      <p>Search query is {new URLSearchParams(location.search).get('query')}</p> // "Search query is react" 출력
    </div>
  );
}
```

---

### useNavigate

- `useNavigate` 훅은 다른 경로로 이동하거나 이전 페이지로 돌아갈 때 사용된다.
- 라우트를 변경하거나 현재 라우트 스택을 조작하는 데 사용된다.

```jsx
import { useNavigate } from 'react-router-dom';

function HomeButton() {
  let navigate = useNavigate();

  function handleClick() {
    navigate('/'); // 홈페이지 ("/")로 이동
  }

  return <button onClick={handleClick}>Go to Home</button>;
}
```

---

### useMatch

- `useMatch` 훅은 현재 URL이 주어진 경로 패턴과 일치하는지 여부를 확인할 때 사용된다.
- 조건부 렌더링이나 특정 경로에서만 실행되어야 하는 로직 구현 시 유용하다.
- URL 예시: **`/about`**

```jsx
import { useMatch } from 'react-router-dom';

function CheckActiveRoute() {
  let match = useMatch("/about");

  return <div>{match ? "This is the About page" : "This is not the About page"}</div>;
  // "This is the About page" 출력
}
```

## !!this.name을 쓰는 이유 

`!!this.name`의 사용은 JavaScript에서 흔히 볼 수 있는 패턴이다.  
이 구문의 목적은 `this.name`의 값을 boolean으로 명시적으로 변환하는 것이다.

> JavaScript에서는 `null`, `undefined`, `0`, `""` (빈 문자열), `NaN`, 그리고 `false` 자체를 "falsy" 값으로 간주합니다. 반면, 이외의 모든 값은 "truthy"로 간주한다.

`!!this.name`이렇게 하면 원래의 불리언 값이 반전되어, `this.name`이 비어 있지 않은 경우 `true`, 비어 있는 경우 `false`를 명확하게 반환한다.