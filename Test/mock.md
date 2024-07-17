# 모의 함수 Mock Functions

- 모의 함수는 함수의 실제 구현을 제거하고 함수 호출, 전달된 매개변수, 생성자 | 함수 인스턴스 등을 캡처하며 테스트 시 반환 값을 설정할 수 있게 한다.
- 모의 함수는 코드 간의 연결을 테스트하는 데 사용된다.

## Mock 사용 유형

- 테스트 코드에서 사용할 모의 함수를 생성
- 모듈 종속성을 재정의하기 위해 `수동 모의`를 작성

### Mock 속성

- 모든 모의 함수에는 .mock 속성이 있으며, 이 속성은 함수 호출 방식, 반환 값, this 값등을 저장한다.
- `mock.instances`, `mock.contexts`, `mock.calls`, `mock.results` 등 다양한 속성을 통해 호출 정보를 검사할 수 있다.

```tsx
const myMock1 = jest.fn();
const a = new myMock1();
console.log(myMock1.mock.instances);
// > [ <a> ]

const myMock2 = jest.fn();
const b = {};
const bound = myMock2.bind(b);
bound();
console.log(myMock2.mock.contexts);
// > [ <b> ]
```

### Mock 반환 값

- mock은 테스트 중 코드에 테스트 값을 주입하는 데도 사용할 수 있다.

```tsx
const myMock = jest.fn();
console.log(myMock());
// > undefined

myMock.mockReturnValueOnce(10).mockReturnValueOnce("x").mockReturnValue(true);

console.log(myMock(), myMock(), myMock(), myMock());
// > 10, 'x', true, true
```

[2] 이해 못함

### 모듈 Mock

- 외부 모듈 (예: Axios)을 모의하여 실제 API 호출 없이 테스트할 수 있다.
- `jest.mock` 함수를 사용하여 모듈을 모의하고, 필요한 반환 값을 설정한다.

```tsx
import axios from "axios";
import Users from "./users";

jest.mock("axios");

test("사용자를 가져와야 합니다", () => {
  const users = [{ name: "Bob" }];
  const resp = { data: users };
  axios.get.mockResolvedValue(resp);

  // 또는 다음을 사용할 수 있습니다. 사용 사례에 따라 다릅니다:
  // axios.get.mockImplementation(() => Promise.resolve(resp))

  return Users.all().then((data) => expect(data).toEqual(users));
});
```

### 부분 Mock !!!!!!!!!!!

- 모듈의 일부만 Mock하고, 나머지는 실제 구현을 유지한다.
- `jest.requireActual`을 사용하여 실제 모듈의 나머지 부분을 유지하면서 특정 부분만 모의한다.

```tsx
export const foo = "foo";
export const bar = () => "bar";
export default () => "baz";
```

```tsx
//test.js
import defaultExport, { bar, foo } from "../foo-bar-baz";

jest.mock("../foo-bar-baz", () => {
  const originalModule = jest.requireActual("../foo-bar-baz");

  // 기본 내보내기와 'foo'라는 명명된 내보내기를 모의합니다.
  return {
    __esModule: true,
    ...originalModule,
    default: jest.fn(() => "mocked baz"),
    foo: "mocked foo",
  };
});

test("부분 모의 테스트", () => {
  const defaultExportResult = defaultExport();
  expect(defaultExportResult).toBe("mocked baz");
  expect(defaultExport).toHaveBeenCalled();

  expect(foo).toBe("mocked foo");
  expect(bar()).toBe("bar");
});
```

> export와 export default 모듈은 mock 방법이 다른 부분을 모르고 export 방식으로 통일하여 오류가 발생

### Mock 완전 구현

모의 함수의 구현을 완전히 교체해야 하는 경우가 있다.

- `jest.fn` 또는 mock 함수의 `mockImplementationOnece`, `mockImplementation`을 사용하여 Mock 함수의 구현을 설정할 수 있다.
- 이는 복잡한 동작을 재현하거나 여러 호출에 대해 다른 결과를 생성해야 할 때 유용하다.

Jest.fn

```tsx
const myMockFn = jest.fn((cb) => cb(null, true));

myMockFn((err, val) => console.log(val));
// > true
```

mockImplementation

```tsx
module.exports = function () {
  // 일부 구현;
};
```

```tsx
jest.mock("../foo"); // 자동 모의로 자동으로 발생합니다.
const foo = require("../foo");

// foo는 모의 함수입니다.
foo.mockImplementation(() => 42);
foo();
// > 42
```

: `mockImplementationOnce`로 정의된 구현을 모두 소진하면 `jest.fn`으로 설정된 기본 구현이 실행된다.

```tsx
const myMockFn = jest
  .fn(() => "default")
  .mockImplementationOnce(() => "first call")
  .mockImplementationOnce(() => "second call");

console.log(myMockFn(), myMockFn(), myMockFn(), myMockFn());
// > 'first call', 'second call', 'default', 'default'
```

### 체이닝된 메서드 모의

- 체이닝된 메서드는 `mockReturnThis()`를 사용하여 `this`를 반환하도록 설정할 수 있다.

```tsx
const myObj = {
  myMethod: jest.fn().mockReturnThis(),
};

// 다음과 동일합니다.

const otherObj = {
  myMethod: jest.fn(function () {
    return this;
  }),
};
```

### Mock 이름

- `mockName` 메서드를 사용하여 모의 함수에 이름을 설정할 수 있으며, 이는 테스트 오류 출력에서 유용하다.

```tsx
const myMockFn = jest
  .fn()
  .mockReturnValue("default")
  .mockImplementation((scalar) => 42 + scalar)
  .mockName("add42");
```

### 사용자 정의 매처

- mock 함수 호출 방식을 단언하기 위한 사용자 정의 매처 함수가 있다.
- `toHaveBeenCalled`, `toHaveBeenCalledWith`, `toHaveBeenLastCalledWith`,`toMatchSnapshot`등을 사용하여 함수 호출을 검사할 수 있다.

```tsx
// 모의 함수가 한 번 이상 호출되었는지 확인
expect(mockFunc).toHaveBeenCalled();

// 모의 함수가 지정된 인수로 한 번 이상 호출되었는지 확인
expect(mockFunc).toHaveBeenCalledWith(arg1, arg2);

// 모의 함수의 마지막 호출이 지정된 인수로 호출되었는지 확인
expect(mockFunc).toHaveBeenLastCalledWith(arg1, arg2);

// 모든 호출과 모의 이름이 스냅샷으로 기록되었는지 확인
expect(mockFunc).toMatchSnapshot();
```