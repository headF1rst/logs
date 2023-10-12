- 희망휴무_신청_전체_인원 
	- 특정 주차(week_of_year)내 require_day_off의 데이터 수
- 희망휴무_배정_인원
	- 특정 기간내 `AllRequestDaysOffRule`, `Request1DayOffRule`, `MissingAllRequestDaysOffRule` rule의 수
- 희망휴무_1순위_배정_인원
	- 특정 기간내 `AllRequestDaysOffRule`, `Request1DayOffRule` rule의 수
- 희망휴무_2순위_배정_인원
	- 특정 기간내 `MissingAllRequestDaysOffRule` rule의 수

```sql
-- 202335주차 희망휴무 히트율  
SELECT  
    COUNT(*) AS 희망휴무_신청_전체_인원,  
    SUM(배정됨) AS 희망휴무_배정_인원,  
    SUM(배정_1순위) AS 희망휴무_1순위_배정_인원,  
    SUM(배정_2순위) AS 희망휴무_2순위_배정_인원  
FROM (  
    SELECT  
       r.delivery_manager_id,  
        MAX(CASE WHEN ds.day_of_week > 0 AND ds.rule IN ('AllRequestDaysOffRule', 'Request1DayOffRule', 'MissingAllRequestDaysOffRule') THEN 1 ELSE 0 END) AS 배정됨,  
        MAX(CASE WHEN  CAST(r.순위1 AS INTEGER) = ds.day_of_week AND ds.rule IN ('AllRequestDaysOffRule', 'Request1DayOffRule') THEN 1 ELSE 0 END) as 배정_1순위,  
        MAX(CASE WHEN  CAST(r.순위2 AS INTEGER) = ds.day_of_week AND ds.rule = 'MissingAllRequestDaysOffRule' THEN 1 ELSE 0 END) as 배정_2순위  
    FROM (  
        SELECT  
            delivery_manager_id,  
            NULLIF(SPLIT_PART(day_of_week, ',', 1), '') AS 순위1,  
            NULLIF(SPLIT_PART(day_of_week, ',', 2), '') AS 순위2,  
            CAST(NULLIF(unnest(string_to_array(day_of_week, ',')), '') AS INTEGER) AS day_of_week  
        FROM  
            r2_require_day_off  
        WHERE  
            week_of_year = '202335'  
    ) AS r  
    LEFT JOIN (  
        SELECT  
            delivery_manager_id,  
            rule,  
            CASE    
WHEN EXTRACT(DOW FROM work_day) = 0 THEN 7  
                ELSE EXTRACT(DOW FROM work_day)  
            END AS day_of_week  
        FROM  
            r2_delivery_manager_schedule  
        WHERE  
            work_type = 'OFF' AND "rule" NOT IN ('SYSTEM', 'ADMIN') AND work_day BETWEEN '2023-08-28' AND '2023-09-04'  
    ) AS ds ON r.delivery_manager_id = ds.delivery_manager_id AND ds.day_of_week = r.day_of_week  
    GROUP BY r.delivery_manager_id  
) AS subquery;
```

