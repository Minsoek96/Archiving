## ES Modules vs CommonJS
ES Modules 는 자바스크립트에서 사용되는 모듈 방식  
CommonJS는 node에서 사용되는 전통 모듈 방식

ESModules는 모듈을 가져올때 import문을 사용하고  
내보낼때는 export문을 사용한다. 

CommonJS는 모듈을 가져올때 require()문을 사용하고   
내보낼때는 module.exports를 사용한다.

import와 require 문의차이  
import는 비동기적인 방식으로 모듈 로딩 한다.    
- 페이지 구성에 필요한 부분만 먼저 가져오고 , 나머지는 지연로딩 하여 초기 로딩 속도의 장점을 취하기 위한 브라우저에 적합   
require문은 동기적인 방식으로 모듈을 로딩하여 코드를 순차적으로 실행한다.

- 모듈을 순차적으로 로딩하여, 모듈간의 서로의 의존성을 확실하기 위한 서버사이드에 적합

node.js의 CommonJS방식은 ES6이전에 도입되었다.  
Node.js 13.2.0 이상부터 ESM방식을 사용 할 수 있다. 

---

[CommonJS vs. ES modules in Node.js](https://dev.to/logrocket/commonjs-vs-es-modules-in-nodejs-2eo1)  
[ESM vs CJS](https://www.knowledgehut.com/blog/web-development/commonjs-vs-es-modules#what-is-commonjs-in-node.js?%C2%A0)  
[Exploring Node.js Modules: CommonJS vs. ES6 Modules](https://medium.com/globant/exploring-node-js-modules-commonjs-vs-es6-modules-2766e838bea9)