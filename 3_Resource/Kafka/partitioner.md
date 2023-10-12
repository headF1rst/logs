#papercut 

`partitioner`: 데이터(레코드)를 [[topic]]의 어떤 [[partition]]에 넣을지 결정. (기본값 = `UniformStickyPartitioner`)

레코드에 포함된 `메세지 키` 혹은 `메세지 값`에 따라서 partition의 위치가 결정된다. producer가 데이터를 발행하면, `partitioner`를 통해서 [[broker]]로 데이터가 전송된다.

![[스크린샷 2023-10-06 오후 1.30.19.png|150]]
### UniformStickyPartitioner
레코드에 `메세지 키` 존재 여부에 따라 다르게 동작한다.

**`메세지 키가 존재하는 경우`**
`partitioner`는 해시값(`hash(메시지 키)`)을 기준으로 어느 partition에 배정할지 결정한다.

동일한 `메세지 키`를 갖는 레코드는 동일한 해시값을 생성하기 때문에 항상 동일한 파티션에 저장되는것을 보장한다. 덕분에 순서를 지켜서 데이터를 처리할 수 있게 된다.

**`메세지 키가 존재하지 않는 경우`**
`Round robin`으로 레코드가 partition에 배정된다.
producer에서 배치로 모을 수 있는 최대한의 레코드를 모아서 partition으로 보낸다.
### partitioner interface

partitioner interface를 통해서 `커스텀 파티셔너 클래스`를 구현하여 `메세지 키`, `메세지 값`, `topic 이름` 등에 따라서 partition 할당 정책을 정할 수 있다.

10개의 partition 중 8개를 VIP 고객의 데이터 전용으로 할당해서, VIP 고객의 처리량을 늘릴수 있게 커스텀 파티셔너 클래스를 구현할 수도 있다.

