# WWW(웹)을 이용할 때는 이렇게 데이터를 주고 받는다.

## HTTP

Http는 웹에서 클라이언트와 서버 간의 데이터를 주고받기 위한 프로토콜로 주요 특징은 요청(Request)와 응답(Response)라는 구조를 기반으로 한다. HTTP는 1.0버전 부터 시작하여 계속 개선되어 왔다

### HTTP 1.0의 특징와 문제점

![http1.0](/dev/network/http_10.png)

**특징** : HTTP 1.0에서는 한번의 요청마다 새로운 TCP 연결을 맺고 응답 후에는 연결을 끊는다. 하나의 URL에 대해 하나의 TCP 연결이 사용되었다.

**문제점** : 이 단순성으로 인해, 많은 양의 데이터를 전송할 경우 성능 문제가 발생할 수 있다. 예를 들어 하나의 웹페이지에 여러 리소스(이미지, css파일등)이 포함되어 있다면, 각 리소스에 대해 새로운 연결을 맺고 끊어야 하기 때문에 네트워크 대기 시간이 크게 증가하는 원인이 된다.

### HTTP 1.1의

![http1.1](/dev/network/http_11.png)

- **지속 연결(Persistent Connection)**
  - HTTP 1.1에서는 기본적으로 **지속 연결(Keep-Alive)**이 활성화되어 있다. 이는 하나의 TCP 연결을 통해 여러 요청과 응답을 주고받을 수 있다는 의미이다.
  - HTTP 1.1에서는 한 번 연결한 TCP세션을 유지해 여러 리소스를 한꺼번에 요청할 수 있다. 이를 통해 연결을 재사용하여 오버헤드를 줄여 성능을 향상시킨다.

### HTTP요청 구조

- **Request Line(요청라인)**

  - 요청의 첫 번째 줄로, 클라이언트가 서버에 어떤 작업을 요청하는지 나타냄
    - **Method** : 클라이언트가 서버에서 수행하고자 하는 작업의 종류를 나타냄. 예를 들어, `GET`, `POST`, `PUT`, `DELETE` 등이 있다.
    - **URI(Uniform Resource Identifier)** : 요청 대상 리소스의 경로를 나타냄. 예를 들어 `/index.html`
    - **HTTP Version** : 사용 중인 HTTP의 버전

- **Headers**

  - 요청에 대한 추가 정보를 제공하며, 각 헤더는 키: 값 형식으로 나타난다.
    - **Host** : 요청을 받는 서버의 호스트 이름
    - **User-Agent** : 요청을 보낸 클라이언트의 정보
    - **Accept** : 클라이언트가 수락할 수 있는 미디어 타입 (`text/html, application/json`)
    - **Content-Type** : Body에 포함된 데이터의 형식을 나타냄 (`application/json, multipart/form-data`)
    - **Authorization** : 인증 정보( `Authorization : Bearer <token>`)

- **빈 줄(CRLF)**

  - 헤더와 Body 사이의 구분을 위한 빈줄
  - HTTP 요청에서 헤더와 본문을 구분하기 위해 반드시 한줄의 빈줄이 들어가야 한다.

- **Body**
  - 요청의 실제 데이터를 담고 있는 부분으로, 주로 `POST`, `PUT`와 같은 요청에서 사용된다.
    - **JSON 데이터** : `application/json` 형식으로 전송됨
    - **HTML Form데이터** : `application/x-www-form-urlencoded` 또는 `multipart/form-data` 형식으로 전송됨

### MIME 타입

HTTP는 다양한 종류의 데이터 전송할 수 있으며, 이를 정의하는 것이 MIME 타입이다. 예를 들어 텍스트, 이미지, 비디오 등의 파일 형식을 MIME로 정의하여 전송할 수 있다.

### URI(Uniform Resource Identifier)

URI는 웹에서 리소스의 위치를 나타내는 문자열이다.

```bash
scheme://host/path?query#fragment
```

- scheme: 프로토콜을 의미하며, 예를 들어 http, https, ftp 등이 있다.
- host: 주로 도메인 이름이나 IP 주소, 그리고 포트 번호를 포함한다.
- path: 서버 내에서의 자원의 경로를 의미한다.
- query: 클라이언트가 서버로 보낼 추가적인 데이터를 포함한다.
- fragment: 특정 리소스 내에서의 위치를 가리키는 것으로, 예를 들어 웹페이지 내의 특정 섹션을 의미할 수 있다.
