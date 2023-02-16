## SQL 코딩테스트 대비

- [프로그래머스 SQL 고득점Kit](https://school.programmers.co.kr/learn/challenges?tab=sql_practice_kit) 1회차 문제풀이 완료 (2022.02.17)

</br>

## 개념 정리


### IF : Select, Where 절에서 사용가능

```sql
SELECT IF(10>5, '크다', '작다') AS result;
```



### Like

- _ 한자리를 의미
- % 0자 이상의 글자



### IN

- **where**절 내 여러 값을 설정하고자 할 때 사용

- 연산 속도가 상대적으로 빠름

- **or** 연산과 유사한 효과

  ```sql
  select *
  from Customers
  where country in ('UK', 'Korea') // Customers 중 country가 UK이거나 KOREA인 것 다 뽑기
  ```

- 문제 : 동물 보호소에 들어온 동물 중 이름이 'Lucy, Ella, Pickle, Rogan, Sabrina, Mitty'인 동물의 아이디와 이름, 성별 및 중성화 여부를 조회하는 SQL 문을 작성해주세요.

  ```sql
  select ANIMAL_ID, NAME, SEX_UPON_INTAKE
  from ANIMAL_INS ani
  where NAME in ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty');
  ```



### Between

- A **이상** B **이하**

  ```sql
  select *
  from products
  where price not between 10 and 20
  ```

  

### IFNULL

- **`IFNULL(feature명, 'NONE')`** 

  -  feature컬럼의 값이 null이면 'NONE으로 채워넣기'
  - 순서 주의!

  ```sql
  SELECT ANIMAL_TYPE, IFNULL(NAME,'No name') AS NAME, SEX_UPON_INTAKE
  FROM ANIMAL_INS
  ```



### IF

- `IF(컬럼명 IS NULL, '1', '2') FROM 테이블명`

- 컬럼이 NULL일 경우 1을, NULL이 아닐때는 2를 return한다.



### **NULLIF(A, B)**

- (A ==  B) return null 

- false면 return A

  ```sql
  SELECT NULLIF(1, 1) ; # null 을 리턴
  SELECT NULLIF(1, 2) ; # 1을 리턴
  ```



### Case

- `when`, `else`, `end`의 키워드와 함께 사용

  ```sql
  SELECT HISTORY_ID,
          CAR_ID,
          DATE_FORMAT(START_DATE, '%Y-%m-%d') as START_DATE ,
          DATE_FORMAT(END_DATE, '%Y-%m-%d') as END_DATE,
          case when (DATEDIFF( END_DATE, START_DATE) + 1) >= 30 then '장기 대여'
               else '단기 대여'
               end as RENT_TYPE
  FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
  WHERE YEAR(START_DATE) = 2022 AND MONTH(START_DATE) = 9
  ORDER BY HISTORY_ID DESC
  ```

  

### Limit

- `LIMIT N` 위에서부터 N개

- `LIMIT N, M` : N부터 M까지(둘다 포함)



### MIN(), MAX(), COUNT(), AVG(), SUM()

- coun(*) : 모든 행의 개수 count (null 포함)
- count(특정필드) : null 제외 count
- Select에서 사용
- 집계함수라고 함
- `Group by`와 함께 사용되는 경우가 많음
- `having`과 함께 사용



### Union

- 두 개 이상의 질의 결과를 하나의 테이블로 합치고자 할 때 사용

- 기본적으로 중복 row 제거

- Union all : 중복값 허용 == 동일한 row가 여러번 나올 수 있다

- 필드들이 동일해야하는데, 두 테이블의 구성값이 다를경우 아래와 같이 쓸수 있음

  - Ex) 온라인, 오프라인 판매기록 데이터 합치기

  - `NULL AS 필드명`

    ```sql
    SELECT A, B, C
    FROM ONLINE_SALE
    
    UNION
    
    SELECT A, NULL AS B, C
    FROM OFFLINE_SALE
    ```

    

### Distinct

- 중복된 데이터를 없애고 조회하는 경우

  ```sql
  Select Distinct 필드이름
  
  SELECT YEAR(SALES_DATE) AS YEAR, 
  				MONTH(SALES_DATE) AS MONTH, 
  				GENDER, 
  				COUNT(DISTINCT ONLINE_SALE.USER_ID) as USERS #여기!
  FROM ONLINE_SALE
  LEFT JOIN USER_INFO
  	ON USER_INFO.USER_ID = ONLINE_SALE.USER_ID
  WHERE GENDER IS NOT NULL
  GROUP BY YEAR(SALES_DATE), MONTH(SALES_DATE), GENDER
  ORDER BY YEAR, MONTH, GENDER
  ```

  

### Where

- 여러 조건을 적용하고 싶을 경우, `AND` 와 `OR` 키워드를 사용한다
- 개인적으로 `WHERE 필드명 IS NOT NULL` 자주 사용했음
  - Cf. `HAVING 필드명 IS NOT NULL` 도 가능



### 조건식의 종류

- Between, in, is, like

  ```sql
  WHERE 필드이름 BETWEEN 0 AND 100
  WHERE 필드이름 NOT BETWEEN 0 AND 100
  
  WHERE 필드이름 IN (0, 10, 100)
  WHERE 필드이름 NOT IN (0, 10, 100)
  
  WHERE 필드이름 IS NULL
  WHERE 필드이름 NOT IS NULL
  
  WHERE 필드이름 LIKE '홍__';
  WHERE 필드이름 NOT LIKE'홍__';
  ```



### ORDER BY

- 정렬 조건을 순서대로 나열

  ```sql
  SELECT 필드이름
  FROM 테이블
  ORDER BY 필드이름1, 필드이름2 DESC, 필드이름3 ASC
  ```

