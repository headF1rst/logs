- `read-modify-write` 순회를 회피한다.
- 로우 레벨 잠금을 사용한다. `SELECT ...FOR UPDATE`
- `SERIALIZABLE` 트랜잭션 사용.
- Optimistic Lock 사용.
### read-modify-write 순회 회피

read-modify-write 순회의 가장 좋은 해결책은 문제 자체를 회피하는것이다.

|Session 1| Session 2|
|-|-|
|UPDATE accounts SET balance = balance - 100 WHERE user_id = 1; (잔고 = 200)||
||UPDATE accounts SET balance = balance - 100 WHERE user_id = 1; (잔고 = 100)|

두 트랜잭션이 동시에 수행되더라도 첫번째 세션이 로우에 대한 락을 획득하고 수행되기 때문에 두번째 세션은 첫번째 세션이 작업을 마치고 락을 해제할때 까지 대기해야한다.

이러한 해결책은 단순한 케이스에서만 사용가능하다. 현재 잔고상태에 따라 출금여부를 결정해야하는것과 같이 복잡한 로직이 포함되어야 하는 경우에는 활용하기 힘들다.
### 로우 레벨 잠금

`SELECT FOR UPDATE`문을 사용하면, 락을 획득하고 해당 트랜잭션이 커밋되거나 롤백되기 전까지 수정하기 위해 조회하는 다른 트랜잭션의 접근을 차단한다.

|Session 1| Session 2|
|-|-|
|SELECT balance FROM accounts WHERE user_id = 1 FOR UPDATE; (300)||
||SELECT balance FROM accounts WHERE user_id = 1 FOR UPDATE; (트랜잭션 1이 커밋될때까지 기다린다)|
|UPDATE accounts SET balance = 200 WHERE user_id = 1; (잔고 = 200)||
|COMMIT||
||두번째 트랜잭션이 200을 조회한다.|
||UPDATE accounts SET balance = 100 WHERE user_id = 1; (잔고 = 100)|
||COMMIT|

단, 로우 레벨 잠금은 read-modify-write 순회가 단일 트랜잭션안에 포함되어있을때만 동작한다. 락이 트랜잭션 수명 동안만 존재하기 때문이다.
### SERIALIZABLE 트랜잭션

read-modify-write 순회가 항상 단일 트랜잭션에 포함되어있고 PostgreSQL 9.1버전 이상을 사용한다면 `SELECT FOR UPDATE` 대신 `SERIALIZABLE`을 사용할 수 있다.

두개의 트랜잭션이 각각 동시에 같은 로우의 데이터를 조회하고 값을 변경한다고 하자. 먼저 락을 획득한 트랜잭션1 은 값을 변경하고 정상적으로 커밋되지만, 대기하고 있던 트랜잭션2는 처음 조회한 값이 변경되었다는 사실을 인지하고 serialization 에러와 함께 트랜잭션을 롤백시킨다.

이후 재시도 로직을 통해서 롤백된 트랜잭션을 다시 수행하도록 할 수 있다. 
로우 잠금을 계속 시도하는것이 데드락을 유발할 수 있는 케이스에 유용하다.
### Optimistic 동시성 제어

낙관적 락(Optimisitc Lock)은 ORM에 의해 어플리케이션단에서 동시성 제어 로직으로 구현된다.

낙관적 락이 적용된 모든 테이블은 `version` 혹은 `last-updated timestamp` 컬럼을 갖고있다. 모든 UPDATE 로직은 WHERE절을 통해서 version을 확인하는데, 조회 시점의 version과 변경을 시도했을때의 version값이 다르다면 다른 트랜잭션이 로우를 변경했다는 것을 의미하기 때문에 수행중이던 트랜잭션을 롤백한다.

SERIALIZABLE과 달리 자동 커밋 모드에서도 동작하며, 여러 트랜잭션에 속해있더라도 동작한다.
![[스크린샷 2023-10-01 오후 2.31.34.png]]