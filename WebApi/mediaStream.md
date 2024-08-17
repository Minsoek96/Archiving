# MediaStream 

MediaStream() 생성자는 새로 생성된 미디어 스트림을 반환하며, 이 스트림은 미디어 트랙의 걸렉션 역할을 하며 각각 `MediaStreamTrack` 객체로 표현된다.

- MediaStream : 하나 이상의 `MediaStreamTrak`을 포함하는 컨테이너 객체 
- MediaStreamTrack : 오이오 또는 비디오 데이터의 개별 스트림을 나타내는 객체 

## 미디어 스트림의 주요 속성 및 메서드 

### 속성 

- active : 스트림이 활성화되어 있는지 여부를 나타낸다.
- id : 스트림의 고유 ID를 나타낸다.

### 메서드

- `getTracks()` : 스트림에 포함된 모든 트랙을 반환한다.
- `getAudioTracks()` : 스트림에 포함된 모든 오디오 트랙을 반환한다.
- `getVideoTracks()` : 스트림에 포함된 모든 비디오 트랙을 반환한다.
- `addTrack(track)` : 스트림에 새로운 트랙을 추가한다.
- `removeTrack(track)` : 스트림에서 특정 트랙을 제거한다.
- `clone()` : 스트림의 복사본을 생성한다.

### srcObject 속성 

`getUserMedia`를 통해 얻은 `MediaStream`을 비디오 또는 오디오 요소에 연결할 때 사용된다. 예전에는 `src` 속성을 사용하여 미디어 파일의 URL을 지정했지만, `MediaStream` 처럼 실시간 미디어 데이터를 다룰 떄는 `srcObject`를 사용해야 한다.


## MediaRecorder 

미디어를 쉽게 녹화할 수 있는 기능을 제공한다. 이 API는 특히 사용자가 직접 오디오나 비디오를 녹화하고 파일로 저장하거나 다른 서비스로 전송하는 기능을 구현할 때 유용하다.

> MediaRecorder 는 MediaStream을 입력으로 받아 이를 녹화한 뒤 Blob 형태로 제공할 수 있다.

### 속성 

mimeType : MediaRecorder 객체가 생성될 때 녹화 컨테이너로 선택된 MIME 유형을 반환한다.
state : MediaRecorder 객체의 현재 상태를 반환한다.
stream : 미디어 레코드를 생성할 때 생성자에 전달된 스트림을 반환한다.

### 메소드 

start() :  녹화를 시작한다.
stop() : 녹화를 중지한다.
pause() : 녹화를 일시 중지한다.
resume() : 일시 중지된 녹화를 재개한다.
requestData() : 현재까지 녹화된 데이터를 수집하는 `dataavailabel` 이벤트를 즉시 발생시킨다.

### dataavailable

미디어 녹화가 진행되는 동안 특정 간격으로, 또는 녹화가 중지될 때 발생하며, 이때 녹화된 데이터를 포함한 Blob 객체가 생성된다.

`dataavailable` 이벤트가 발생할 때마다 이벤트 객체에는 `Blob` 형태의 미디어 데이터가 포함된다. 이 데이터는 비디오 또는 오디오 데이터가 될 수 있으며, 이를 사용하여 파일로 저장하거나 재생할 수 있다.

[이벤트 발생 시점]

- MediaRecorder의 start() 메서드에 `timeslice`를 지정한 경우 지정된 간격마다 발생한다.
- MediaRecorder의 `requestData()` 메서드를 호출했을 때,
- MediaRecorder의 `stop()` 메서드를 호출하여 녹화를 중지했을 때