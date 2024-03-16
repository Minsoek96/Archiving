# Routing

## Dynamic Routes

> 동적 페이지를 생성하는데 사용된다.

- 정확한 세그먼트 이름을 미리 알지 못하는 경우
- 동적 데이터로 경로를 결정하려는 경우

요청 시 채워지거나 빌드 시 미리 렌더링되는 동적 세그먼트를 사용할 수 있다.

**단일 다이나믹 라우트**  
`pages/posts/[id]`

**중첩 다이나믹 라우트**  
`pages/posts/[id]/[comment]`

## Route Groups

> 폴더 이름을 (괄호)로 묶어 경로 그룹을 만들 수 있다.

여러 경로를 그룹화하여 공통의 레이아웃, 로딩 동작, 데이터 패칭 등을 공유 할 수 있게 하는 기능이다. 그룹별 중복되는 코드를 줄이고, 관련된 경로를 효율적으로 관리할 수 있다. 

```
 ┃ ┣ 📂server
 ┃ ┃ ┣ 📂app
 ┃ ┃ ┃ ┣ 📂(afterLogin)
 ┃ ┃ ┃ ┃ ┗ 📂home
 ┃ ┃ ┃ ┃ ┃ ┣ 📜page.js
 ┃ ┃ ┃ ┃ ┃ ┗ 📜page_client-reference-manifest.js
 ┃ ┃ ┃ ┣ 📂(beforeLogin)
 ┃ ┃ ┃ ┃ ┣ 📂@modal
 ┃ ┃ ┃ ┃ ┃ ┗ 📜page_client-reference-manifest.js
 ┃ ┃ ┃ ┃ ┣ 📜page.js
 ┃ ┃ ┃ ┃ ┗ 📜page_client-reference-manifest.js
```

## Parallel Routes

> 병렬 경로를 사용하는 라우트 기능  
동일한 레이아웃 내에서 여러 페이지를 동시에 또는 조건부로 렌더링할 수 있다.

### @Slot

슬롯이라는 개념이 존재하고, 슬롯은 `@폴더` 규칙을 사용하여 정의할 수 있다.  
슬롯은 공용된 부모 `Layout`에서 props를 받아드리며, `children` prop와 함께 렌더링할 수 있다.

```jsx
export default function Layout({
  children,
  team,
  analytics,
}: {
  children: React.ReactNode
  analytics: React.ReactNode
  team: React.ReactNode
}) {
  return (
    <>
      {children}
      {team}
      {analytics}
    </>
  )
}
```

### Slot === Segment???

슬롯은 경로 세그먼트가 아니며 URL 구조에 영향을 주지 않는다.

- `/dashboard/@analytics/views`의 경우
- URL은 @analytics가 슬롯이기 때문에 `/dashboard/views`가 된다.

### 주의사항

기본적으로 Next.js는 각 슬롯에 대한 활성 상태를 추적한다. 하지만 슬롯 내에 렌더링되는 내용은 네비게이션 유형에 따라 달라진다. 

**소프트 네비게이션** :  클라이언트 측 네비게이션 중에  Next.js는 부분 렌더링을 수행하여 슬롯 내의 서브페이지를 변경하면서, 현재 URL와 일치하지 않더라도 다른 슬롯의 활성 서브페이지를 유지한다. 

**하드 네비게이션** : 전체 페이지 로드 (브라우저 새로고침)후 Next.js는 현재 URL와 일치하지 않는 슬롯에 대한 활성 상태를 결정할 수 없다. 대신 일치하지 않는 슬롯에 대해 default.js파일을 렌더링하거나, default.js 파일이 없는 경우 404를 표시한다.

### `default.js`

> 초기 로드나 전체 페이지를 다시 로드할 때 일치하지 않는 슬롯에 대한 대체물로 렌더링할 수 있다.

`/dashboard/settings` 로 네비게이션할 때, `@team` 슬롯은 `/settings` 페이지를 렌더링하면서 `@analytics` 슬롯에 대해 현재 활성화된 페이지를 유지한다.

**새로 고침을 하는 경우**  
Next.js는 `@analytics` 에 대해 `default.js` 를 렌더링한다. 
만약 `default.js` 가 존재 하지 않는다면, **대신 404 페이지가 렌더링**된다.

## Intercepting Routes

현재 표기 되는 페이지의 레이아웃을 그대로 사용하면서 화면 상에 표현되는 URL에 따라 추가적인 뷰를 그릴 수 있게 한다.

새로고침 시에는 가로채기각 발생하지 않는다.

### Convention

인터셉팅은 `../`와 유사한 `(..)` 규칙을 사용하여 정의 될 수 있다.

- (.)는 같은 레벨의 세그먼트와 일치시키기 위해
- (..)는 한 레벨 위의 세그먼트와 일치시키기 위해
- (..)(..)는 두 레벨 위의 세그먼트와 일치시키기 위해
- (...)는 루트 앱 디렉토리부터의 세그먼트와 일치시키기 위해