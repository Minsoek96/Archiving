# DeclarativeProgramming

> 명령형 프로그래밍은 코드가 동작하는 과정(**how**) 을 설명하는 반면,\
> 선언형 프로그래밍은 코드가 수행하는 결과(**what**)에 초점을 맞춘다.

***

## 실생활 예제

### 상황 : 오래만에 외식을 하려 유명한 음식점을 방문했다.

**명령형 접근법**\
고객(개발자) : 17번 테이블이 비어있는 것을 확인 했습니다. 저희가 저쪽으로 가서 앉아도 될까요?\
\=> 고객은 스스로 자리를 찾고, 그 과정을 직접 관리하여 진행한다.

**선언형 접근법** 고객(개발자) : 2명이요.\
\=> 고객이 단순히 필요한 자리 수 만을 선언하면 자리를 찾는 과정은 직원(라이브러리/시스템)에게 맡긴다.

명령형 접근법은 고객이 어떻게 자리를 찾을 것인지에 중점을 둔다. 그러나\
선언형 접근법에서는 고객이 "무엇을" 원하는지(2명의 자리)만 선언하면 어떻게 자리를 찾을지는 서비스 직원(즉, 외부)에게 맡긴다.

***

### 상황 : 파티를 위해 손님들에게 초대장을 보내려 한다.

**명령형 접근법**\
00역 4번출구에서 나와서 오른쪽 모퉁이를 돌고 한 블럭 가셔서 왼쪽으로 드세요. 그리고 홈마트가 보일때까지 쭉 걸어오세요 홈마트에서 오른쪽 모퉁이를 돌면 저희 집입니다.

* 손님들에게 도착할 위치까지의 구체적인 단계별 경로를 안내한다. 여기서는 초대하는 사람이 어떻게 집에 오는지를 상세하게 안내하고 있다.

**선언형 접근법** : 집주소\[상세주소]

* 손님들에게 단순히 집의 주소만을 알려준다. 손님들이 무엇을 알아야 하는지(즉, 집의 주소)만 선언하며 그들 스스로 경로를 결정하게 한다.

***

명령형 프로그래밍 : C, C++, 자바

선언적 프로그래밍 : HTML, SQL

하이브리드 : 자바스크립트, C#, 파이썬

> `SELECT * FROM Users WHERE Country = 'Korea`;

```jsx
<article>
  <header>
    <h1>Declarative Programming</h1>
    <p>Sprinkle Declarative in your verbiage to sound smart</p>
  </header>
</article>
```

선언형 프로그래밍의 핵심은 어떻게 동작하는지보다는 어떤 결과를 내놓을지에 중점을 둔다.\
이러한 특성 덕분에, 우리는 코드의 결과를 명확하게 예측할 수 있다.

## 자바스크립트로 알아보는 예제

***

### 배열의 모든 요소를 더하시오

#### 명령형

```jsx
let sum = 0;
let array = [1, 2, 3, 4, 5];
for (let i = 0; i < array.length; i++) {
  sum += array[i];
}
console.log(sum);
```

* 합계를 저장할 변수 `sum`을 0으로 초기화한다.
* `array`의 길이만큼 반복문을 실행한다.
* 각 반복에서 `array[i]`의 값을 `sum`에 더한다
* 결과를 콘솔에 출력한다.

#### 선언형

```jsx
let array = [1, 2, 3, 4, 5];
let sum = array.reduce((pre, cur) => pre + cur, 0);
console.log(sum);
```

* `array.reduce`메서드를 사용하여 배열의 모든 요소의 합을 한 번에 계산한다.
* 결과값을 `sum`변수에 저장하고, 콘솔에 출력한다.

두 유형의 코드 분석만 살펴봐도, 명령형 방식이 선언형 방식에 비해 과정이 복잡하고 가독성이 떨어진다는 것을 알 수 있다. 그러나, 제시된 예시는 매우 간단하여, 단순히 for문을 고차 함수 메서드로 바꾼 것이 아닌가 하는 의문이 들 수 있다.

더 복잡한 예시를 통해 선언형 프로그래밍이 어떻게 코드를 더 간결하고 명확하게 만들어 줄 수 있는지 살펴보겠다.

### 더 심화된 예시

***

```jsx
let users = [
  { id: 1, name: "수도승", age: 28, city: "신림" },
  { id: 2, name: "매니저", age: 22, city: "강남" },
  { id: 3, name: "유리", age: 32, city: "봉천" },
  { id: 4, name: "맹구", age: 27, city: "사당" },
  { id: 5, name: "훈이", age: 31, city: "강남" },
];

let gangnamUsers = [];
let totalAge = 0;

for (let i = 0; i < users.length; i++) {
  if (users[i].city === "강남") {
    gangnamUsers.push(users[i]);
    totalAge += users[i].age;
  }
}

let avarageAge = totalAge / gangnamUsers.length;
let oldThanAverageUsers = [];
console.log(avarageAge);

for (let i = 0; i < gangnamUsers.length; i++) {
  if (gangnamUsers[i].age > avarageAge) {
    oldThanAverageUsers.push(gangnamUsers[i].name);
  }
}
console.log(gangnamUsers);
console.log(totalAge);
console.log(oldThanAverageUsers);
```

* 강남 유저 탐색 결과를 저장할 변수 gangnamUsers를 \[]초기화 한다.
* 탐색유저의 평균값을 저장할 변수 totalAge를 0으로 초기화 한다.
* 반복문을 통해 강남유저와 totalAge 값을 구한다.
* totalAge 값을 구해진 강남유저 배열 길이 만큼 나눠서 평균값을 구한다.
* 평균값보다 나이 많은 유저를 저장할 변수 oldThanAverageUsers를 \[]로 초기화한다.
* 반복문을 통해 oldThanAverageUsers를 구한다.
* 결과를 출력한다.

#### 선언형2

```jsx
let users = [
  { id: 1, name: '수도승', age: 28, city: '신림' },
  { id: 2, name: '매니저', age: 22, city: '강남' },
  { id: 3, name: '유리', age: 32, city: '봉천' },
  { id: 4, name: '맹구', age: 27, city: '사당' },
  { id: 5, name: '훈이', age: 31, city: '강남' },
]

let gangnamUsers =
        users.filter((user) => user.city === "강남");
let totalAge =
        gangnamUsers.reduce((pre,cur) => pre + cur.age, 0);
let oldthanAverageUsers =
        gangnamUsers.filter((user) => user.age > totalAge/)

console.log(gangnamUsers)
console.log(totalAge)
console.log(oldThanAverageUsers)
```

* `filter` 메서드를 사용해 강남에 사는 사용자를 찾아 `gangnamUsers` 배열에 저장한다.
* `reduce` 메서드를 사용해 강남 사용자들의 총 나이를 `totalAge`에 더한다.
* 평균 나이를 구한 뒤, `filter`와 `map`메서드를 사용해 평균 나이보다 많은 강남 사용자의 이름을 `olderThanAverageUsers`배열에 저장한다.
* 결과를 출력한다.

선언형 코드는 결과에만 중점을 두며, 어떠한 연산을 할 것 인지에 대한 세부적인 과정은 감추어진 상태로 표현된다. 코드가 더 간결하고 가독성이 향상되며, 문제 해결에 집중할 수 있다.

***

[Imperative vs Declarative Programming](https://ui.dev/imperative-vs-declarative-programming)
