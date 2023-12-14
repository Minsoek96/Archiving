### **TDD(Test-Driven Development)**

테스트 부터 작성하는 개발 방법  
Red → Green → Refactor  
실패하는 테스트를 만들고 빠르게 통과시킨후 리팩토링 하며 고쳐나간다.

### **BDD(Behavior-Driven Development)**  
테스트 대상의 행동을 묘사하는 방식  

- TDD에서 파생된 개발방법론
- 시나리오 기반
  - 개발자가 아니라도 이해할 수 있을 정도의 시나리오
- **Given**-**When**-**Then**

**Given**: 주어진 상황 (테스트 전제조건)  
**When**: 해당 상황에서의 행동 (지정된 동작)  
**Then**: 원하는 결과 (동작으로 인해 예상되는 결과)

### **Describe-Context-It 패턴**

테스트 대상에 대해 좀 더 섬세하게 설명하는 방식  
**Describe**: 설명할 테스트 대상을 명시  
**Context**: 테스트 대상이 놓인 상황을 설명  
**It**: 테스트 대상의 행동을 설명  

### Jest  

자바스크립트 테스트 프레임워크  
테스트 성능을 극대화하기 위해 자체 프로세스 실행하여 병렬화 동작하고
복잡한 설정 없이 쉽게 사용 가능 하며 바벨 이나 리액트 등 다양한 환경에 사용 가능

### React Testing Library

React 컴포넌트를 테스트 하기 위한 UI테스트에 특화된 라이브러리  
실제 사용자의 경험와 유사한 환경을 만들기 위해 가상 DOM이 아닌 실제 DOM에 적용하는 방식을 사용한다. (E2E 테스트와 유사하게 사용가능)

<aside>
💡 단, “F/E 테스트 = only React 컴포넌트 테스트”가 되는 상황은 최대한 피하는 게 좋다. 본질에 집중하지 못하고 너무 많은 테스트 코드를 작성할 위험이 있다. 유지보수를 돕기 위해 테스트 코드를 작성하는데, 테스트 코드를 잘못 작성하면 오히려 유지보수를 저해할 수 있다.

</aside>

---

https://adjh54.tistory.com/305#3  
[https://kooku0.github.io/blog/프론트엔드에서-테스트코드 짜기/](https://kooku0.github.io/blog/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%EC%97%90%EC%84%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%BD%94%EB%93%9C%20%EC%A7%9C%EA%B8%B0/)  
https://testing-library.com/docs/react-testing-library/intro/
