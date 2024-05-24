# 디바운스, 쓰로틀링

> 디바운스(`debounce`)와 쓰로틀링(`throttling`)은 빈번하게 발생하는 이벤트를 제어하여 성능을 최적화하는 두 가지 주요 기법이다.

## 디바운스(DeBounce)

전자에서 스위치의 접점이 붙거나 떨어지는 그 짧은 순간에 접점이 고속으로 여러번 on/off되는 현상을 바운싱 현상이라고 말한다.그리고 이런 바운싱 현상을 방지하기위한 것이 디바운싱 기법이다.

### 프로그래밍에서의 디바운싱 

연이어 발생한 이벤트를 하나의 그룹으로 묶어서 처리하는 방식으로, 주로 그룹에서 처음이나 마지막으로 실행된 함수를 처리하는 방식으로 사용된다. 

### 디바운싱 사용 예시 

사용자가 창 크기 조정을 위해 `resize` 이벤트를 걸고 로그를 기록 한다고 가정해보자.
디바운싱이 적용이 안된 코드라면 로그 창에서 `resize` 이벤트가 무한히 찍혀있을 것이다. 하지만 디바운싱을 적용한다면 손에서 마우스를 뗀 순간에 `resize`로그가 단 한번 직힌다. 이렇게 하면 이벤트 부하가 현저히 줄어들기 때문에 성능적으로 이득을 취하고 요청 시마다 요금이 발생하는 API요청 작업이라면 불필요한 요청을 절감하여 비용또한 감소할 수 있다.

```jsx
function debounce(func, wait) {
    let timeout;
    return function(...args) {
        const context = this;
        clearTimeout(timeout);
        timeout = setTimeout(() => {
            func.apply(context, args);
        }, wait);
    };
}

// 로그 기록 함수
function logResizeEvent() {
    console.log('Window resized!');
}

// 디바운스 적용
const debouncedLog = debounce(logResizeEvent, 300);

// resize 이벤트에 적용
window.addEventListener('resize', debouncedLog);
```

## 쓰로틀링이란? 

쓰로틀링은 출력을 조절한다라는 의미를 가지고 있다.  
프로그래밍에서 쓰로틀링은 이벤트를 일정주기마다 발생하도록 하는 기술이다.  
`Throttle`의 설정시간으로 100ms를 주게 된다면, 해당 이벤트는 100ms 동안 최대 한 번만 발생하게 된다. 즉, 마지막 함수가 호출된 후 일정 시간이 지나기 전에 다시 호출되지 않도록 한다.

### 쓰로틀링 예시

사용자의 스크롤 동작을 감지하고 특정 작업을 수행한다고 가정해 보자

쓰로틀링이 적용이 안된 코드라면 스크롤을 움직일 때마다 동작을 수행하려고 할 것이다. 심지어 이미 해당 작업을 수행하고 있는 와중에도 새로운 요청은 계속 쌓일 것이다. 그런데 쓰로틀링이 적용되어있다면 특정 시간 동안 단 한번의 동작만 수행될것이다.  스크롤 하는 동안 무한히 발생하는 요청을 한번으로 줄이는 것은 성능에서 엄청난 이점을 가 질 수 있다.

```jsx
const throttle = (func, limit) => {
    let lastCall = 0;
    return (...args) => {
        const now = new Date().getTime();
        if (now - lastCall < limit) return;
        lastCall = now;
        func(...args);
    };
}

// 스크롤 이벤트 로그 기록 함수
const logScrollEvent = () => {
    console.log('Window scrolled!');
};

// 쓰로틀링 적용
const throttledLog = throttle(logScrollEvent, 300);

// scroll 이벤트에 적용
window.addEventListener('scroll', throttledLog);
```

## 디바운스와 쓰로틀링 차이점 요약 

|  | 디바운스 (Debounce) | 쓰로틀링 (Throttling) |
| --- | --- | --- |
| 목적 | 빈번한 호출을 지연시켜 마지막 호출만 실행 | 지정된 간격으로 이벤트를 처리 |
| 사용 예시 | 입력 필드에서의 검색 제안 | 스크롤, 리사이즈 이벤트 처리 |
| 주요 특징 | 이벤트가 멈춘 후 일정 시간 후에 실행 | 일정 간격마다 이벤트를 실행 |
| 적용 상황 | 사용자가 입력을 멈춘 후에 작업을 실행할 때 유용 | 빈번하게 발생하는 이벤트를 일정 간격으로 처리할 때 유용 |

### 리딩 엣지  VS 트레일링 엣지

리딩 엣지(Leaading Edge) :  이벤트가 처음 발생했을 때 함수를 즉시 실행.  
트레일링 엣지(Trailing Edge) : 이벤트가 끝난 후 일정 시간 지연 후에 함수를 실행
