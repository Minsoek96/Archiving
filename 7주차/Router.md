# Router란 ? 

**사전적 의미**   
> 네트워크와 네트워크 간의 경로(Route)를 설정하고 가장 빠른길로 트래픽을 이끌어주는 네트워크 장비  

## React Router  

> React 기반 웹 애플리케이션에서 사용되는 라우팅 라이브러리  
> SPA의 라우팅을 관리하는 목적으로 사용된다.  

### 주요특징  

**선언적 라우팅** : JSX를 사용하여 라우팅을 선언적으로 구현한다.  
**컴포넌트 기반** : 모든 것이 컴포넌트로 구성되어 있다.  
**동적 라우팅** : 라이프 사이클에 따라 라우트를 동적으로 변경할 수 있다.  
**중첩 라우트** : 라우트 내에 많은 라우트를 중첩하여 복잡한 구조를 관리한다.  


## Browser Router  

HTML5의 History API를 사용하여 URL 변경을 관리한다.  
-> `pushState`, `replaceState`, `popstate` 등  

`<BrowserRouter>`는 URL을 사용하여 브라우저의 주소창에 현재 위치를 저장하고 브라우저에 내장된 history 스택을 탐색한다. 
  
## Route  

URL와 React컴포넌트를 연결하는 역할  
즉, 사용자의 현재위치(URL)에 따라 어떤 컴포넌트를 렌더링할지 결정한다.  

### Router의 기본 작동 원리  

path Matching :  

- `path` prop을 받아 현재 URL와 비교한다.  
- 만약 URL이 `Route`의  `path`와 일치한다면 `Route`의 `component`나 `render` prop에 정의된 컴포넌트가 렌더링된다.  

Rendering  

### 주요속성  

**path** : URL와 매칭될 경로를 지정한다.  
**component** : 렌더링할 컴포넌트를 지정한다.  
**render** : 렌더링할 함수형 컴포넌트를 정의한다.(인라인 렌더링 로직 유용)  
**exact** : `true`로 설정되면 경로가 정확히 일치할 때만 해당 `Route`가 렌더링 된다.  
**children** : 항상 렌더링되는 컴포넌트를 정의한다.(매칭 X시에도)  

## Memory Router

`<MemoryRouter>`는 내부에서 배열로 위치를 관리한다. History 스택과 같이 외부 소스에 연결되지 않는다. 따라서 테스트와 같이 history 스택을 완벽하게 제어해야 하는 시나리오에 이상적이다.  

`<MemoryRouter initialEntries>`의 기본값은 ["/"](루트/URL에 있는 단일 항목)이다.  
`<MemoryRouter initialIndex>`는 초기 항목의 마지막 인덱스를 기본값으로 한다.  