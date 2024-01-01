# Express

## Express 프레임워크

Express는 "Node.js 기반의 웹 애플리케이션 프레임워크로 개발자들이 빠르고 효율적으로 동적인 웹 서비스와 API를 구축할 수 있도록 설계된 유연하고 확장 가능한 플랫폼

#### 역사

Express는 2010년 TJ Holowaychuk에 의해 처음 출시되었다.

Node.js 자체는 낮은 수준의 API를 제공하며, 여러 기본적인 웹 서버 기능을 직접 구현 해야 한다. Express는 이러한 복잡성을 추상화하고, 라우팅, 미들웨어, 템플릿 렌더링 등과 같은 고급 기능을 제공하여, 개발자가 보다 효율적으로 웹 애플리케이션을 구축할 수 있도록 도와준다.

#### 특징:

**라우팅**: URL 경로와 HTTP 메소드별로 요청을 처리할 수 있는 간결한 API를 제공한다.\
**미들웨어 지원**: 요청과 응답 사이에서 실행되는 함수를 사용하여 애플리케이션의 기능을 확장할 수 있다.\
**템플릿 엔진**: Pug, EJS와 같은 다양한 템플릿 엔진을 지원하여 서버 사이드에서 HTML을 동적으로 생성할 수 있다.\
**오류 처리**: 개발자가 오류를 쉽게 처리하고 관리할 수 있는 기능을 제공한다.\
**보안**: HTTP 헤더의 보안 설정을 쉽게 할 수 있는 기능과 보안 관련 모듈을 지원한다.

### URL 구조

URL은 보통 Scheme, Domain Name, Port, Path, Parameter, Anchor 로 이루어진다.

\*\*[https://www.example.com:443/path/to/myfile.html?key1=value1\&key2=value2#SomewhereInTheDocument\*\*](https://www.example.com/path/to/myfile.html?key1=value1\&key2=value2#SomewhereInTheDocument\*\*)

#### Scheme(=Protocol)

자원에 접근하는 데 사용되는 프로토콜을 나타낸다.

http : 클라이언트와 서버 간에 정보를 주고 받기 위한 규약

https : http의 보안 버전 클라이언트와 서버 간의 모든 통신을 암호화 하여 보안을 강화한 규약

#### Domain Name

IP 주소를 사용자가 쉽게 이용할 수 있는 이름으로 변환 해주는 서비스

`www.example.com`

#### Port

포트번호는 서버상의 특정 서비스에 접근하기 위해 사용된다.

`:80`

#### Path

서버 내 자원의 위치를 가르킨다.

`/path/to/myfile.html`

#### Query

(선택사항) ? 시작하여 특정 페이지를 요청할 때 추가적인 정보를 제공한다.

key-value 형태로 이루어진다.

`?key1=value&key2=value2`

#### Anchor(Fragment)

웹페이지 내의 특정 부분으로 직접 이동할 때 사용된다.

`#SomewhereInTheDocument`

### REST API

### HTTP Method(CRUD)

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
