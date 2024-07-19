# jest 문법 사전

## 기본 매처

### toBe

- 값을 비교할 때 사용한다. `===`와 같은 방식으로 비교한다.
- `expect(value).toBe(42);`

### toEqual

- 객체나 배열의 값을 비교할 때 사용한다.
- `expect(object).toEqual({ name: 'John' });`

### toBenull

- 값이 null인지 확인한다.
- `expect(value).toBeNull();`

### toBeUndefined

- 값이 undefined인지 확인한다.
- `expect(value).toBeUndefined();`

### toBeDefined

- 값이 정의되어 있는지 확인한다.
- `expect(value).toBeDefined();`

### toBeTruthy

- 값이 참인지 확인한다.
- `expect(value).toBeTruthy();`

### toBeFalsy

- 값이 거짓인지 확인한다.
- `expect(value).toBeFalsy();`

## 숫자 관련 매처

### toBeGreaterThan

- 값이 특정 값보다 큰지 확인한다.
- `expect(value).toBeGreaterThan(10);`

### toBeGreaterThanOrEqual

- 값이 특정 값보다 크거나 같은지 확인한다.
- `expect(value).toBeGreaterThanOrEqual(10);`

### toBeLessThan

- 값이 특정 값보다 작은지 확인한다.
- `expect(value).toBeLessThan(10);`

### toBeLessThanOrEqual

- 값이 특정 값보다 작거나 같은지 확인한다.
- `expect(value).toBeLessThanOrEqual(10);`

## 문자열 관련 매처

### toMatch

- 문자열이 정규 표현식과 일치하는지 확인한다.
- `expect(string).toMatch(/regex/);`

## 배열 및 반복자 관련 매처

### toContain

- 배열이나 반복자에 특정 항목이 포함되어 있는지 확인한다.
- `expect(array).toContain(item);`

## 예외 처리 매처

### toThrow

- 함수가 예외를 던지는지 확인한다.
- `expect(() => { throw new Error('error'); }).toThrow('error');`

## 모의 함수 관련 매처

### toHaveBeenCalled

- 함수가 한 번 이상 호출되었는지 확인한다.
- `expect(mockFunc).toHaveBeenCalled();`

### toHaveBeenCalledTimes

- 함수가 특정 횟수만큼 호출되었는지 확인한다.
- `expect(mockFunc).toHaveBeenCalledTimes(2);`

### toHaveBeenCalledWith

- 함수가 특정 인수와 함께 호출되었는지 확인한다.
- `expect(mockFunc).toHaveBeenCalledWith(arg1, arg2);`

### toHaveBeenLastCalledWith

- 함수가 마지막으로 호출된 인수를 확인한다.
- `expect(mockFunc).toHaveBeenLastCalledWith(arg1, arg2);`

### toHaveBeenNthCalledWith

- 함수가 n번째로 호출된 인수를 확인한다.
- `expect(mockFunc).toHaveBeenNthCalledWith(1, arg1, arg2);`

### toHaveReturned

- 함수가 반환되었는지 확인한다.
- `expect(mockFunc).toHaveReturned();`

### toHaveReturnedTimes

- 함수가 특정 횟수만큼 반환되었는지 확인한다.
- `expect(mockFunc).toHaveReturnedTimes(2);`

### toHaveReturnedWith

- 함수가 특정 값을 반환했는지 확인한다.
- `expect(mockFunc).toHaveReturnedWith(value);`

### toHaveLastReturnedWith

- 함수가 마지막으로 특정 값을 반환했는지 확인한다.
- `expect(mockFunc).toHaveReturnedWith(value);`

### toHaveNthReturnedWith

- 함수가 n번째로 특정 값을 반환했는지 확인한다.
- `expect(mockFunc).toHaveNthReturnedWith(1, value);`


