# Components

## Link

`<a/>`요소를 확장한 버전  
사용자가 애플리케이션 내에서 다른 페이지로 이동할수 있게 해준다.  

### 주요특징

**클라이언트 사이드 네비게이션** : `<Link/>`을 사용하여 이동시 전체 페이지를 다시 재로드 하지 않고 클라이언트 사이드에서 처리된다.

**프리패치** : Next.js는 기본적으로 뷰포트내에 있는 `<Link/>` 컴포넌트에 대한 대상 페이지를 사용자가 링크를 클릭하기 전에 해당 페이지의 코드를 미리 로드함으로써,페이지 전환시간을 단축시킨다.  

### 속성 

`replace` 기본값은 flase이다. 브라우저의 기록 스택에 새 URL을 추가하는 대신 현재기록 상태를 대처한다. 

```jsx
import Link from 'next/link'
 
export default function Page() {
  return (
    <Link href="/dashboard" replace>
      Dashboard
    </Link>
  )
}

```

`scroll` 기본값은 true이다. false인 경우 다음 페이지 이동후 스크롤을 상단으로 초기화 하지 않는다. 

```jsx
import Link from 'next/link'
 
export default function Page() {
  return (
    <Link href="/dashboard" scroll={false}>
      Dashboard
    </Link>
  )
}
```