## Node

**Node.js는 Chrom v8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경(Runtime Environment)으로 주로 서버 사이드 개발에 사용되는 소프트웨어이다.**

Node.js는 Non-blocking I/O와 단일 스레드 이벤트 루프를 통한 높은 Request 처리 성능을 가진다.

### NoN-Blocking ?

**I/O 작업의 개념**   
: I/O 작업은 파일 시스템 접근, 네트워크 통신 , 데이터베이스 쿼리 등과 같이 데이터를 읽고 쓰는 작업을 포함한다. 

**blocking I/O**  
: I/O작업이 완료될 때까지 어플리케이션의 실행을 멈추게 한다.

**Non-blocking I/O**  
: I/O작업이 완료될 때까지 대기하지 않는다. 제어권을 어플리케이션이 가지고 어플리케이션은 계속 동작한다.

3가지의 특징을 조합하여 설명하면  
Node.js는 단일 스레드를 사용하여 스레드 동시성 관리의 부담 없이 자원을 효율적으로 사용 가능하며 단일 스레드의 최대 단점인 한가지의 작업 밖에 할 수 없다는 단점을 극복하기 위해 non-Blocking I/O 모델 방식을 채택하여 확장성에 매우용이 하며 이벤트 루프로 I/O작업을 효율적으로 관리해준다. 하지만 non-Blocking을 처리하기위한 수많은 콜백 때문에 콜백지옥에 빠져 코드관리에 어려움에 빠질수있고, 단일 스레드 이므로 CPU연산작업에는 비효율적이다????????

---
[About Node.js® | Node.js](https://nodejs.org/en/about)  
[비동기와 논블로킹 | devlog.akasai](https://akasai.space/node-js/about_node_js_3/)  
[IBM Developer](https://developer.ibm.com/articles/l-async/)  
[동기 vs 비동기, 블로킹 vs 논블로킹 쉽게 이해하기](https://siyoon210.tistory.com/147)