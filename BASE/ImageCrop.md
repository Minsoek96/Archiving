# 이미지 편집 뷰어 제작기 

X사이트의 이미지 편집 뷰어를 만들기 위해 여러 방법을 조사한 결과 `React-image-crop`라이브러리를 사용하면 쉽게 이미지 편집 기능을 구현할 수 있다는 것을 알게 되었다. 하지만 직접 구현해보고 싶은 도전 욕구가 생겨 HTML 캔버스를 활용하는 방법을 선택했다. 캔버스를 이용해 이미지를 편집하는 방법 중 `drawImage`메소드가 적합하다는 것을 알게 되었고, 이를 기반으로 구현을 시작했다. 

## `drawImage` 메소드 활용 

`drawImage` 메소드는 이미지의 특정부분을 잘라내고 캔버스에 그릴 수 있다. 이런 점을 활용해 특정 이미지의 원하는 부분만을 새롭게 표현할 수 있다.

`drawImage`에 대해서 상세하게 알아보자

`context.drawImage(img, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);`

- sx : 원본 이미지에서 잘라낼 부분의 x좌표 
- sy : 원본 이미지에서 잘라낼 부분의 y좌표
- sWidth : 원본 이미지에서 잘라낼 부분의 너비
- sHeight : 원본 이미지에서 잘라낼 부분의 높이
- dx : 캔버스에서 이미지의 왼쪽 상단 모서리를 그릴 x 좌표,
- dy : 캔버스에서 이미지의 왼쪽 상단 모서리를 그릴 y 좌표,
- dWidth : 캔버스에 그릴 이미지의 폭,
- dHeight :  캔버스에 그릴 이미지의 높이,

## 유저가 보는 화면 일치시키기

이미지 편집 과정에서 중요한 것은 사용자가 보는 화면과 실제 편집 되는 이미지가 일치하도록 하는 것이다. 이를 위해 줌 기능을 구현하면서 해당 기능와 일치하는 좌표 계산이 필요했다. 사용자 인터페이스와 캔버스에 그려지는 이미지가 정확히 일치하려던 계획은 다음과 같다. 

### 줌 박스의 구현 

줌박스란? 유저가 보는 화면상에서 스케일이 되었을 때 잘라내고 싶은 이미지가 존재하는 공간을 표현한 것이다. 문제는 유저가 보는 화면상의 줌 박스를 구현하는 것은 간단하게 CSS의 flex속성을 이용하면 구현할 수 있다. 하지만 Scale된 값을 유저가 보는 화면와 일치하게 편집하는 것이 핵심이다.

줌 박스의 구현하기 위해 다음과 같은 단계를 거쳤다.

1. 줌 박스 크기 계산 
    
    - `zoomType` 객체에서 줌 박스의 크기를 가져와 10을 곱하여 실제 줌 박스의 너비와 높이를 계산한다. 
    - 10을 곱하는 이유는 개인적인 취향이다. 평소 rem,em 단위의 표현을 즐기기 때문에 10을 곱하여 px를 표현하기 위함이다.

2. 이미지의 중앙 좌표 계산 
    
    - 줌박스가 위치할 이미지의 중앙 좌표를 계산하기 위해 이미지의 너비와 높이를 반으로 나눈다.

3. 줌박스의 시작 위치 계산 

    - 실제 이미지의 중앙 너비와 높이의 반에서 줌박스의 중앙 너비와 높이의 값에 스케일된 지점에서 계산한다. 

4. 잘라낼 부분의 위치와 너비 높이

    - 실제 이미지 상에서 줌,인 아웃이 되어 표현된 부분의 넓이와 높이를 구하기 위해 기존의 줌박스의 크기에서 배율을 나누어 계산한다.

위의 계획을 적용하여 다음과 같은 유틸 함수를 만들었다. 

```tsx
const drawImageToCanvas = (
  ctx: CanvasRenderingContext2D,
  img: HTMLImageElement,
  scale: number,
  zoomType: ZoomProps,
) => {
  const zoomBoxWidth = zoomType.width * 10;
  const zoomBoxHeight = zoomType.height * 10;
  const sx = img.width / 2 - zoomBoxWidth / 2 / scale;
  const sy = img.height / 2 - zoomBoxHeight / 2 / scale;
  const sWidth = zoomBoxWidth / scale;
  const sHeight = zoomBoxHeight / scale;
  const dWidth = zoomBoxWidth;
  const dHeight = zoomBoxHeight;

  ctx.canvas.width = zoomBoxWidth;
  ctx.canvas.height = zoomBoxHeight;

  ctx.drawImage(img, sx, sy, sWidth, sHeight, 0, 0, dWidth, dHeight);
};
```

해당 유틸 함수를 적용한 결과 다음과 같다. 

## 문제점 발생 

실제 이미지의 원본 비율을 로딩한 후 편집을 하면 예상대로 잘 동작한다. 그러나 이미지의 너비와 높이를 지정하고 `contain` 속성을 활용하여 비율을 조정하면 편집 시 이미지의 좌표가 틀리는 현상이 발생한다는 점이다. 

해당 문제가 발생한 이유는 줌 스케일링이라는 기능에만 집중한 나머지 가장 중요한 문제를 간과한 것에서 발생했다. 이미지를 CSS를 이용하여 클라이언트상에서 표현해도 실제 이미지의 비율이 변하지 않는다. 사용자의 입장에서의 표현만 해당 방식으로 보이는 것이다. 즉, 계산해야하는 좌표 공식은 스케일링만 생각할 것이 아니라, 실제로 클라이언트 상에 표현되는 이미지의 너비와 높이에서 줌스케일링 된 표현와 `contain` 비율을 적용한 이미지의 좌표를 계산한다.

```jsx
const drawImageToCanvas = (
  ctx: CanvasRenderingContext2D,
  img: HTMLImageElement,
  scale: number,
  zoomType: ZoomProps,
) => {
  const containerWidth = img.clientWidth;
  const containerHeight = img.clientHeight;

  const zoomBoxWidth = zoomType.width * 10;
  const zoomBoxHeight = zoomType.height * 10;

  // 원본 이미지 크기
  const imgWidth = img.naturalWidth;
  const imgHeight = img.naturalHeight;

  // 이미지 비율 계산
  const imgRatio = imgWidth / imgHeight;
  const containerRatio = containerWidth / containerHeight;

  let drawWidth;
  let drawHeight;

  // 컨테이너 크기에 맞춰 이미지 크기 조정
  if (imgRatio > containerRatio) {
    drawWidth = containerWidth;
    drawHeight = containerWidth / imgRatio;
  } else {
    drawHeight = containerHeight;
    drawWidth = containerHeight * imgRatio;
  }

  // 잘라낼 영역 계산 (비율에 맞게 조정된 크기 기준)
  const sx = (imgWidth - (zoomBoxWidth / scale) * (imgWidth / drawWidth)) / 2;
  const sy = (imgHeight - (zoomBoxHeight / scale) * (imgHeight / drawHeight)) / 2;
  const sWidth = (zoomBoxWidth / scale) * (imgWidth / drawWidth);
  const sHeight = (zoomBoxHeight / scale) * (imgHeight / drawHeight);

  ctx.canvas.width = zoomBoxWidth;
  ctx.canvas.height = zoomBoxHeight;

  ctx.drawImage(img, sx, sy, sWidth, sHeight, 0, 0, zoomBoxWidth, zoomBoxHeight);
};
```