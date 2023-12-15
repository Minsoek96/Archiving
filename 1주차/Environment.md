# 개발 환경 셋팅

💡 왜 어려운가? : 시대에 흐름에 따라 선호 환경이 바뀌기 때문에 앞으로의 변경에 대응할 수 있는 능력을 키우는 게 중요하다.

## Fnm (Fast Node Manager)

다양한 노드 버전을 쉽게 설치하고 관리를 도와준다.

```text
- fnm : Fast Node Manager
- nvm: Node Version Manager
```

fnm는 Rust언어로 작성되어 메모리 사용의 효율성과 속도의 중점을 맞췄다.  
nvm은 Bash스크립트 언어로 작성되어 OS자원을 많이 사용한다.  
fnm > nvm 속도 측면에서 fnm이 빠르다.

&nbsp;

**Parcel**은 번들러 , 만능도구  
번들러는 앱을 실행하는데 필요한 웹문서를 모듈화 하여 보다 쉽게 관리할수있게 해준다.

&nbsp;

**.gitignore설정**  
github gitignore node추천

## -D는 어떤 의미 ??

```bash
npm install -D
```

**-D** : `—save-dev` 의 짧은 표현  
`devDependencies` / `dependencies`  

개발의존성(devDependencies)  
: 개발 과정에서 만 필요한 패키지  
**EX**: 코드린팅 , 단위 테스트 , 컴파일링 등  

일반의존성(dependencies)  
: 애플리케이션이 실제로 실행될때 필요하다.  
**EX**: React , Express 와 같은 라이브러리

## **Code Spliting**

성능 최적화를 위한 중요한 기술

    큰 자바스크립트 파일을 여러개의 작은 청크로 나누어, 사용자의 필요에 따라 필요한 청크만을 로드하는 방식이다.

- 초기 로딩 시간 감소
- 비동기 로딩
- 리소스 효율적 사용

**EX**: webpack, React.lazy, Suspense
