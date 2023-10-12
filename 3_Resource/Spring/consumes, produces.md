`consumes`와 `produces`를 사용하면 Mapping을 할 때 받고싶은 데이터를 강제 함으로써 오류상황을 줄일 수 있다.

`consumes`: 클라이언트가 서버에게 보내는 데이터 타입을 명시한다.
`produces`: 서버가 클라이언트에게 반환하는 데이터 타입을 명시한다.

Multipart file을 받고, Json 데이터를 반환하도록 명시하고 싶다면 다음과 같이 처리할 수 있다.
```java
@PostMapping(value = "/excel/upload", consumes = MediaType.MULTIPART_FORM_DATA_VALUE, produces = MediaType.APPLICATION_JSON_UTF8_VALUE)

public ResponseEntity<String> tcOrderRegisterExcelUpload(MultipartFile uploadFile) throws Exception {
	// ...
}
```

이 경우 해당 api를 호출하는 클라이언트는 헤더에 보내는 데이터 타입을 명시해줘야한다.
```
Content-Type: multipart/form-data
```

요청하는 입장에서 특정 타입의 데이터를 원한다면 다음 내용을 헤더에 추가한다.
```
Accept:application/json
```
