# Day.js

> `Day.js`는 대부분의 API가 `Moment.js`와 호환되며 최신 브라우저에서 날짜와 시간에 대한 구문 분석, 유효성 검사, 조작, 추력을 간편하게 처리하는 경량 JavaScript라이브러리 이다.

## 등장 배경

기존의 `Moment.js`의 치명적인 몇가지의 문제점을 해결하기 위해 등장


### 강력한 만큼 파일의 크기도 크다.

`Moment.js`는 기본파일들(로케일)이 번들의 사이즈를 키우는 문제가 있어 웹의 로딩 시간이 늘어났다.

> `Day.js`는 극단적으로 작은 파일 크기(**약2KB**)를 가지며, 필요한 기능만 선택적으로 추가할 수 있는 플러그인 시스템을 통해 더욱 경량화 할 수 있다.

### 불변성

`Moment.js`는 날짜 객체를 조작할 때 원본 객체를 변경하여, 예상치 못한 사이드 이펙트가 발생할 수 있는 가능성이 존재, 예를 들어 날짜 객체를 조작한 후 원본 객체를 다시 사용해야 하는 경우, 이미 조작된 상태이기 때문에 문제가 발생한다.

> `Day.js`는 불변 객체를 사용하여 날짜를 조작할 때마다 새로운 객체를 반환한다.

### 모듈화

`Moment.js`에서는 모든 기능이 하나의 큰패키지로 제공된다. 사용하지 않는 기능이라도 포함되어 파일크기가 커질 수 밖에 없다.

> Day.js는 핵심 기능에 초점을 맞추고 추가적인 기능은 플러그인을 통해 제공한다.

---

## 특징

- 친숙한 Moment.js API와 패턴
- 불변 오브젝트(immutable)
- 메소드 체인(Chainable)
- l18n 지원
- 2kb 미니 라이브러리
- 모든 브라우저 지원

---

## DOCS

### 설치

```jsx
npm install dayjs
```

### Base

현재 날짜 객체 생성

```jsx
const now = dayjs();
console.log(now.toString());
```

특정 날짜를 가지고 객체를 생성

```jsx
const specificDate = dayjs('2004-03-20');
console.log(specificDate.format('YYYY-MM-DD');
```

날짜 포맷팅

```jsx
console.log(now.format("YYYY년 MM월 DD일"));
```

날짜 조작

```jsx
const nextWeek = now.add(1, "week");
console.log(nextWeek.format("YYYY-MM-DD"));

// 3일후
console.log(now.add(3, "day").format("YYYY-MM-DD"));

// 2개월 전
console.log(now.subtract(2, "month").format("YYYY-MM-DD"));

// 날짜 설정
console.log(now.set("year", 2025).format("YYYY-MM-DD"));
```

날짜 비교

```jsx
const isBefore = specificDate.isBefore(now);
console.log(isBefore);

if (dayjs("2024-03-20").isBefore("2024-04-01")) {
  console.log("3월 20일은 4월 1일 이전입니다.");
}
```

국제화(I18n)

```jsx
import "dayjs/locale/ko"; // 한국어 로케일 파일을 임포트합니다.
dayjs.locale("ko"); // 로케일을 한국어로 설정합니다.

console.log(now.format("YYYY년 MM월 DD일")); // 한국어로 날짜를 포맷팅합니다.
```

### PlugIn

Day.js의 기능을 확장하고 싶을 때는 플러그인을 사용할 수 있다.

```jsx
import relativeTime from "dayjs/plugin/relativeTime";
dayjs.extend(relativeTime);

console.log(now.from(dayjs("2023-01-01"))); // '2개월 후'와 같이 출력됩니다.
```
