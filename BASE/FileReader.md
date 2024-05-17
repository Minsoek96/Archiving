# FileReader

> 웹 어플리케이션에서 파일을 비동기적으로 읽기 위한 API이다. 주로 파일 입력 요소를 통해 사용자가 업로드한 파일을 읽고, 해당 파일의 내용을 처리하는 데 사용된다.

## 주요 메서드

### readAsArrayBuffer(file)

- 파일을 `ArrayBuffer`로 읽는다.
- `ArrayBuffer`는 이진 데이터를 다루기 위한 일반적인 용도로 사용된다.

### readAsBinaryString(file)

- 파일을 이진 문자열로 읽는다.
- 해당 방법은 거의 사용되지 않는다.

### readAsDataURL(file)

- 파일을 Base64 인코딩된 데이터 URL로 읽는다.
- 이미지 파일을 포함하여 파일을 웹 페이지에서 직접 사용하려고 할 때 유용하다.

### readAsText(file, [encoding])

- 파일을 텍스트로 읽는다.
- 두 번 째 매개 변수로 인코딩을 지정할 수 있다.(EX: UTF-8)

---

## 주요 이벤트

`FileReader` 객체는 파일 읽기 작업의 상태를 모니터링하기 위해 여러 이벤트를 제공한다.

**onloadstart** : 읽기 작업이 시작될 때 발생한다.

**onprogress** : 읽기 작업이 진행되는 동안 주기적으로 발생한다.

**onload** : 읽기 작업이 중단되었을 때 발생한다.

**onerror** : 읽기 작업 중 오류가 발생했을 때 발생한다.

**onloadend** : 읽기 작업이 완료되었을 때 발생한다.(항상 )

---

## createObjectURL이란

> 웹 API중 하나로, 브라우저가 특정한 파일이나 데이터를 나타내는 URL을 생성하게 해준다. 해당 URL은 그 데이터를 가르키는 고유한 주소로, 주로 파일 객체를 다루는데 사용된다.

### 기본 개념

`createObjectURL` 메서드는 `URL` 인터페이스의 메서드로 파일이나 데이터 블롭 (`Blob`), 파일(`File`), 또는 미디어 스트림(`MediaSource` 나 `MediaStream`) 같은 객체를 받는다. 해당 메서드는 임시 URL을 반환한다.  
**URL은 브라우저의 메모리에 저장 , 브라우저 세션 동안만 유효한다.**

```html
<input type="file" id="input" />
<img id="preview" />
<script>
  document.getElementById('input').addEventListener('change', function(event) {
    const file = event.target.files[0];
    const objectURL = URL.createObjectURL(file);
    document.getElementById('preview').src = objectURL;
  });
</script>
```
  
### URL 해제 

`createObjectURL`로 생성된 URL은 브라우저의 메모리를 차지하므로 더 이상 피룡하지 않으면 해제해주는 것이 좋다. 이를 위해 `revokeObjectURL` 메서드를 사용한다.

`URL.revokeObjectURL(objectURL);`

### 요약 

| 특징 | FileReader | createObjectURL |
| --- | --- | --- |
| 목적 | 파일 내용을 읽어 처리 | 파일이나 데이터를 URL로 변환 |
| 작업 방식 | 파일 내용을 메모리에 읽음 | 객체를 가리키는 URL 생성 |
| 사용 예 | 텍스트 파일 읽기, 데이터 URL 변환 | 이미지 미리보기, 파일 다운로드 링크 |
| 비동기 처리 | 비동기 (이벤트 기반) | 동기 (즉시 URL 반환) |
| 메모리 사용 | 파일 내용을 메모리에 저장 | 객체를 가리키는 URL 생성 (메모리 효율적) |
| 주 사용 상황 | 파일 내용을 읽어야 할 때 | 파일을 바로 사용하거나 미리보기 할 때 |