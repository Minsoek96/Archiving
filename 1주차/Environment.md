---
description: '💡 왜 어려운가? : 시대에 흐름에 따라 선호 환경이 바뀌기 때문에 앞으로의 변경에 대응할 수 있는 능력을 키우는 게 중요하다.'
---

# 개발환경셋팅

### Fnm (Fast Node Manager)

***

다양한 노드 버전을 쉽게 설치하고 관리를 도와준다.

```
- fnm : Fast Node Manager
- nvm: Node Version Manager
```

**Parcel**은 번들러 , 만능도구\
번들러는 앱을 실행하는데 필요한 웹문서를 모듈화 하여 보다 쉽게 관리할수있게 해준다.

&#x20;

**.gitignore설정**\
github gitignore node추천



### -D는 어떤 의미 ??

***

```bash
npm install -D
```

**-D** : `—save-dev` 의 짧은 표현\
`devDependencies` / `dependencies`

개발의존성(devDependencies)\
: 개발 과정에서 만 필요한 패키지\
**EX**: 코드린팅 , 단위 테스트 , 컴파일링 등

일반의존성(dependencies)\
: 애플리케이션이 실제로 실행될때 필요하다.\
**EX**: React , Express 와 같은 라이브러리

### **Code Spliting**

**---**

성능 최적화를 위한 중요한 기술

* 초기 로딩 시간 감소
* 비동기 로딩
* 리소스 효율적 사용

**EX**: webpack, React.lazy, Suspense
