#papercut #Database 

`Index`: **정렬되어** 메모리에 저장된 컬럼 복사본.

![[스크린샷 2024-01-08 오전 9.47.25.png|300x200]]
- 인덱스를 통해서 데이터를 찾고, 인덱스와 연결된 원래 테이블 행을 가져온다.
- 인덱스가 없다면 특정 조건의 데이터를 찾기 위해서 테이블 내 모든 데이터를 탐색해야한다. ([[Full Table Scan]])
- auto increment와 같은 PK는 자동으로 정렬되어있기 때문에 별도의 인덱스 생성없이 인덱스와 같은 효과를 가질 수 있다. (`Cluster Index`)

단점
- 인덱스는 컬럼을 복사해서 정렬해놓은 복사본이기 때문에 용량을 차지하게 된다.
- 데이터 수정, 삭제, 삽입 연산시에 인덱스 복사본에도 반영해줘야하기 때문에 비용이든다.
	- 수정, 삭제, 삽입 연산을 하기위해서 데이터를 조회하는 과정이 필요하기 때문에 인덱스가 더 효율적일 수 있다.
### 인덱스 컬럼 선택 기준

`카디널리티 (Cardinality)`: 중복 수치. 카디널리티가 높을수록 고유값.

> 1개의 컬럼만 인덱스를 걸어야할 경우.

카디널리티가 가장 높은 컬럼을 선택.

> 여러 컬럼으로 인덱스 구성할 경우.

카디널리티가 높은 컬럼부터 낮은 컬럼순으로 선언해서 조합.

```sql
CREATE INDEX IDX_SALARIES_DECREASE ON salaries 
(group_no, from_date, is_bonus);
```

인덱스 컬럼중 첫번째 인덱스 조건을 where 절에 포함시키지 않으면 인덱스를 타지 않는다.
![[스크린샷 2024-01-03 오후 9.39.46.png]]

`from_date`을 제외한 조회 쿼리는 filtered가 10%로 효율적이진 않지만 인덱스를 탄다.
![[스크린샷 2024-01-03 오후 9.41.03.png]]
### 인덱스 조회시 주의사항

- 범위 조건의 피연산 컬럼은 인덱스를 타지만, 이 후의 인덱스 컬럼들은 인덱스를 타지 않는다. (`between`, `like`, `<`, `>`)
```sql
where group_no=XX and is_bonus=YY and from_date > ZZ 
```

범위 조건(`>`)의 피연산 컬럼인 `from_date` 이 후에 선언되어 조합된 `is_bonus`는 인덱스를 타지 못한다.

- `=`, `in`의 인자로 상수가 아닌 서브 쿼리가 들어가면, 서브 쿼리의 외부가 먼저 수행되고 `in`은 체크 조건으로 실행되기 때문에 성능 이슈가 발생한다.

- `AND`연산은 쿼리 대상 row를 줄이지만 `OR` 연산자는 비교해야할 row수를 늘리기 때문에 [[Full Table Scan]] 확률이 높아진다.

- 인덱스는 가공된 데이터를 저장하고 있지 않기 때문에 인덱스로 사용되는 컬럼은 있는 그대로 사용해야만 인덱스를 탄다.
	- where salary * 10 > 1500; 은 인덱스를 안타지만 
		where salary > 1500 / 10은 인덱스를 탄다.
	- 타입이 같아야 한다.

- null값의 경우 `is null` 조건으로 [[Index Range Scan]]이 가능하다.



[쿼리 최적화: 빠른 쿼리를 위한 7가지 체크리스트](https://medium.com/watcha/%EC%BF%BC%EB%A6%AC-%EC%B5%9C%EC%A0%81%ED%99%94-%EC%B2%AB%EA%B1%B8%EC%9D%8C-%EB%B3%B4%EB%8B%A4-%EB%B9%A0%EB%A5%B8-%EC%BF%BC%EB%A6%AC%EB%A5%BC-%EC%9C%84%ED%95%9C-7%EA%B0%80%EC%A7%80-%EC%B2%B4%ED%81%AC-%EB%A6%AC%EC%8A%A4%ED%8A%B8-bafec9d2c073)
https://jojoldu.tistory.com/243