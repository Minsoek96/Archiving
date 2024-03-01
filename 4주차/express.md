# Express

## Express 프레임워크?

***

Express는 "Node.js 기반의 웹 애플리케이션 프레임워크로 개발자들이 빠르고 효율적으로 동적인 웹 서비스와 API를 구축할 수 있도록 설계된 유연하고 확장 가능한 플랫폼

### 역사

Express는 2010년 TJ Holowaychuk에 의해 처음 출시되었다.  
Node.js 자체는 낮은 수준의 API를 제공하며, 여러 기본적인 웹 서버 기능을 직접 구현 해야 한다.\
Express는 Node.js의 복잡성을 추상화하고, 라우팅, 미들웨어, 템플릿 렌더링 등과 같은 고급 기능을 제공하여,\
개발자가 보다 효율적으로 웹 애플리케이션을 구축할 수 있도록 도와준다.

### 특징

라우팅\
: URL 경로와 HTTP 메소드별로 요청을 처리할 수 있는 간결한 API를 제공한다.

```jsx
app.get('/user', function(req, res) {
    res.send('User page');
});

app.post('/user', function(req, res) {
    // 사용자 추가 로직
    res.send('User added');
});
```

미들웨어 지원\
: 요청과 응답 사이에서 실행되는 함수를 사용하여 애플리케이션의 기능을 확장할 수 있다.

```jsx
app.use(express.json()); // 요청 본문을 JSON으로 파싱하는 미들웨어

app.use(function(req, res, next) {
  console.log('Time:', Date.now());
  next(); // 다음 미들웨어 또는 라우터로 제어를 넘깁니다.
});
```

템플릿 엔진\
: Pug, EJS와 같은 다양한 템플릿 엔진을 지원하여 서버 사이드에서 HTML을 동적으로 생성할 수 있다.

오류 처리\
: 개발자가 오류를 쉽게 처리하고 관리할 수 있는 기능을 제공한다.

보안\
: HTTP 헤더의 보안 설정을 쉽게 할 수 있는 기능과 보안 관련 모듈을 지원한다.

## URL 구조

***

URL은 보통 Scheme, Domain Name, Port, Path, Parameter, Anchor 로 이루어진다.