- `ORDER BY 1` == `ORDER BY 필드1`
- `ORDER BY 1 DESC` 도 가능



### 소수점

- CEIL(소수) : 올림

- FLOOR(소수) : 소수점 버림 -> 결과가 정수

- Round(소수, 반올림할 자리 값) : 반올림

  - 반올림할 자리값은 생략가능
  - 생략한경우 default로 0이 들어감
  - [0] 소수점 첫째자리, [1] 소수점 둘째자리 ...

- TRUNCATE : 버림 -> 소숫점 몇번째 부터 버릴건지 지정할 수 있음

  ```sql
  SELECT TRUNCATE(135.375, 2); # 135.37
  ```

  

### DATE_FORMAT

- Year, Month, Day 정보만 출력하고 싶다면

- `1992-03-16 00:00:00` 👉 `1999-07-09`

  ```sql
  DATE_FORMAT(DATE_OF_BIRTH, "%Y-%m-%d")AS DATE_OF_BIRTH
  ```



### DATEDIFF

- 기간을 구해야하는 문제라면,
  `END_DATE` - `START_DATE` 계산할때 **`+1`** 해야하는것 잊지말것!





### GROUP BY

- 두개의 조건으로 `GROUP BY`를 사용할 수도 있음

  ```sql
  SELECT user_id, product_id
  FROM online_sale
  GROUP BY user_id, product_id
  HAVING COUNT(user_id) >= 2
  ```

- STEP에 따라 GROUP을 나누고 싶으면, 피쳐의 값을 수정하는 코드를 GROUP BY 다음에 짜주면 됨 

  ```sql
  SELECT Floor(PRICE/10000) * 10000 AS PRICE_GROUP, Count(*) AS PRODUCTS
  from PRODUCT
  group by Floor(PRICE/10000) * 10000
  ORDER BY 1
  ```

  - Cf) 정수끼리 / 를 하면 몫만 나오는 것으로 알고있는데
    왜인지 모르겠지만 소수점 결과가 나왔다.
  - 이를 정수로 만들기 위해 `FLOOR`를 사용함
  - [StackOverflow 참고](https://stackoverflow.com/questions/35792821/sql-group-by-steps)

  

### RECURSIVE

- [문제) 입양 시각 구하기(2)](https://school.programmers.co.kr/learn/courses/30/lessons/59413)
- 07시부터 20시 까지의 데이터만 있는데, 나머지 구간에 대해(23시까지) 없는 데이터에는 0을 채워 넣어야함



### HOUR

- HOUR(DATETIME) 이렇게 하면 '시 정보'만 알수있음



### 두 날짜 비교 

- DATE(1999.7.09) **between** START_DATE **and** END_DATE
- '1999.07.09' **between** START_DATE **and** END_DATE



### WITH

- WITH 구문 다중 변수 사용

  - `,` 사용

  ```sql
  WITH CTE1 AS ( ),
  CTE2 AS ( )
  ```





### 집합연산

- `Union` :  합집합 (중복 비허용)

- `Union All` : 합집합 (중복 허용)

- **[ORACLE]** `Minus` : 차집합

  - **[MySQL]** `LEFT JOIN` & `IS NULL` 

  - **[MySQL]** `WHERE` & `NOT IN`

- **[ORACLE]** `INTERSECT` : 교집합

  - **[MySQL]** : `INNER JOIN`

  ```sql
  SELECT 필드이름
  FROM 테이블1
  UNION (또는 UNION ALL)
  SELECT 필드이름
  FROM 테이블2
  ```





### JOIN

- <img src="https://hongong.hanbit.co.kr/wp-content/uploads/2021/11/OUTER-JOIN_%EB%8D%94%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-1.png" alt="OUTER-JOIN_더알아보기-1" style="zoom: 50%;" />

- 👆출처 : 한빛미디어
- `LEFT INNER JOIN`
- `LEFT OUTER JOIN`
- **[ORACLE]** `FULL OUTER JOIN`
  - **[MySQL]** `LEFT OUTER JOIN` (`UNION`) `RIGHT OUTER JOIN`
- `INNER JOIN`을 사용할때 `LEFT`/`RIGHT`를 붙이는것 처럼,
  `OUTER JOIN`이 사용할때도 마찬가지로  `LEFT`/`RIGHT`가 함께 와야함.
  실수하지말것!
- `FROM` 안에 2개 이상의 TABLE을 사용하고  `JOIN`을 사용할 경우,
  FROM절 안에 쓰는 테이블의 순서 주의!

 

### STRING

- SUBSTRING

  ```sql
  SELECT LEFT(PRODUCT_CODE, 2) AS CATEGORY, COUNT(*) AS PRODUCTS
  FROM PRODUCT
  GROUP BY LEFT(PRODUCT_CODE, 2)
  ORDER BY CATEGORY
  ```

- https://yeahvely.tistory.com/89





----

## 에러관련

1. **Every derived table must have its own alias**

   - 서브쿼리를 사용하는 경우, 서브쿼리 끝에 가상의 테이블의 이름을 넣어줌

     ```sql
     SELECT *
     From ( 서브쿼리 ) A
     ```



2. **Invalid use of group function**

   - 명령어 순서 실수하지 않았는지 체크하기
   - `SELECT` 컬럼명 > `FROM` 테이블명 > `WHERE` > `GROUP BY` > `HAVING` > `ORDER BY`

    

3. 제대로 구현했는데도 지속적으로 오류가 발생할경우, **DISTINCT** 따져봤는지 생각해볼것
   - `COUNT(DISTINCT 필드명)`





---

## 출처

1. [동네코더의 IT, 인공지능 이야기](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=baek2sm&logNo=221578169366)
2. [live the life you love](https://paris-in-the-rain.tistory.com/100)
