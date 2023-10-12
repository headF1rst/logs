카프카 클러스터를 모니터링하고 관리하기 위한 UI.
### 메인 대시보드 
![[스크린샷 2023-10-09 오후 1.49.09.png]]
- `Online`: 현재 UI와 연결되어 정상 수행되고 있는 클러스터 개수 - 1개로 되어있다.
- `Borkers count`: 현재 카프카 [[broker]] 개수.
- `Partition`: 현재 존재하는 카프카 [[partition]] 개수.
- `Topics`: 클러스터에서 이용하는 [[topic]]. 기본 토픽(1)이 생성되어 있어 1개로 지정되어있다.
- `Production`: 발행한 메시지 용량.
- `Consumption`: 수신한 메시지 용량.
### Broker 목록화면
![[스크린샷 2023-10-09 오후 2.01.30.png]]
- Uptime
	- `Total Brokers`: 총 [[broker]] 개수.
- Partitions
	- `Online`: 현재 온라인 개수는 1.
	- `URP (Un Replication Partitions)`: 복제가 덜 된 파티션 수.
	- `ISR (In Sync Replicas)`: 싱크 레플리카 3개로 현재 모두 싱크 되어있다.
	- `OSR (Out of Sync Replicas)`: 싱크가 맞지 않는 레플리카 수. - 0개


 
https://devocean.sk.com/blog/techBoardDetail.do?ID=163980#none