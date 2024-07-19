# timerMock

`setTimeOut()`, `setInterval()`, `clearTimeout()`, `clearInterval()`는 실제 시간이 경과해야 하기 떄문에 테스트 환경에 적합하지 않다.

> jest는 타이머를 제어할 수 있는 함수로 대체하여 시간을 제어한다.

## 가짜 타이머 활성화

- `jest.useFakeTimers()`를 호출하여 가짜 타이머를 활성화할 수 있다.
- 타이머는 `jest.useRealTimers()`로 원래 동작으로 복원할 수 있다.

```tsx
function timerGame(callback) {
  console.log("Ready....go!");
  setTimeout(() => {
    console.log("Time's up -- stop!");
    callback && callback();
  }, 1000);
}

module.exports = timerGame;
```

```tsx
jest.useFakeTimers();
jest.spyOn(global, "setTimeout");

test("게임이 끝나기 전에 1초를 기다립니다.", () => {
  const timerGame = require("../timerGame");
  timerGame();

  expect(setTimeout).toHaveBeenCalledTimes(1);
  expect(setTimeout).toHaveBeenLastCalledWith(expect.any(Function), 1000);
});
```

### 모든 타이머 실행

- Jest 타이머 제어 API를 사용하여 테스트 중에 시간을 빠르게 진행할 수 있다.

```tsx
jest.useFakeTimers();
test("1초 후 콜백을 호출합니다.", () => {
  const timerGame = require("../timerGame");
  const callback = jest.fn();

  timerGame(callback);

  // 이 시점에서는 콜백이 아직 호출되지 않았어야 합니다.
  expect(callback).not.toHaveBeenCalled();

  // 모든 타이머가 실행될 때까지 빠르게 진행합니다.
  jest.runAllTimers();

  // 이제 콜백이 호출되었어야 합니다!
  expect(callback).toHaveBeenCalled();
  expect(callback).toHaveBeenCalledTimes(1);
});
```

### 대기 중인 타이머 실행

- 재귀 타이머와 같은 시나리오에서 타이머가 자신의 콜백에서 새로운 타이머를 설정한다.
- 이런 경우 `runAllTimers()` 을 사용하면 무한 루프에 빠져 오류가 발생한다.
- 이 경우 `jest.runOnlyPendingTimers()`를 사용하여 문제를 해결할 수 있다.

```tsx
function infiniteTimerGame(callback) {
  console.log("Ready....go!");

  setTimeout(() => {
    console.log("Time's up! 10초 후 다음 게임이 시작됩니다...");
    callback && callback();

    // 10초 후에 다음 게임을 예약합니다.
    setTimeout(() => {
      infiniteTimerGame(callback);
    }, 10000);
  }, 1000);
}

module.exports = infiniteTimerGame;
```

```tsx
jest.useFakeTimers();
jest.spyOn(global, "setTimeout");

describe("infiniteTimerGame", () => {
  test("1초 후 10초 타이머를 예약합니다.", () => {
    const infiniteTimerGame = require("../infiniteTimerGame");
    const callback = jest.fn();

    infiniteTimerGame(callback);

    // 이 시점에서는 1초 후에 게임이 종료되도록 setTimeout이 한 번 호출되었어야 합니다.
    expect(setTimeout).toHaveBeenCalledTimes(1);
    expect(setTimeout).toHaveBeenLastCalledWith(expect.any(Function), 1000);

    // 현재 대기 중인 타이머만 빠르게 진행합니다.
    // (해당 과정에서 생성된 새로운 타이머는 포함되지 않습니다.)
    jest.runOnlyPendingTimers();

    // 이 시점에서는 1초 타이머의 콜백이 실행되었어야 합니다.
    expect(callback).toHaveBeenCalled();

    // 그리고 10초 후에 게임을 다시 시작하도록 새로운 타이머가 생성되었어야 합니다.
    expect(setTimeout).toHaveBeenCalledTimes(2);
    expect(setTimeout).toHaveBeenLastCalledWith(expect.any(Function), 10000);
  });
});
```

### 타이머를 시간 단위로 진행

- `jest.advenceTimersByTime(msToRun)` 을 사용하면 API가 호출되면 모든 타이머가 `msToRun` 밀리초만큼 진행된다.
- 테스트 에서 대기 중인 모든 타이머를 지워야 하는 상황에는 `jest.clearAllTimers()`를 사용할 수 있다.

```tsx
function timerGame(callback) {
  console.log("Ready....go!");
  setTimeout(() => {
    console.log("Time's up -- stop!");
    callback && callback();
  }, 1000);
}

module.exports = timerGame;
```

```tsx
jest.useFakeTimers();
it("1초 후 콜백을 호출합니다 (advanceTimersByTime 사용)", () => {
  const timerGame = require("../timerGame");
  const callback = jest.fn();

  timerGame(callback);

  // 이 시점에서는 콜백이 아직 호출되지 않았어야 합니다.
  expect(callback).not.toHaveBeenCalled();

  // 모든 타이머가 실행될 때까지 빠르게 진행합니다.
  jest.advanceTimersByTime(1000);

  // 이제 콜백이 호출되었어야 합니다!
  expect(callback).toHaveBeenCalled();
  expect(callback).toHaveBeenCalledTimes(1);
});
```

