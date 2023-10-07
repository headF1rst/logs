`lag`: producer offset과 consumer offset의 차이. 주로 consumer의 상태를 모니터링 하는 용도로 사용된다.

producer가 데이터를 발행하는 속도가 consumer가 소비하는 속도보다 빠르면 `consumer lag`이 발생한다.

![[스크린샷 2023-10-06 오후 2.10.18.png|200]]

[[topic]]에 여러 [[partition]]이 존재할 경우, `lag`은 여러개 존재할 수 있다. 다음과 같은 경우 `lag`이 2개 존재한다.

`records-lag-max`: 가장 큰 lag 값

![[스크린샷 2023-10-06 오후 2.12.46.png|200]]