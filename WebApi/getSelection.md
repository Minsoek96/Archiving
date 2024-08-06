# getSelection

사용자가 선택한 텍스트를 관리하거나 조작할 수 있도록 도와준다. 이 API는 DOM과 상호작용하며, 주로 텍스트 편집기, 웹 기반 텍스트 처리에서 활용된다.

## 기본 개념

[**window.getSelection()**]

- `window.getSelection()` 메서드는 현재 사용자가 브라우저에서 선택한 텍스트를 반환한다.
  반환 값은 `Selection` 객체이다.

[**Selection객체**]

- `Selection` 객체는 사용자가 선택한 영역을 나타낸다. 이 객체는 선택된 텍스트의 시작점과 끝 선택된 범위의 속성 등을 포함한다.

### 주요 속성과 메서드

[**속성**]

- anchorNode: 선택 시작 지점을 포함하는 노드.
- anchorOffset: 선택 시작 지점의 오프셋(문자열 내 위치).
- focusNode: 선택 종료 지점을 포함하는 노드.
- focusOffset: 선택 종료 지점의 오프셋.
- isCollapsed: 선택 영역이 비어 있는지 여부 (true: 선택된 텍스트 없음, false: 선택된 텍스트 있음).

[**주요 메서드**]

- addRange(range) : Range 객체를 선택 영역에 추가한다.
- removeAllRanges() : 현재의 모든 선택 영역을 제거한다.
- toString() : 선택된 텍스트를 문자열로 반환한다.
- getRangeAt(index) : 지정된 인텍스의 `Range` 객체를 반환한다.

### getRangeAt

`getRangeAt(index)` 메서드는 `Selection` 객체에서 인덱스에 해당하는 `Range`객체를 반환한다. `Range` 객체는 선택된 텍스트의 시작과 끝을 정의하며, 이 객체를 통해 선택된 영역을 조작하거나 정보를 추출할 수 있다.

Range ? : DOM에서 문서 내의 텍스트나 노드의 특정 범위를 표현하는데 사용된다.

- 시작지점 (start) :  범위의 시작위치 
- 끝 지점 (end) : 범위의 끝위치

### 기본 사용법

- 기본사용법

```tsx
const selection = window.getSelection();
if (selection.rangeCount > 0) {
  const range = selection.getRangeAt(0); // 첫 번째 Range 객체를 가져옵니다.
  console.log("Range 시작 노드:", range.startContainer);
  console.log("Range 시작 오프셋:", range.startOffset);
  console.log("Range 끝 노드:", range.endContainer);
  console.log("Range 끝 오프셋:", range.endOffset);
  console.log("Range 텍스트:", range.toString());
}
```

- 선택된 범위 조작

```tsx
const selection = window.getSelection();
if (selection.rangeCount > 0) {
  const range = selection.getRangeAt(0);
  range.deleteContents(); // 기존 선택 영역의 내용을 삭제
  range.insertNode(document.createTextNode("새 텍스트")); // 새 텍스트 삽입
}
```
