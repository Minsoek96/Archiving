# SRP(Single Responsibility Principle)

SOLID 원칙의 첫 번째 원칙  
하나의 클래스는 하나의 만 가져야 한다는 원칙

> 책임을 분리하지 않을 경우 하나의 클래스만 변경해도 클래스간의 결합도가 높기 때문에 연쇄적으로 수정을 해야하는 상황이 발생할 수 있다. 하지만 단일 책임의 원칙을 잘지키면 서로간의 결합도가 낮기 때문에 하나의 클래스만 수정하면된다.

> 단 하나의 책임에 의해서만 변경되는것을 의미

## Atomic Design

---

작은 원자부터 시작하여 모든 물질을 생성하는 화학계에서 영감을 얻은 디자인시스템

> Atoms => Molecules => Organisms => Template => page

### 등장배경

Atomic Design 이전에는 container-presentional 패턴을 사용했다. 이 패턴은 뷰와 비즈니스 로직을 분리하는 개념을 적용하여 디자인을 나누었지만, 그 기준이 사람마다 약간씩 달라 의견 조율에 불편함이 있었다. 이런 문제를 해결하고자 더 세부적이고 명확한 규칙이 필요했다.

### Atom(원자)

> atom은 더 이상 분해할 수 없는 기본요소\
> 기본 `HTML element태그` 혹은 글꼴, 애니메이션, 레이아웃과 같이 추상적인 요소도 포함될 수 있다.

### Molecules(분자)

> 여러 원자들이 결합하여 구성된 상대적으로 간단한 UI구성단계  
> 자신의 고유한 특성을 가진다.\
> molecule은 SRP원칙을 지킨다.\
> **EX** : 인풋와 버튼을 결합한 검색폼

### Organisms(생물체)

> 서비스에서 표현될 수 있는 명확한 영역과 특정 컨텍스트를 가진다.\
> 구체적인 컨텍스트를 가지기 때문에 재사용성이 낮아지는 특성을 가진다.\
> **EX** : `header`컨텍스트에 `logo(atom)`, `navigation(molecules)`, `search form(molecules)`

### Templates(템플릿)

> organism들이 어떻게 위치해야 하는지를 정의하는 페이지 레이아웃을 만든다.  
> 여러 개의 `organisms`, `molecules`로 구성할 수 있다.\
> **EX** : 실제 콘텐츠가 없는 page 수준의 스켈레톤

### pages

> 실제 콘텐츠와 데이터를 적용하여 완성된 페이지를 만든다.  
> 사용자에게 제공되는 최종 결과물  
> **EX** : Template에 콘텐츠를 담은 최종적인 UI페이지

---

> Atomic Design 방법론은 디자인과 개발 과정에서의 재사용 가능한 컴포넌트와 패턴의 중요성을 강조한다.

[https://www.howtographql.com/basics/0-introduction/](https://www.howtographql.com/basics/0-introduction/)\
[https://fe-developers.kakaoent.com/2022/220505-how-page-part-use-atomic-design-system/](https://fe-developers.kakaoent.com/2022/220505-how-page-part-use-atomic-design-system/)
