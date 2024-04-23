# next/image

> <Image/> 컴포넌트는 웹페이지에서 이미지를 효율적으로 관리하고 최적화하기 위해 설계된 기능이다. 기본적인 <img> 태그의 기능을 향상시키고 다양한 최적화 기능을 자동으로 제공한다.

## 주요 특징

### 자동 이미지 최적화

`next/image` 는 이미지를 자동으로 최적화하여 파일 크기를 줄이고 로드 속도를 향상 시킨다.

- 형식 변환 : 가능한 경우 WebP와 같은 더 효율적인 이미지 형식으로 변환된다.
- 크기 조정 : 이미지를 필요한 크기로 자동으로 조정한다.
- 압축 : 이미지를 압축하여 파일 크기를 줄인다.

### 자동 사이즈 조절

`next/image`는 이미지를 자동으로 브라우저 창 크기에 맞게 조정하여 이미지가 항상 적절한 크기로 표시되도록 한다.

### 레이지 로딩

`next/image`는 이미지가 스크롤 영역에 들어올 때까지 로드되지 않도록 레이지 로딩을 사용하여 페이지 로드 속도를 향상 시킨다.

### 웹 P 지원

`next/image` 는 웹 P 이미지 형식을 지원하여 브라우저가 지원하는 경우 웹P 이미지를 자동으로 로드한다. 웹P는 일반적인 JPEG 및 PNG 형식보다 더 효율적인 이미지 형식이다.

---

## 컴포넌트 속성 옵션

| 속성           | 설명                                                                    | 기본값    |
| -------------- | ----------------------------------------------------------------------- | --------- |
| src            | 표시할 이미지의 URL                                                     | 필수      |
| alt            | 접근성을 위해 사용되는 이미지의 대체 텍스트                             | 필수      |
| width          | 이미지 너비 (픽셀)                                                      | 선택 사항 |
| height         | 이미지 높이 (픽셀)                                                      | 선택 사항 |
| layout         | 이미지 레이아웃. fill, contain, intrinsic , fixed                       | intrinsic |
| objectFit      | 이미지에 대한 object-fit CSS 속성. cover, contain, scale-down , initial | cover     |
| objectPosition | 이미지에 대한 object-position CSS 속성. center 또는 top left            | center    |
| unoptimized    | true로 설정하면 이미지가 next/image에 의해 최적화되지 않는다.           | false     |
| placeholder    | 이미지 로딩 중 표시할 플레이스홀더. blur 또는 empty                     | blur      |
| loader         | 이미지를 로드하는 사용자 지정 로더 함수.                                | default   |
| quality        | 최적화 시 이미지 품질. 0과 100 사이의 숫자일 수 있다.                   | 75        |
| domains        | 이미지를 제공할 수 있는 도메인 목록                                     | []        |
| blurhash       | 이미지 로딩 중 표시할 이미지의 BlurHash                                 | null      |
| priority       | 로드할 이미지의 우선 순위. low, medium ,high                            | medium    |

### 세부사항

- `src`와 `alt`를 제외한 모든 속성은 선택 사항이다.
- `width`와 `height`를 모두 제공하면 이미지가 종횡비를 유지하면서 해당 치수에 맞게 크기가 조정된다.
- `layout` 속성은 이미지가 컨테이너 내에서 어떻게 위치되는지를 제어한다.
- `objectFit` 속성은 이미지가 컨테이너에 맞게 어떻게 크기가 조정되는지를 제어한다.
- `objectPosition` 속성은 컨테이너 내에서 이미지의 위치를 제어한다.
- `unoptimized` 속성은 디버깅 목적으로 또는 `next/image`에 의해 최적화되기를 원하지 않는 타사 소스로부터 이미지를 로드하는 데 유용할 수 있다.
- `placeholder` 속성은 실제 이미지가 로드되는 동안 플레이스홀더 이미지를 표시하여 페이지의 인지된 성능을 향상시키는 데 도움이 될 수 있다.
- `loader` 속성을 사용하면 이미지 로드 방식을 사용자 지정할 수 있다.
- `quality` 속성은 최적화 시 이미지 품질을 제어합니다. 더 높은 값은 파일 크기가 더 크지만 이미지 품질이 더 좋아진다.
- `domains` 속성을 사용하여 이미지를 제공할 수 있는 도메인을 제한할 수 있다.
- `blurhash` 속성을 사용하면 실제 이미지가 로드되는 동안 이미지의 흐릿한 버전을 표시할 수 있다. 이는 페이지의 인지된 성능을 향상시키는 데 도움이 될 수 있다.
- `priority` 속성은 로드할 이미지의 우선 순위를 제어한다. 우선 순위가 높은 이미지는 우선 순위가 낮은 이미지보다 먼저 로드된다.

---
  
## 변경사항 (Next.js 13버전)

### 필수 속성 추가

- alt : 이미지를 설명하는 대체 텍스트가 필수 속성이 되었다.

### 삭제된 속성:

- `layout`  :  이전에는 이미지 레이아웃을 제어하는 `layout` 속성이 있었지만, 13버전 부터는 이미지 컨테이너 크기를 CSS로 직접 스타일링 하는 방식으로 변경되었다.
- `objectFit` : 이미지 크기 조정을 위해 사용되던 `objectFit` 속성도 삭제되었다. 마찬가지로 CSS 스타일링을 이용하여 이미지 크기 조정을  수 있다.
- `objectPostion` :  이미지 위치 조정을 위한 `objectPostion` 속성도 삭제되었다.

### onLoadingComplete 업데이트

- `onLoadingComplete` : 콜백 함수에서 이제 이미지 요소 (<img>)자체를 인자로 받을 수 있따.

### props 표

| 속성 | 유형 | 설명 | 기본값 | 필수 여부 |
| --- | --- | --- | --- | --- |
| src | String | 이미지 소스 URL 또는 파일 경로 | - | 필수 |
| width | Integer (px) | 이미지 너비 (픽셀) | - | 필수 |
| height | Integer (px) | 이미지 높이 (픽셀) | - | 필수 |
| alt | String | 대체 텍스트 | - | 필수 |
| loader | Function | 이미지 로딩을 위한 커스텀 로더 함수 | defaultLoader | 선택 |
| fill | Boolean | 이미지를 컨테이너 전체에 채우도록 설정 | false | 선택 |
| sizes | String | 이미지 크기 조정 규칙 | (max-width: 768px) 100vw, 50vw | 선택 |
| quality | Integer (1-100) | 이미지 품질 | 75 | 선택 |
| priority | Boolean | 이미지 로딩 우선순위를 높임 | false | 선택 |
| placeholder | String | 이미지 로딩 동안 표시되는 플레이스홀더 유형 | blur | 선택 |
| style | Object | 이미지 스타일 | - | 선택 |
| onLoadingComplete | Function | 이미지 로딩 완료 시 호출되는 함수 | - | 선택 |
| onError | Function | 이미지 로딩 오류 발생 시 호출되는 함수 | - | 선택 |
| loading | String | 이미지 로딩 방식 | lazy | 선택 |
| blurDataURL | String | 이미지 로딩 동안 표시되는 블러 효과 데이터 URL | - | 선택 |
| overrideSrc | String | SEO를 위한 대체 이미지 소스 URL | - | 선택 |