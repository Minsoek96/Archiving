# HTML DOM API 란?

> 웹페이지의 구조를 표현하고 프로그래밍 방식으로 조작할 수 있게 해주는 API

- DOM을 통한 HTML요소에 대한 접근 및 제어 
- 사용자가 입력한 텍스트를 검증하거나, 양식에 기본값을 설정 하는 등의 작업이 가능하다. 
- 웹 페이지에서 콘텐츠 드래그 앤 드롭 
- 브라우저 탐색 기록에 대한 접근 
- Web Storage, Web Workers, Web Socket와 같은 기타 API에 대한 연결 인터페이스 지원  

## DOM의 기본 개념

노드(Node) : DOM에서 모든 것은 노드이다.  
->  Element Node, Attribute Node, Text Node등 다양한 유형이 존재한다.   
트리구조 :  DOM은 문서를 트리 구조로 표현한다.  
-> 요소, 속성, 텍스트 등이 노드로 표현되며 부모 - 자식 관계를 가진다.

## DOM API의 주요기능

노드 조작 : 노드를 생성, 수정, 삭제할 수 있다.  
: `document.createElement()`, `Node.appendChild()`, `Node.removeChild()` 등

이벤트 핸들링 : 웹 페이지에서 발생하는 다양한 이벤트(클릭, 키보드, 입력 등)을 감지하고 대응할 수 있다.  

트리 탐색 : DOM트리를 탐색하고 특정 노드를 찾거나 접근할 수 있다.  
:`getElementById()`, `getElementsByClassName()`, `querySelector()`  

스타일 조작 : DOM을 통해 CSS 스타일을 동적으로 변경할 수 있다.  
:`element.style.backgroundColor` 등 

속성 관리 : 요소의 속성(Attribute)을 조작할 수 있다.  
: `getAttribute()`, `setAttribute()` 등 

### Location  

`Location` 객체는 웹 브라우저에서 현재 문서의 URL에 대한 정보를 제공한다.  

### 주요 속성  

`location.href`

- 현재의 페이지의 전체 URL을 반환하거나  
- 새 URL로 설정한다. 설정 시, 브라우저는 지정된 URL로 이동한다.  

`location.protocol`

- 프로토콜을 반환한다.  

`location.host`

- 호스트 이름과 포트 번호를 포함한 URL 의 호스트 부분을 반환한다.
- EX: `exampl.com:80`

`location.hostname`

- 호스트 이름(URL의 도메인 이름 부분)만을 반환한다.  

`location.port`  

- URL의 포트 번호를 반환한다. 
- 포트가 명시되지 않은 경우 빈 문자열을 반환한다.  
  
`location.pathname`

- URL에서 도메인 이후의 경로를 반환한다.  
- EX: `/path/page.html`  

`location.search`

- URL의 쿼리 스트링을 반환한다.  
- ? 으로 시작한다.  

`location.hash`

- URL의 해시값을 반환한다.  
- #뒤에 오는 부분

`location.origin`

- 프로토콜, 호스트, 포트를 결합한 원점(Origin)을 반환한다.  

### 주요 메서드 

`location.assign(url)`  

- 새로운 URL을 로드한다.  
- 해당 메서드는 링크를 클릭한 것과 유사하다.   

`location.reload(forceReload)`

- 현재 페이지를 다시 로드한다. 
- true인 경우 캐시를 무시하고 서버에서 페이지를 새로 가져온다.  

`location.replace(url)`

- 현재 페이지를 새페이지로 대체한다. 
- 브라우저 히스토리에 페이지가 남지 않아 뒤로 가기가 불가능 하다.  

`location.toString()`

- `laoction.href`와 동일하게 전체 URL을 문자열로 변환한다.  

## Pathname

현재 URL의 경로(path)을 나타내는 속성  
URL이 `http://www.example.com/folder/page.html`라면, `pathname`은 `/folder/page.html`을 반환한다.

