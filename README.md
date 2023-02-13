## SQL 코딩테스트 대비



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

- [출처](https://paris-in-the-rain.tistory.com/100)



### Between

- A **이상** B **이하**

  ```sql
  select *
  from products
  where price not between 10 and 20
  ```

  

### IFNULL

- IFNULL(feature명, 'NONE') -> feature컬럼의 값이 null이면 'NONE으로 채워넣기'

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



### Case : 아직 안써봄

- `when`, `else`, `end`의 키워드와 함께 사용



### Limit

- `LIMIT N` 위에서부터 N개

- `LIMIT N, M` : N부터 M까지(둘다 포함)



### MIN(), MAX(), COUNT(), AVG(), SUM()

- count는 null을 포함시키지 않고 집계됨
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
- `Select Distinct 필드이름`



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

  

### 집합연산

- Union :  합집합 (중복 비허용)

- Union All : 합집합 (중복 허용)

- Minus : 차집합 

- INTERSECT : 교집합

  ```sql
  SELECT 필드이름
  FROM 테이블1
  UNION (또는 UNION ALL, MINUS, INTERSECT)
  SELECT 필드이름
  FROM 테이블2
  ```



### 소수점

- CEIL(소수) : 올림
- FLOOR(소수) : 버림
- Round(소수, 반올림할 자리 값) : 반올림
  - 반올림할 자리값은 생략가능
  - 생략한경우 default로 0이 들어감
  - [0] 소수점 첫째자리, [1] 소수점 둘째자리 ...



### DATE_FORMAT

- Year, Month, Day 정보만 출력하고 싶다면

- `1992-03-16 00:00:00` 👉 `1999-07-09`

  ```sql
  DATE_FORMAT(DATE_OF_BIRTH, "%Y-%m-%d")AS DATE_OF_BIRTH
  ```




### GROUP BY

- 두개의 조건으로 `GROUP BY`를 사용할 수도 있음

  ```sql
  SELECT user_id, product_id
  FROM online_sale
  GROUP BY user_id, product_id
  HAVING COUNT(user_id) >= 2
  ```

  



----

## 에러관련

1. Every derived table must have its own alias

   - 서브쿼리를 사용하는 경우, 서브쿼리 끝에 가상의 테이블의 이름을 넣어줌

     ```sql
     SELECT *
     From ( 서브쿼리 ) A
     ```



---

## 출처

1. [동네코더의 IT, 인공지능 이야기](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=baek2sm&logNo=221578169366)
2. [live the life you love](