\*\*[https://www.example.com:443/path/to/myfile.html?key1=value1\&key2=value2#SomewhereInTheDocument\*\*](https://www.example.com/path/to/myfile.html?key1=value1\&key2=value2#SomewhereInTheDocument\*\*)

### Scheme(=Protocol)

자원에 접근하는 데 사용되는 프로토콜을 나타낸다.\
http : 클라이언트와 서버 간에 정보를 주고 받기 위한 규약\
https : http의 보안 버전 클라이언트와 서버 간의 모든 통신을 암호화 하여 보안을 강화한 규약

### Domain Name

IP 주소를 사용자가 쉽게 이용할 수 있는 이름으로 변환 해주는 서비스\
`www.example.com`

### Port

포트번호는 서버상의 특정 서비스에 접근하기 위해 사용된다.\
`:80`

### Path

서버 내 자원의 위치를 가르킨다.\
`/path/to/myfile.html`

### Query

(선택사항) ? 시작하여 특정 페이지를 요청할 때 추가적인 정보를 제공한다.\
key-value 형태로 이루어진다.\
`?key1=value&key2=value2`

### Anchor(Fragment)

웹페이지 내의 특정 부분으로 직접 이동할 때 사용된다.\
`#SomewhereInTheDocument`

## REST API

***

> 웹 서비스 간에 데이터를 송수신하기 위한 아키텍처 스타일  
RESTful한 API를 말하며 일련의 특징과 규칙 등을 지키는 API를 일컫는다.

### Uniform  

API에서 자원들은 각각의 독립적인 인터페이스를 가지며 각각의 자원들이 **URI 자원식별**, **표현을 통한 자원조작**, **Self-descriptive messages**, **HATEOAS** 구조를 가지는 것을 말한다.  

#### URI 자원 식별

모든 자원은 고유한 식별자, 즉 (URI)를 통해 식별된다.  
URI는 특정 리소스를 위치시키고 접근하는데 사용된다.  
example.com/users/123 는 특정 사용자를 식별하는 URI

#### 표현을 통한 자원조작(HTTP 메서드를 사용)

HTTP Method를 통해 자원을 조회, 삭제 등 작업을 설명할 수 있는 정보가 담겨야 하는 것을 말한다.

#### Self-descriptive Messages

HTTP Header에 타입을 명시하고 각 메시지(자원)들을 MIME types에 맞춰 표현되어야 한다.
예를 들어 `json`을 반환한다면 `apllication/json`으로 명시해주어야 한다.  
MIME type은 문서, 파일 등의 특성과 형식을 나타내는 표준이다.  
`font/ttf`,`test/plain`,`text/csv`등을 말한다.

#### HATEOAS 구조

하이퍼링크에 따라 다른 페이지를 보여줘야 하며 데이터마다 어떤 URL에서 원했는지 명시 해주어야 하는 것을 말한다.  
보통은 href,link,url 속성 중 하나에 해당 데이터의 URI을 담아서 표기해야 한다.  
(_links 속성에 담기도 한다. 링크를 유추할 수 있는 변수명을 쓰면 된다.)

### Stateless  

각 요청은 독립적이며, 이전 요청의 상태에 의존하지 않는다.  
즉, 서버는 클라이언트의 상태를 저장하지 않는 것을 의미한다.

### Chacheable  

HTTP는 원래 캐싱이 된다. 아무런 로직을 구현하지 않더라도 자동적으로 캐싱이 된다.  
새로고침 하면 304가 뜨면서 원래 있던 js와 css이미지 등을 불러오는 것을 볼 수 있다.  
이는 HTTP 메서드 중 GET에 한정되며 `Cache-Control:max-age=100` 이런 식으로 한정된 시간을 정할 수가 있으며 캐싱된 데이터가 유효한지를 판단하기 위해 `Last-modified` 와 `Etag`  라는 헤더값을 쓴다.  
 `Etag` 전달되는 값에 태그를 붙여서 캐싱되는 자원인지를 확인해주는 것이다.

### Clint-Server 구조 

클라이언트와 서버가 서로 독립적인 구조를 가져야 한다.  
서버는 그저 API를 제공하고 그 API에 맞는 비즈니스 로직을 처리하면 된다.  
마찬가지로 클라이언트에서는 HTTP로 받는 로직만 잘 처리하면 되는 것이다.  

### Layerd System  

계층구조로 나눠져 있는 아키텍처를 뜻한다. WEB기반 서비스를 하면 보통 이러한 시스템을 구축하게 된다.

## REST API의 URI 규칙 

자원을 표기하는 URI의 아래의 6가지 규칙을 적용해야 한다.

1. 동작은 HTTP 메소드로만 해야 하고 urI에 해당 내용이 들어가면 안된다.
    1. 수정 = put, 삭제 = delete, 추가 = post, 조회 = get을 이용해야한다.
    2. /books/delete/1  이렇게 표기하면 안된다.
2. .jpg .png 등 확장자는 표시 하지 말아야 한다.
3. 동사가 아닌 명사로만 표기해야 한다. 유저가 책을 소유한다라는 것을 표현한다면
    1. ‘유저/유저아이디/inclusion/책/책아이디’로 표현하고
    2. /geAllUser식의 동사를 집어 넣으면 안된다.
4. 계층적인 내용을 담고 있어야 한다./집/아파트/전세 이런 식으로 내려가야한다.
5. 대문자가 아닌 소문자로만 쓰며 너무 길 경우 -를 쓴다.
6. HTTP 응답 상태 코드를 적재적소에 활용한다.

```jsx
app.get('/books/')
// 모든 책을 조회한다.
app.post('/books/:booksid')
// 책을 생성한다.
app.put('/books/:booksid')
// 책을 수정한다.
app.get('/books/:booksid')
// 책을 조회한다.
app.put('/users/:userid/books/:booksid');
// 어떤 유저가 특정 책을 빌린다.
app.patch('/users/:userid/books/:booksid');
// 어떤 유저가 특정 책을 빌린다.
```

## HTTP Method(CRUD)

클라이언트가 서버에 요청을 보내는 방법을 정의

CRUD는 Create(생성), Read(읽기), Update(갱신), Delete(삭제)의 약자로, 데이터 관리의 네 가지 기본 기능을 나타낸다.

1. **Create (생성)**:
   * **POST**: 서버에 리소스를 생성하라는 요청을 보낸다
   * 새 게시물을 만들거나 사용자 정보를 서버에 추가할 때 사용된다.
2. **Read (읽기)**:
   * **GET**: 서버로부터 리소스를 검색하라는 요청을 보낸다.
   * 데이터를 조회할 때 사용되며, 데이터를 변경하지 않는 안전한(read-only) 작업에 사용된다.
3. **Update (갱신)**:
   * **PUT**: 서버에 리소스를 갱신하라는 요청
   * 기존 리소스를 새로운 데이터로 완전히 대체한다.
   * **PATCH**: PUT과 유사하지만, 리소스의 일부분만 갱신하라는 요청
   * 즉, 전체 리소스를 대체하는 대신 일부분만 수정한다.
4. **Delete (삭제)**:
   * **DELETE**: 서버에 리소스를 삭제하라는 요청
   * 지정된 리소스를 제거한다.
