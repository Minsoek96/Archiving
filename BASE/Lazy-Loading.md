# Lazy-Loading (지연 로딩)

> `Lazy Loading`은 직역하면 게으른 로딩을 의미한다. 이 용어는 일반적으로 해야 할 일을 나중에 하도록 미루는 행동을 형용하는 단어인 '게으르다'에서 유래되었다. `Lazy Loading`은 사용자가 실제로 해당 데이터나 객체를 필요로 할 때 까지 데이터나 객체의 로딩을 미루는 것을 의미한다.

---

## Lazy Loading의 유래

일반적으로 `게으르다`는 부정적인 의미를 내포하는 단어지만, `Lazy Loading`에서는 최적화 전략으로서의 긍정적인 측면을 가지고 있다. 이것이 최적화 기법으로서 선택된 이유는 사용자 경험과 성능 향상에 큰 기여를 하기 떄문이다.

---
  

## 일반적인 Loading 방식 (등장배경)

> 일반적인 로딩 방식에서는 웹 페이지를 로딩할 때 모든 리소스가 함께 로딩된다. 다음은 이 과정을 간략하게 설명한 것이다.

- **요청 단계**: 사용자가 웹 페이지를 열 때, 브라우저는 웹 페이지의 HTML을 다운로드하고 파싱한다. 이 과정에서 브라우저는 HTML 소스 코드에 있는 모든 외부 리소스에 대한 URL을 찾아낸다. 이러한 외부 리소스는 CSS 파일, JavaScript 파일, 이미지 등이 있다.
- **다운로드 단계**: 브라우저가 URL들을 찾게 되면, 해당 URL에서 리소스를 다운로드하는 HTTP 요청을 시작한다. 이때 이미지들도 같이 다운로드가 시작된다.
- **렌더링 단계**: 각 리소스가 다운로드되면 브라우저는 페이지를 렌더링하기 시작한다.이미지의 경우, 해당 이미지가 다운로드되면 그 위치에 이미지가 표시된다. 아직 다운로드가 완료되지 않은 이미지에 대해서는 빈 공간이나 로딩 표시가 나타날 수 있다.
- **완료 단계**: 모든 리소스가 다운로드되고 처리되면 페이지 로딩은 완료된다. 이제 사용자는 모든 이미지와 텍스트, 기타 리소스를 볼 수 있다.

이런 일반적인 로딩 방식의 문제점은 사용자가 실제로 보지 않을 페이지의 아래 부분에 위치한 이미지까지도 불필요하게 다운로드되는 경우가 있다. 이는 로딩 시간을 늘리고 네트워크 리소스를 낭비하는 결과를 초래한다.


---
  

## 해결법

이런 문제점을 해결하기 위해 `Lazy Loading`이 등장했다. `Lazy Loading`은 사용자가 실제로 볼 필요가 있는 이미지만 먼저 로드하고, 나머지는 사용자가 필요한 시점에 로드하는 방식이다. 이렇게 하면 초기 페이지 로딩 시간을 더 크게 단축시키고, 네트워크 리소스를 효율적으로 사용할 수 있게 된다. 따라서 사용자는 더 빠르게 웹 페이즈를 볼 수 있게 되며, 이는 사용자가 경험을 향상시키는데 큰 도움이 된다.

## JavaScript에서 Lazy Loading

`Lazy Loading`을 구현하는 방법은 여러 가지가 있다. 여기서는 가장 흔히 사용되는 두 가지 방법

- JavaScript를 이용한 Lazy Loading
- 네이티브 Lazy Loading

### JavaScript를 이용한 Lazy Loading

JavaScript를 이용한 `Lazy Loading`은 `Intersection Observer API`를 이용한다. 이 API는 특정 요소가 뷰포트 내에 들어오거나 나갈 때를 감지할 수 있다.

예를 들어, 이미지 태그에는 `데이터 속성(data-attribute)`를 사용하여 실제 이미지 URL을 저장해두고, 이 이미지 태그가 뷰포트 내에 들어올 때 해당 URL을 이미지 태그의 `'src'` 속성에 설정하는 방식을 사용할 수 있다.

```jsx
<img class="lazy-load-image" data-src="image_url.jpg" />;

//----

let images = document.querySelectorAll(".lazy-load-image");

let observer = new IntersectionObserver((entries, observer) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      let img = entry.target;
      img.src = img.dataset.src;
      observer.unobserve(img);
    }
  });
});

images.forEach((img) => {
  observer.observe(img);
});
```

이 코드는 뷰포트 내에 들어오는 모든 `.lazy-load-image 클래스`를 가진 이미지 태그에 대해 `Intersection Observer`를 설정한다. 이미지가 뷰포트 내로 들어오면, 해당 이미지의 `'src' 속성을 'data-src'` 속성에 저장된 실제 `URL`로 설정합니다. 이렇게 하면 이미지가 실제로 필요할 때만 로드된다.

---
  

### 네이티브 Lazy Loading

브라우저의 네이티브 Lazy Loading은 **`img`** 태그와 **`iframe`** 태그에 'loading' 속성을 'lazy'로 설정함으로써 사용할 수 있다. 이 기능은 현재 Chrome, Firefox, Edge 등 주요 브라우저에서 지원하고 있다.

HTML 예시:

```html
htmlCopy code
<img src="image_url.jpg" loading="lazy" />
<iframe src="video_url" loading="lazy"></iframe>
```

이 방식은 JavaScript를 이용한 방식보다 더 간단하고, JavaScript를 사용하지 않는 사용자에게도 Lazy Loading을 제공할 수 있다는 장점이 있다. 하지만 아직 모든 브라우저에서 지원하지 않기 때문에 브라우저 호환성을 고려해야 합니다. 이런 이유로 종종 JavaScript를 이용한 방식과 함께 사용되곤 한다.
