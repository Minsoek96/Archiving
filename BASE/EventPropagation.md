# EventPropagation

> 이벤트 전파는 DOM에서 이벤트가 전달되는 방식을 말한다.

- 이벤트 캡처링(Event Capturing)
- 이벤트 버블링(Event Bubbling)

이벤트 캡처링 -> 타겟 -> 이벤트 버블링 

---

## 이벤트 캡처링(Event Capturing)

> 이벤트 캡처링은 이벤트가 최상위 노드에서부터 시작하여, 대상 요소까지 이벤트를 전달하는 과정이다. 

사용자가 버튼을 클릭하면, 이벤트 `document`객체에서부터 시작하여, 클릭된 버튼에 도달할 때까지 상위 요소를 따라 내려온다.  
`document` → `<html>` → `<body>` → `<div`  → `<button>` 순서로 이벤트가 전달된다.  
주로 이벤트를 가로채거나 특정 조건에서만 이벤트를 처리할 필요가 있을때 사용된다.

### 사용방법 

`element.addEventListener(event, function, true)`

이벤트 리스너의 3번째 인자를 `true`로 설정한다.  
기본적으로 이벤트의 예기치 못한 동작 방지를 위해 `false`로 설정되어있다.

## 이벤트 버블링(Event Bubbling)

> 이벤트 버블링은 이벤특가 대상 요소에서 시작하여, 최상위 노드까지 이벤트를 전파하는 과정이다. 
  
`<button>` → `div` → `<body>` → `<html>` → `document` 순서로 이벤트가 버블링된다.  
대부분의 이벤트 처리는 버블링 단계에서 수행된다.   
버블링을 이용하면 하위요소에서 발생한 이벤트를 상위 요소에서 처리할 수 있다.  

## 이벤트 위임(Event Delegation)이란?

>이벤트 위임은 부모 요소에 이벤트 리스너를 등록하여, 그 부모의 여러 자식 요소들에서 발생한 이벤트를 처리하는 기법이다.

- 이벤트를 줄여 메모리 사용량을 줄인다.
- 동적으로 요소를 추가하거나 제거할 때의 복잡성을 감소시킨다.

```jsx
<ul id="itemList">
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
  <!-- 더 많은 아이템이 있을 수 있습니다 -->
</ul>

------

document.getElementById('itemList').addEventListener('click', function(e) {
  // e.target은 클릭된 대상 요소입니다.
  if (e.target.tagName === 'LI') {
    alert(e.target.textContent + '이(가) 클릭되었습니다.');
  }
});
```

### 그외 중요사항 

**이벤트 중지** : `event.stopPropagation()` 메소드를 사용하여 이벤트의 캡처링이나 버블링을 중지할 수 있다.  
**기본 동작 방지** : `event.preventDefault()` 메소드를 사용하여 이벤트의 기본 동작을 방지할 수 있다.