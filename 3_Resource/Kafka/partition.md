#papercut 

하나의 Partition은 큐와 같이 내부에 데이터가 끝에서부터 순차적으로 저장된다.

Consumer는 `offset`을 사용하여 Partition의 데이터를 가장 오래된 순서대로(offset 0부터) 가져간다.
![[스크린샷 2023-10-06 오전 11.27.23.png]]
데이터가 들어오지않으면, 들어올 때 까지 Consumer는 대기한다.

Consumer가 파티션내 데이터를 가져가더라도 **데이터는 삭제되지 않는다**.
![[스크린샷 2023-10-06 오전 11.26.56.png]]

카프카의 이러한 성질 덕분에 새로운 Consumer가 붙었을때 이전 데이터를 다시 0번부터 가져가 사용할 수 있다. 즉, **동일 데이터에 대해서 두번 처리할 수 있다.** 단, 다음 제약이 만족하는 상황에서만 가능하다.
- Consumer 그룹이 다른 경우
- `auto.offset.reset = earliest`인 경우
### 예제
로그를 분석하고 시각화하기 위해 ElasticSearch에 로그를 저장하고, 백업을 위해 Hadoop에도 로그를 저장하는 케이스가 있다.
![[스크린샷 2023-10-06 오전 11.33.40.png]]
### 키 할당 정책

한 Topic에 Partition이 여러개인 경우, 키는 정책에 따라 특정 Partition으로 할당된다.

- 키가 null이고, 기본 [[partitioner]] 사용할 경우 -> `Round robin`으로 할당.
- 키값이 존재하고, `기본 파티셔너` 사용할 경우 -> 키의 해시(hash)값을 구하고, 특정 파티션에 할당.


Partition 개수를 늘리면 Consumer 개수를 늘려서 데이터 처리를 분산 시킬 수 있다.
Partition을 늘릴 수는 있지만 줄일 수는 없다. Partition을 늘릴때는 신중해야한다.
### 파티션내 데이터 삭제

Partition에 저장된 데이터(레코드)는 설정값에 따라서 삭제된다.
레코드가 저장되는 `최대 시간`과 `크기`를 지정하여 적절하게 데이터가 삭제될 수 있도록 설정할 수 있다.

- `log.retention.ms`: 최대 record 보존 시간
- `log.retention.byte`: 최대 record 보존 크기 (byte)
