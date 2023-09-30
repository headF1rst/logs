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

