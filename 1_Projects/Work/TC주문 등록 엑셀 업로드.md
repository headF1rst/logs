`POST api/tcorders/add/excel/upload`

- 엑셀 크기, 타입 체크. 
- 권한 타입이 Vendor면 vendorId 가져오기
- n건 주문 등록. (주문 등록 건수 n이 반환된다.)
	- 엑셀 파일 데이터에 대한 검증
	- 택배대행외 다른 배송 타입이 엑셀에 포함되어있다면 업로드 가능 시간을 검증해야한다.
		회차 마감시간에 따른 주문등록 컷오프 시간이 존재.![[스크린샷 2023-10-10 오후 2.03.22.png]]
	- [[ApplicationEventPublisher]]로 `tcOrderList` 이벤트(tcOrderSendEvent) 발행.
	- `EventMessageListener.onSendTcOrder()`에 의해서 이벤트가 [[TC 주문 전송]]에 사용된다.


주문 등록 -> 주문 리스트 이벤트 발행 -> 주문 등록 완료 이벤트 발행 -> 주문 등록 이벤트 리스너 동작 -> 주문 전송 (자동 전송)

주문 등록시 [[주소 정제]]도 함께 이루어진다. 