#papercut 

`브로커(broker)`: 카프카가 설치되어 있는 **서버** 단위. 보통 3개 이상의 브로커로 구성하여 사용하는것을 권장한다.
### 파티션 replication

1개의 [[topic]]이 1개의 [[partition]]을 가지고 있고, `replication` 수가 3개이면 다음과 같이 분산된다.
![[스크린샷 2023-10-06 오후 1.05.16.png]]

단, `replication`수는 broker 수보다 많을 수 없다.

원본 [[partition]]은 `리더`가되고 나머지 두 개의 복제본 [[partition]]은 `팔로워`가 된다. 이러한 리더와 팔로워 partition을 합쳐서 `ISR (In Sync Replica)`라고 한다.
![[스크린샷 2023-10-06 오후 1.08.45.png]]

replication은 partition의 **고가용성**을 위해 사용된다. 위와 같은 설정에서 리더 partition이 속한 broker가 죽더라도, 팔로워 partition중 하나가 리더 partition 역할을 승계한다.

`리더 partition`: producer가 topic의 partition에 데이터를 전달할 때 전달받는 주체.
### producer의 ack 옵션

producer에는 `ack` 상세 옵션이 존재한다. `ack`는 `0`, `1`, `all` 옵션 중 하나를 선택할 수 있다.

`ack = 0`: producer는 리더 partition에 데이터를 전송하고 응답값을 받지 않는다.
- 데이터가 정상적으로 전송됐는지, 팔로워 partition에 데이터가 복제되었는지 알 수 없다.
- 때문에 속도는 빠르지만 데이터 유실 가능성이 있다.

`ack = 1`: producer는 리더 partition에 데이터를 전송하고 데이터가 정상적으로 전송됐다는 응답값을 받는다. 단, 팔로워 partition에 데이터가 복제되었는지 까지는 알 수 없다.
- 리더 partition이 데이터를 받은 즉시 브로커가 죽는다면 팔로워 partition에 데이터가 복제되지 못한다.
- 때문에 데이터 유실 가능성이 있다.

`ack = all`: 리더 partition에 데이터를 보낸 후, 팔로워 partition에 데이터가 복제되는지 확인하는 절차를 갖는다.
- 데이터 유실 가능성이 없다.
- 절차가 많기 때문에 속도가 느리다.

> 3개 이상의 broker 사용시 replication은 3으로 설정하는것을 권장