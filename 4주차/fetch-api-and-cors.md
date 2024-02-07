# Fetch API & CORS

## FetchAPI

***

웹 브라우저에서 리소스를 비동기적으로 가져오는 기능을 제공하는 JavaScript인터페이스

### 역사 및 필요성

기존에는 `XMLHttpRequest` 가 웹에서 서버와의 비동기 통신을 처리하는 표준 방법이었다. 하지만 사용방식도 복잡하고 콜백을 통한 처리 때문에 코드에 가독성도 많이 떨어졌다. `fetchAPI`는 쉽게 비동기 통신을 처리 할 수 있고 `Promise`기반이라 코드의 가독성 또한 높아졌다.

### 특징

> **Promise 기반** : fetchAPI는 자바스크립트의 Promise를 반환한다.\
> **유연성** : 다양한 유형의 요청과 데이터 포맷을 지원하며, 헤더, 쿼리, 매개변수 캐시 전략등을 설정할 수 있다.\
> **CORS** : CORS에 대한 처리를 지원한다.\
> **스트림 응답** : 결과를 스트림으로 처리할 수 있어 큰 데이터를 처리하는 경우 유용하다.

## Promise

***

javaScript에서 Promise는 비동기 연산의 결과를 나타내는 객체\
Promise는 비동기 작업이 성공적으로 끝났을 때 결과 값을 처리하거나, 실패했을 때 오류를 처리하는 방법을 제공한다.

### Promise의 등장배경

이전에는 주로 콜백 함수를 사용하여 비동기 작업을 처리했지만, 복잡한 작업에서 콜백 지옥이 형성 되는 문제를 해결하고자 `Promise`가 도입되었다.

### Promise의 특징

상태 : Promise는 세 가지 상태를 가진다.

* Pending(대기 중) : 초기 상태, 비동기 처리가 아직 완료되지 않음
* Fulfilled(이행됨) : 연산이 성공적으로 완료됨
* Rejected(거부됨) : 연산이 실패함

**불변성** : 한번 fullfilled 나 Rejected 같은 상태가 되면, 그 상태로 고정된다.

**체이닝** : Promise는`.then()`,`.catch()`, `.finally()`메소드를 사용하여 연속적인 비동기 작업을 쉽게 처리할 수 있다.

### 사용 방법

생성 : 새 Promise는 `new Promise()`생성자와 함께 만들어진다.\
생성자는 `resolve`와 `rejected`라는 두 개의 인수를 가진 함수를 받는다.

```javascript
let promise = new Promise((resolve, rejeted) => {
    if(/*성공적인 처리 조건 */) {
        resolve('Sucess!');
    } else {
        rejected('Fuilure');
    }
    });
```

## ReableStream

***

데이터를 조각별로 읽을 수 있는 인터페이스\
데이터를 메모리에 한 번에 모두 로드하지 않고 조금씩 읽을 수 있어, 큰 크기의 데이터를 효율적으로 처리할 수 있다.

```javascript
fetch("http://localhost:3000/products");
// → Promise

await fetch("http://localhost:3000/products");
// → Response

const response = await fetch("http://localhost:3000/products");
// → response.body는 ReadableStream

const reader = response.body.getReader();

const chunk = await reader.read();
// → chunk.value는 Uint8Array 타입.
// → 원래는 chunk.done이 true일 때까지 반복해야 한다.

const body = new TextDecoder().decode(chunk.value);

const data = JSON.parse(body);
```

## Unicode

***

유니코드(Unicode)는 전 세계의 모든 문자를 공통된 코드로 표현하기 위해 만들어진 국제 표준이다.

> 소프트웨어와 시스템이 텍스트 데이터를 교환하고 처리하는 표준방법

## CORS란

***

교차 출처 리소스 공유\
출처를 구성하는 세 요소는 프로토콜 - 도메인(호스트이름)-포트로( Origin),\
이중 하나라도 다르면 CORS 에러를 만나게 된다.

<figure><img src="../.gitbook/assets/cors.png" alt=""><figcaption></figcaption></figure>

CORS를 설정한다는 건 출처가 다른 서버 간의 리소스 공유를 허용한다는것\
SOP가 서로 다른 출처일 때 리소스 요청과 응답을 차단하는 정책

> 우리를 머리 아프게 하는 정책은 SOP정책이고 CORS는 SOP의 제한을 안전한 방식으로 완화할 수 있는 정책이다.

### 작동방식

1. 브라우저는 먼저 리소스가 있는 서버에 사전 요청(pre-flight request)을 보낸다.
2. 해당 요청은 서버가 교차 출처 요청을 허용하는지 확인하는데 사용된다.
3. 서버가 적절한 CORS헤더로 응답하면 브라우저는 실제 요청을 보내고 리소스에 접근할 수 있다.

CORS는 주로 HTTP헤더를 사용하여 교차 출처 요청의 허용 여부를 전달한다. `Access-Control-Allow-Origin` 헤더는 특정 출처에서의 요청을 허용하거나 모든 출처를 허용하는 등의 설정을 할 수 있다.

브라우저는 기본적으로 SOP정책을 도입하여 서로 다른 출처의 리소스의 접근을 제한하여 악의적인 사용자가 XSS와 CSRF 같은 웹 공격을 방지합니다. CORS라는 예외 정책을 통해 신뢰할 수 있는 출처의 리소스의 접근을 허용할 수 있습니다. CORS 정책은 프리플라이트 라는 사전 요청을 보내 해당 서버가 `Acess-Control-Allow-Origin` 헤더로 적절한 응답을 할 경우에만 본 요청을 수행합니다.
