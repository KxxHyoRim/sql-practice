#### 평균 일일 대여 요금 구하기
```sql
SELECT round(AVG(DAILY_FEE)) AS AVERAGE_FEE
From CAR_RENTAL_COMPANY_CAR
where CAR_TYPE = "SUV"
```
