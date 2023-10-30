**Person 테이블**

|name|phone|pid|
|-|-|-|
|고산하|010-1111-1111|1|
|홍단무|010-2222-2222|2|
|홍길동|010-3333-3333|3|

**Property 테이블**

|pid|spid|selling|
|-|-|-|
|1|1|후추|
|3|2|딸기|
|3|3|글러브|
|3|4|후드티|
|4|5|돈까스|
### JOIN (INNER JOIN)

테이블간의 교집합을 나타낸다.
```sql
SELECT
	name, phone, selling
FROM Person p 
JOIN Property py ON p.pid = py.pid; 
```


https://superman28.tistory.com/23