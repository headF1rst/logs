#Database 

컬럼에 데이터를 유형별로 그룹화 할 수 있는 기능.

`GROUP BY`: 특정 컬럼을 그룹화 한다.
`HAVING`: 특정 컬럼을 그룹화한 결과에 조건을 건다.
(WHERE = 그룹화 하기전 조건문, HAVING = 그룹화 후 조건문)

|id|type|name|
|-|-|-|
|1|1|안중근|
|2|1|윤봉길|
|3|2|김유신|
|4|2|이순신|
|5|3|이성계|
|6|3|왕건|
|7|4|반갑수|

```sql
SELECT type, COUNT(name) AS cnt FROM hero_collection GROUP BY type;
```

|type|cnt|
|-|-|
|1||
|2||
|3||
|4||
### GROUP BY에 두 개 이상의 컬럼을 쓰는 경우

성별 컬럼과 지역 컬럼이 있을 때 `GROUP BY 성별, 지역`을 하게되면

경기도 여성 / 경기도 남성 / 서울 여성 / 서울 남성

이런 식으로 성별과 지역으로 만들 수 있는 **모든 경우의 수**로 그룹을 만든다.
### GROUP BY 오류

SELECT문의 모든 컬럼은 **집계 함수**가 되거나 **GROUP BY 절에 나타나야** 한다.
그렇지 않으면, GROUP BY 표현식이 아니라는 오류 메시지가 발생한다.

오류 발생 이유는 GROUP BY가 하고 싶은 데이터 그룹화를 하지 못하기 때문이다.

`customer Table`

|id|city|state|last_purchase_date|
|-|-|-|-|
|1|강남|서울|2020-02-02|
|2|강북|서울|2021-02-10|
|3|수지|용인|2021-02-11|
|4|기흥|용인|2022-01-01|

```sql
SELECT state, city, MAX(last_purchase_date) AS last_purchase
FROM customers
GROUP BY state;
```

총 3가지 정보를 조회하는데 `city`만 GROUP BY에도 들어가있지 않고, 집계 함수를 사용하지도 않는다.

`서울`에는 2개의 서로 다른 도시 (`강남`, `강북`)이 있다. 때문에 `state`를 기준으로 GROUP BY를 하게 되면, 최종 조회되는 city에 어느 도시가 들어가야할지 알 수 없다. 

이런 경우에는 city에 집계 함수를 (적용하나 마나 결과는 동일한 경우)적용해서 해결한다.

https://kimsyoung.tistory.com/entry/GROUP-BY%E4%B8%8B-%EC%98%A4%EB%A5%98%EB%AC%B8-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0