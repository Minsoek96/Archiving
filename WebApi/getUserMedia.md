# getUserMedia

`MediaDevices` 인터페이스의 `getUserMedia()` 메서드는 사용자가 미디어 입력 장치(카메라, 마이크)에 접근할 수 있도록 허용을 요청한다. 이를 통해 요청된 유형의 미디어 트랙을 포함하는 `MediaStream` 객체를 생성할 수 있다.

- 기본적으로 활용도 가 높다
- HTTPS와 같은 보안 컨텍스트에서만 사용 가능하다.

> `MediaStream` 객체를 반환하는 `Promise`를 반환한다.  
> Promise는 각각 `NotAllowedError` 또는 `NotFoundErro` DOMException과 함께 거부한다.

## DOMException 이란

웹 API에서 특정 오류가 발생할 때 사용되는 예외 타입이다.

- DOMException 객체는 `name` 와 `message`라는 두 가지 주요 속성을 가진다.
- name : `NotFoundError`, `SecurityError`, `NotAllowedError` 등이 있다.
- message : 오류에 대한 자세한 설명을 담고 있는 메시지

## 주요 사용법

```tsx
navigator.mediaDevices
  .getUserMedia(constraints)
  .then((stream) => {
    /* use the stream */
  })
  .catch((err) => {
    /* handle the error */
  });
```

### constraints

- `getUserMedia()`에 전달되는 객체로, 요청하는 미디어 유형과 해당 유형의 제약 조건을 지정한다.
- `video`와 `audio` 라는 두 가지 멤버를 포함할 수 있으며, 각각의 값은 `true`, `false` 또는 더 세부적인 제약조건이 포함된 객체이다.

[기본제어]

```tsx
getUserMedia({
  audio: true,
  video: true,
});
```

[해상도 제어]

```tsx
getUserMedia({
  audio: true,
  video: {
    width: { min: 1024, ideal: 1280, max: 1920 },
    height: { min: 576, ideal: 720, max: 1080 },
  },
});
```

- 가로 해상도 최소 1024, 이상적인 해상도 1280, 최대 1920
- 세로 해상도 최소 576, 이상적인 해상도 720, 최대 1080

[카메라 전후면 제어]

```tsx
getUserMedia({
  audio: true,
  video: { facingMode: "user" },
});
```

- 모바일의 경우 대부분의 유저가 후면 보단 전면을 선호한다.
- 전면 카메라의 경우 `user` 후면 카메라의 경우 `facingMode: { exact: "environment"}`

[카메라 디바이스 제어]

```tsx
getUserMedia({
  video: {
    deviceId: myPreferredCameraDeviceId,
  },
});
```

- `deviceId` 제약 조건을 직접 사용하면, 해당 장치가 사용 가능한 경우 그 장치를 선택한다.
- 해당 장치가 제약이 있는 경우 다른 장치를 선택한다.

```tsx
getUserMedia({
  video: {
    deviceId: {
      exact: myExactCameraOrBustDeviceId,
    },
  },
});
```

- 특정 장치를 반드시 사용해야 한다면 `deviceId` 제약 조건에 `exact` 키워드를 사용할 수 있다.

### enumerrateDevices() [디바이스 아이디 얻는 방법]

`enumerateDevices()` 메서드는 마이크, 카메라, 헤드셋 등 현재 사용 가능한 미디어 입력 및 출력 장치의 목록을 요청한다. 반환된 프로미스는 디바이스를 설명하는 MediaDeviceInfo 객체 배열로 확인된다.

```tsx
if (!navigator.mediaDevices?.enumerateDevices) {
  console.log("enumerateDevices() not supported.");
} else {
  // List cameras and microphones.
  navigator.mediaDevices
    .enumerateDevices()
    .then((devices) => {
      devices.forEach((device) => {
        console.log(`${device.kind}: ${device.label} id = ${device.deviceId}`);
      });
    })
    .catch((err) => {
      console.error(`${err.name}: ${err.message}`);
    });
}
```
