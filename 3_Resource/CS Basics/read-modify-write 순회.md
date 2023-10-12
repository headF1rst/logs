[PostgreSQL anti-patterns: read-modify-write cycles](https://www.2ndquadrant.com/en/blog/postgresql-anti-patterns-read-modify-write-cycles/ "Permanent Link: PostgreSQL anti-patterns: read-modify-write cycles")

`read-modify-write` 순회는 자주 볼수있는 SQL 안티패턴이다. read-modify-write이란 무엇이고 어떻게 식별할 수 있는지, 이 문제를 해결하기 위한 방법에는 무엇이 있는지 알아보자.

어떤 회원의 잔고에서 100만원을 출금한다고 가정해볼때, 코드는 다음과 같이 작성될 수 있다. (잔고가 음수가 되는 경우는 없다.)
```sql
SELECT balance FROM accounts WHERE user_id = 1;
-- 어플리케이션 코드에서 잔고가 100만원 이상이면 출금을 진행한다.
-- 출금후 남은 금액: ?이 새로운 잔고값이다.
UPDATE accounts SET balance = ? WHERE user_id = 1;
```

위 쿼리문은 문제없이 동작할것 처럼 보이지만 다른 세션에서 동일한 회원이 동시에 갱신된다면 결함을 야기할 수 있다.

회원_1의 잔고에는 300만원이 있고, 동시에 두개의 세션이 회원_1의 잔고로부터 각각 100만원씩 출금하는 경우를 가정해보자.

|Session 1|Session 2|
|-|-|
|SELECT balance FROM accounts WHERE user_id = 1; (300이 조회된다)||
||SELECT balance FROM accounts WHERE user_id = 1; (300이 조회된다)|
|UPDATE balance SET balance = 200 WHERE user_id = 1; (300 - 100 = 200)||
||UPDATE balance SET balance = 200 WHERE user_id = 1; (300 - 100 = 200)|

100만원이 두번 출금되었기 때문에 잔고에는 100만원이 남아있어야하지만 200만원이 남아있는걸 확인할 수 있다.

대부분의 개발과 테스트는 하나의 세션에서 돌아가는 standalone 서버에서 수행되기 때문에 엄밀한 테스트를 진행하지 않는한 운영에 나가기 전까지 이러한 결함을 발견하기 어려우며 디버깅하기도 어렵다.

이런 결함에 대해 인지하고 코드를 방어적으로 작성하는게 중요하다.
### 트랜잭션이 이러한 문제를 해결해주지 않나?

트랜잭션은 만능이 아니며 단순히 트랜잭션을 걸어주는것 만으로 동시성 문제를 해결하지는 못한다.
트랜잭션을 걸어주더라도 위의 문제는 그대로 발생한다.
### 해결책

대부분의 SQL은 동시성 문제를 다루기 위한 몇가지 방법을 제공하고 있다. 가장 대표적인 [[read-modify-write 해결책]]은 다음과 같다.
- `read-modify-write` 순회를 회피한다.
- 로우 레벨 잠금을 사용한다. `SELECT ...FOR UPDATE`
- `SERIALIZABLE` 트랜잭션 사용.
- Optimistic Lock 사용.