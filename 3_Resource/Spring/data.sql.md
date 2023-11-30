#papercut 

데이터베이스에 삽입할 초기 데이터를 정의하는 파일.

`resource` 폴더 하위에 위치하며 어플리케이션 실행 시점에 파일에 작성한 INSERT 쿼리들이 수행된다. 
[[schema.sql]] 파일이 먼저 수행된 다음 실행된다.

스프링에서는 `DataSourceInitializer` Bean을 사용하여 schema.sql 파일과 data.sql 파일을 실행한다.

```sql
INSERT INTO PRODUCT (name, img_url, price) values ('피자', 'https://cdn.dominos.co.kr/admin/upload/goods/20200311_x8StB1t3.jpg', 13000);

INSERT INTO PRODUCT (name, img_url, price) values ('샐러드', 'https://m.subway.co.kr/upload/menu/K-%EB%B0%94%EB%B9%84%ED%81%90-%EC%83%90%EB%9F%AC%EB%93%9C-%EB%8B%A8%ED%92%88_20220413025007802.png', 20000); 

INSERT INTO PRODUCT (name, img_url, price) values ('치킨', 'https://cdn.thescoop.co.kr/news/photo/202010/41306_58347_1055.jpg', 10000);
```