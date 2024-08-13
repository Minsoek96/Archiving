# TreeShaking

`TreeShaking`은 모듈 번들러가 코드에서 사용되지 않는 부분을 제거하여 최종 번들 크기를 줄이는 최적화 기법이다. 이는 주로 `ES6`의 `import`와 `export` 구문을 사용하여 모듈을 정적으로 분석함으로써 가능해진다. `Tree Shakin`은 사용하지 않는 코드를 제거하여 번들 크기를 줄이고, 로딩 속도를 향상시킬 수 있다.

> 나무를 흔들어 쓸모 없는 나뭇잎을 털어내는것에 비유된 의미

## Tree Shaking의 배경과 원리 

`Tree Shaking`은 주로 ES6(ECMAScript 2015) 모듈 시스템의 `import`와 `export`구문을 기반으로 작동한다. ES6 모듈은 정적 구조를 가지며, 인는 컴파일 타임에 모듈의 의존성을 쉽게 분석할 수 있게 한다.

- `Tree Shaking`은 코드의 정적 분석을 통해 사용되지 않는 코드(Dead Code)를 식별한다.
- `Dead Code Elimination` : `Tree Shaking`이 사용되지 않는 코드를 식별한 후, 번들에서 제거하는 과정이다.

> why 정적분석 ? : 코드를 실행하지 않고 코드의 구조를 분석하는 것을 말한다.  
 !! 코드를 실행하지 않기 떄문에 실행 중에 예상치 못한 부작용이 발생할 수 있다.

## Tree Shaking의 작동 원리 

1. 모듈 분석 : 번들러는 애플리케이션에서 사용된 모든 모듈을 로드하고, 각 모듈의 `import`와 `export`를 분석한다.
2. 사용 여부 판단 : 코드가 실제로 사용되고 있는지 판단한다. 사용되지 않는 `export`는 제거대상이 된다.
3. 코드 제거 : Tree Shaking이 사용되지 않는 코드로 식별한 부분을 최종 번들에서 제거한다.

## Tree Shakin 설정 방법

1. ES6 모듈 사용 
Tree Shaking은 ES6 모듈(`import`, `export`)을 사용해야만 동작한다.  
CommonJS(`require`, `module.exports`)를 사용할 경우 Tree Shaking이 작동하지 않으므로 ES6 모듈을 사용하는 것이 필수적이다.
 
2. 번들러 설정  
`Webpack`와 `SWC` 번들러에서 Tree Shakin을 사용하기 위해 기본적인 설정을 확인해야한다.

### Webpack 설정 : 

```tsx
module.exports = {
  mode: 'production', // production 모드는 Tree Shaking을 자동으로 수행
  optimization: {
    usedExports: true, // Tree Shaking 활성화
    minimize: true, // 코드 압축 활성화
  },
};

```

### SWC 설정 (Next.js) :

```tsx
// next.config.js
module.exports = {
  swcMinify: true, // SWC의 코드 최소화 및 최적화
};

```

### sideEffects 설정 

`package.json` 파일에 `"sideEffects" : false` 를 추가하면, Tree Shaking이 부작용 없는 모듈을 최적화할 수 있다.

> 코드가 실행될 때, 코드 외부에 영향을 미치는 모든 동작을 의미한다.  
예를 들어, 전역 변수 변경, DOM 조작, 로깅, 파일 시스템 접근 등이 있다.

### 예외 처리 하는 방법

[ 특정 파일만 ]

```json
  "sideEffects": [
    "./src/polyfill.js",
    "./src/some-side-effect-file.js"
  ]
```

[배열 형태로 설정]

```json
{
  "sideEffects": ["*.css", "*.scss"]
}

```
> CSS 파일은 전역 스타일을 변경하므로, 사용되지 않는다고 판단되더라도 Tree Shaking에서 제거되면 안 된다.

## Tree Shakin의 부작용 

Tree Shaking을 사용할 때 몇 가지 부작용이나 잠재적 문제에 주의해야 한다.

### 부작용 코드의 잘못된 제거 

``` tsx
// polyfill.js
if (!Array.prototype.includes) {
  Array.prototype.includes = function (value) {
    return this.indexOf(value) !== -1;
  };
}
```

이 코드는 `Array.prototype.includes`가 존재하지 않는 환경에서만 실행되지만, Tree Shakin 과정에서 이 코드가 제거되면 구형 브라우저에서 동작하지 않게 된다.

