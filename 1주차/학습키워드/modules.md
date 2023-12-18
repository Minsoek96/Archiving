# ES Modules vs CommonJS

### ES Modules

***

자바스크립트 코드를 재사용하기 위한 공식 모듈

* `import`  `export` 문을 사용 한다
* ES모듈에서는 import문을 파일의 시작 부분에서만 호출할 수 있다.
* import문은 비동기적으로 모듈을 로드한다
* import문은 자바스크립트 엔진에 의해 파싱되어 모든 import문을 찾는다.
* 모듈은 캐싱되어 재사용될 때 다시 해석되지 않는다.

### CommonJS

***

node에서 사용되는 전통 모듈 방식

* `require()` `module.exports` 문을 사용
* require()는 함수로서 런타임에 파싱이 된다.(어느곳에서나 호출가능)
* require()은 모듈을 동기적으로 로드한다.
* require문은 일반적인 자바스크립트 코드처럼 똑같이 해석된다.
* 해당 모듈 역시 캐싱이 된다.

***

[CommonJS vs. ES modules in Node.js](https://dev.to/logrocket/commonjs-vs-es-modules-in-nodejs-2eo1)\
[ESM vs CJS](https://www.knowledgehut.com/blog/web-development/commonjs-vs-es-modules#what-is-commonjs-in-node.js?%C2%A0)\
[Exploring Node.js Modules: CommonJS vs. ES6 Modules](https://medium.com/globant/exploring-node-js-modules-commonjs-vs-es6-modules-2766e838bea9)
