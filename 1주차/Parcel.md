### SWC(Speedy Web Compiler)

SWC는 자바스크립트/타입스크립트에 특화된 컴파일과 번들링 도구이다.  
Rust는 병렬 처리를 고려하여 설계된 언어 이므로 이벤트 루프 기반의 단일 스레드인 자바스크립트보다 빠르다.

<aside>
💡 SWC는 단일 스레드에서는 바벨보다 20배, 4개 코어에서는 70배 더 빠르다.

</aside>

## Parcel

빠른속도와 복잡한 설정이 필요 없는 번들 도구

<aside>
💡 Parcel은 캐싱을 하여 첫번째 빌드 보다 두 번째 빌드 속도부터 불꽃튀게 빠르다.

</aside>

### ESLint  

ES: ECMAScript(자바스크립트 표준)  
Lint : 소스 코드를 분석하여 오류, 버그, 스타일 문제등을 식별하고 표시하는 도구  
자바스크립트코드에 대한 정적 분석 도구

**스타일 통일**  
: 프로젝트의 전체 코드 스타일을 일관되게 유지  
**잠재적 문제 발견**  
: 코드에서 잠재적인 오류나 버그 안티패턴등을 식별  
**best pratice 추천**  

---

[초보 웹 개발자를 위한 자바스크립트 빌드 툴과 SWC | 카카오엔터테인먼트 FE 기술블로그](https://fe-developers.kakaoent.com/2022/220217-learn-babel-terser-swc/)  
[불꽃 튀게 빠른 번들러 Parcel 개념잡기!](https://kdydesign.github.io/2020/09/23/parcel-intro/)
