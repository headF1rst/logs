#papercut 

`Topic`: 카프카내 데이터가 들어가는 저장 공간

카프카에서는 토픽을 여러개 생성할 수 있으며 각 토픽은 목적에 따라 이름을 가질 수 있다. 

![[스크린샷 2023-10-06 오전 11.09.00.png]]

토픽은 데이터베이스 테이블과 유사한 성질을 갖고있다.

하나의 Topic은 여러개의 [[partition]]으로 구성될 수 있으며 파티션은 0번 부터 시작한다.

(Kafka > Topic > Partition)
