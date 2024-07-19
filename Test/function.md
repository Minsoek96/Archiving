# Mock Function

## mockFn.mock.calls

`mock.calls`는 mock함수가 호출될 때마다 그 호출 기록을 배열 형태로 저장한다.  
각 호출은 배열의 한 요소로 기록되며, 그 요소는 호출된 인자들의 배열이다.

### 에제

두 번 호출된 mock 함수 f가 있으며  
첫 번째 호출 시 인수로 f(`arg1`,`arg2`)가 전달되고,  
두 번째 호출 시 인수로 f(`arg3`,`arg4`)가 전달되었다면,

```tsx
[
  ["arg1", "arg2"],
  ["arg3", "arg4"],
];
```