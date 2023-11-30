#papercut 

데이터베이스 스키마를 정의하는 파일.

어플리케이션 실행 시점에 schema.sql 파일이 실행되며 파일에 정의된 스키마를 토대로 테이블을 생성한다.

스프링에서는 `DataSourceInitializer`라는 빈을 통해서 schema.sql과 [[data.sql]]을 실행한다.

```sql
DROP TABLE IF EXISTS product; 

CREATE TABLE PRODUCT 
( 
	id BIGINT NOT NULL AUTO_INCREMENT, 
	name VARCHAR(20) NOT NULL, 
	img_url VARCHAR(255), 
	price INT NOT NULL, 
	PRIMARY KEY (id) 
);
```