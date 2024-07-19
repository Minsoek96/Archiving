# The jest Object

## jest.spyOn(object, methodName)

`jest.spyOn`은 `jest.fn`와 유사하게 모의 함수를 생성하지만,  
`object[methodName]`에 대한 호출을 추적한다.  
jest 모의 함수를 반환한다.

### 함수 덮어쓰기

`jest.spyOn`은 스파이한 메서드를 호출한다. 원래 함수를 덮어쓰고 싶다면,

`jest.spyOn(object, methodName).mockImplementation(()=> customImplementation)` 또는
`object[methodName] = jest.fn(()=> customImplementation)`
을 사용하면 된다.

```tsx
const video = {
  play() {
    return true;
  },
};

export default video;
```

```tsx
import video from "./video";

afterEach(() => {
  jest.restoreAllMocks();
});

test("plays video with mock implementation using jest.spyOn", () => {
  const spy = jest.spyOn(video, "play").mockImplementation(() => false);
  const isPlaying = video.play();

  expect(spy).toHaveBeenCalled();
  expect(isPlaying).toBe(false);
});
```

### using 키워드와 함께

TypeScript >= 5.2 또는 @babel/plugin-proposal-explicit-resource-management 플러그인을 사용하는 경우, using 키워드와 함께 spyOn을 사용할 수 있다

```tsx
test('logs a warning', () => {
  using spy = jest.spyOn(console.warn);
  doSomeThingWarnWorthy();
  expect(spy).toHaveBeenCalled();
});

```

이 코드는 다음과 같다.

```tsx
test("logs a warning", () => {
  let spy;
  try {
    spy = jest.spyOn(console.warn);
    doSomeThingWarnWorthy();
    expect(spy).toHaveBeenCalled();
  } finally {
    spy.mockRestore();
  }
});
```

### getter 와 setter

jest 22.1.0 부터 `jest.spyOn` 메서드는 선택적인 세 번째 인수 `accessType`을 받을 수 있으며, 이는 `get` 또는 `set`이 될 수 있다.

```tsx
const video = {
  // 이건 getter입니다!
  get play() {
    return true;
  },
};

module.exports = video;

const audio = {
  _volume: false,
  // 이건 setter입니다!
  set volume(value) {
    this._volume = value;
  },
  get volume() {
    return this._volume;
  },
};

module.exports = audio;
```

```tsx
const audio = require("./audio");
const video = require("./video");

afterEach(() => {
  // spyOn으로 생성된 스파이를 복원합니다
  jest.restoreAllMocks();
});

test("plays video", () => {
  const spy = jest.spyOn(video, "play", "get"); // 'get'을 전달합니다
  const isPlaying = video.play;

  expect(spy).toHaveBeenCalled();
  expect(isPlaying).toBe(true);
});

test("plays audio", () => {
  const spy = jest.spyOn(audio, "volume", "set"); // 'set'을 전달합니다
  audio.volume = 100;

  expect(spy).toHaveBeenCalled();
});
```
