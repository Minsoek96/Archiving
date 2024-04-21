# translate

> CSS transform 속성의 일부로 사용되며 translate 함수는 요소를 수평 및 수직으로 이동시킨다.

## 주요 내용

`translate()`는 GPU(그래픽 처리 장치) 가속을 사용할 수 있어서 애니메이션과 위치 변경이 매우 부드럽고 효율적이다. 브라우저는 `translate()` 를 사용할 때 레이아웃의 다른 부분을 다시 계산할 필요가 없으므로 성능이 더 좋다.

주로 애니메이션 효과에 사용되며, 빠른 성능과 부드러운 변화를 필요로 할 때 이상적이다.

### 구문

```css
transform: translate(x, y);
```

- x: 요소를 가로축으로 이동할 거리 (양수는 오른쪽, 음수는 왼쪽)
- y: 요소를 세로축으로 이동할 거리 (양수는 아래쪽, 음수는 위쪽)

```css
#my-element {
  transform: translate(50px, 20px);
   /* 요소를 오른쪽으로 50px, 아래쪽으로 20px 이동. */
}
```

### 함수 변형

- `translateX(x)`: 요소를 가로축으로만 이동 (예: translateX(20px))
- `translateY(y)`: 요소를 세로축으로만 이동 (예: translateY(-10px))
- `translate3d(x, y, z)`: 요소를 3D 공간에서 이동 (예: translate3d(50px, 20px, -10px))

---
  

## left, right 와 차이

`left` , `right` 는 요소의 위치를 조정하기 위해 주로 `position` 속성 (`relative` , `absolute` , `fixed` )와 함께 사용된다.

`left`, `right` 속성은 브라우저가 요소의 위치를 변경할 때 페이지 레이아웃을 다시 계산해야 하므로 `translate`() 에 비해 성능이 떨어질 수 있다. 이는 `리플로우(Reflow)` 및 `리페인트(Repaint)` 를 발생시켜 성능 저하를 유발할 수 있다.

주로 요소의 정적인 위치 지정에 사용되며, 페이지 레이아웃에서 요소의 정확한 위치를 지정해야 할 때 유용하다.

## 정리

`translate()`는 CSS의 `transform` 속성을 사용하여 요소의 위치를 변경한다. `transform`은 GPU 가속을 이용할 수 있고, 요소의 레이아웃이나 주변 요소에 영향을 주지 않고 시각적 위치만 변경하기 때문에 리플로우를 유발하지 않는다.

- **애니메이션 및 동적 효과**: **`translate()`** 사용 권장
- **정적 위치 지정**: **`left`**, **`right`** 사용 (절대 위치 필요 시)