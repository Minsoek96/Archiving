## NPM

npm(node package manager)은 자바스크립트 패키지 매니저이다. Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI(Command line interface)를 제공한다.

자신이 작성한 패키지를 공개할 수 도 있고 필요한 패키지를 검색하여 재사용할 수도 있다.

**역사 및 필요성 :**  
NPM은 2010년 처음 출시 되었다.  
기존에 모듈 패키징이 엉망으로 완성되는 것을 관찰하고 펄의 CPAN와 PHP의 PEAR와 같은 기타 유사한 프로젝트의 단점들에서 영향을 받은 아이작 Z숄루터가 개발하였다.

**특징 및 장단점 :**

- 장점 :

  - 세계 최대의 소프트웨어 레지스트리이다.
    - 다양한 라이브러리와 도구에 접근할 수 있다
  - 쉬운 사용성
    - CLI를 통해 패키지 설치 및 관리가 간단하다.
  - 의존성 관리
    - 프로젝트의 의존성을 자동으로 관리하여 효율적인 개발을 지원합니다.

- 단점 :
  - 의존성 지옥
    - 복잡한 의존성 구조로 인해 떄때로 문제가 발생한다.
  - 보안 문제
    - 오픈 소스 패키지 특성

레지스트리 ?

- JavaScript 소프트웨어와 그 주변의 메타정보를 포함하는 큰 공공 데이터베이스

### package.json / package-lock.json  

**pagckage.json**  
: Node.js 프로젝트의 중요한 설정 파일로, 프로젝트와 관련된 메타데이터와 의존성을 정의한다.

- JSON 형식으로 작성된다.
- 프로젝트 이름, 버전, 설명, 저자, 라이선스 등의 정보를 포함한다.
- NPM 패키지를 `dependencies`와 `devDependencies` 로 나누어 관리한다.
- script 명령어를 정의할 수 있다.
- 프로젝트 홈페이지, 버그 URL에 정보를 포함할 수 있다.  
  
**pacakge-lock.json**  
: node_modules 트리 또는 package.json이 변경될때 자동으로 생성된다.  
설치된 의존성의 정확한 버전과 구조를 기록하여 동일한 의존성 트리를 재생성할 수 있게 한다

**차이점:**  
`package.json`은 필요한 의존성을 범위로 정의하는 반면 `package-lock.json`은 의존성들의 구체적인 버전과 구조를 정확하게 기록하여 일관된 설치 환경을 보장한다

**요약:**  
package.json은 의존성에 대한 부분만 기록 하는 게 아닌 프로젝트의 구성 정보까지 포함하는 반면 package-lock.json은 프로젝트의 의존성에 대한 부분을 더 정확하고 세밀하게 관리하여 일관된 환경을 제공해준다.

**npm install . (package.json은 필수 파일)**  
package-lock.json이 없는 경우 npm install은 package.json에 범위 내에서 최신 버전을 설치하게 된다.  
package-lock.json이 있는 경우 npm install은 정확한 버전으로 설치하게 된다.

### node_modules  

- 의존성 저장소
- 설치된 각 패키지의 하위 의존성까지 보관
- npm install 시 자동 생성
- package.json, package-lock.json이 있으면 재설치 가능

### npx  

npm은 개발자가 패키지를 전역 또는 로컬로 설치할 수 있는 방법을 제공한다. npm을 통해 실행 파일을 로컬로 설치하면 **`./node_modules/.bin/`** 디렉토리에 생성된다. 이 패키지를 실행하기 위해서는 일반적으로 **`package.json`**의 **`scripts`** 설정 과정을 거쳐야 한다.

반면, npx는 로컬이나 전역에 남기지 않고 사용이 끝난후 제거하는 일회성 이라는 특징을 가지고 있다. npx는 **`package.json`**의 설정 과정을 거치지 않고 **`./node_modules/.bin/`** 내의 패키지를 직접 실행할 수 있다.

- 일회성 사용이나 간편한 패키지 실행에 유용하다.

---

[PoiemaWeb](https://poiemaweb.com/nodejs-npm)  
[npm (소프트웨어)](<https://ko.wikipedia.org/wiki/Npm_(소프트웨어)>)  
[package.json | npm Docs](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)  
[package-lock.json | npm Docs](https://docs.npmjs.com/cli/v10/configuring-npm/package-lock-json)  
[npm vs npx — What’s the Difference?](https://www.freecodecamp.org/news/npm-vs-npx-whats-the-difference/)
