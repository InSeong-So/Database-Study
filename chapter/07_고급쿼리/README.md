# 계층형 쿼리
## 계층형 구조
- 2차원 형태의 테이블에 저장된 데이터를 계층형 구조로 결과를 반환하는 쿼리

### 예제
![계층별 부서 정보](/chapter/07_고급쿼리/images/1.JPG)
```sql
-- 부서 테이블의 부서 정보를 상위-하위 부서로 나눈 것
-- 필요 정보 : 부서번호, 부서명, 상위 부서번호, 각 부서별 레벨, 계층적 구조로 조회하기 위한 정렬 순서
-- 이를 바탕으로 위와 같은 형태의 데이터를 추출하기 위한 쿼리
SELECT DEPARTMENT_ID,
       DEPARTMENT_NAME,
       0 AS PARENT_ID,
       1 AS LEVELS,
       PARENT_ID || DEPARTMENT_ID AS SORT
FROM DEPARTMENTS
WHERE PARENT_ID IS NULL
UNION ALL
SELECT T2.DEPARTMENT_ID,
       LPAD(' ' , 3 * (2-1)) || T2.DEPARTMENT_NAME AS DEPARTMENT_NAME,
       T2.PARENT_ID,
       2 AS LEVELS,
       T2.PARENT_ID || T2.DEPARTMENT_ID AS SORT
FROM DEPARTMENTS T1,
     DEPARTMENTS T2
WHERE T1.PARENT_ID IS NULL
  AND T2.PARENT_ID = T1.DEPARTMENT_ID
UNION ALL
SELECT T3.DEPARTMENT_ID,
       LPAD(' ' , 3 * (3-1)) || T3.DEPARTMENT_NAME AS DEPARTMENT_NAME,
       T3.PARENT_ID,
       3 AS LEVELS,
       T2.PARENT_ID || T3.PARENT_ID || T3.DEPARTMENT_ID AS SORT
FROM DEPARTMENTS T1,
     DEPARTMENTS T2,
     DEPARTMENTS T3
WHERE T1.PARENT_ID IS NULL
  AND T2.PARENT_ID = T1.DEPARTMENT_ID
  AND T3.PARENT_ID = T2.DEPARTMENT_ID
UNION ALL
SELECT T4.DEPARTMENT_ID,
       LPAD(' ' , 3 * (4-1)) || T4.DEPARTMENT_NAME AS DEPARTMENT_NAME,
       T4.PARENT_ID,
       4 AS LEVELS,
       T2.PARENT_ID || T3.PARENT_ID || T4.PARENT_ID || T4.DEPARTMENT_ID AS SORT
FROM DEPARTMENTS T1,
     DEPARTMENTS T2,
     DEPARTMENTS T3,
     DEPARTMENTS T4
WHERE T1.PARENT_ID IS NULL
  AND T2.PARENT_ID = T1.DEPARTMENT_ID
  AND T3.PARENT_ID = T2.DEPARTMENT_ID
  AND T4.PARENT_ID = T3.DEPARTMENT_ID
ORDER BY SORT;
```
### 결과
![부서 계층 정보](/chapter/07_고급쿼리/images/2.JPG)
- 총 4개의 SELECT 문이 UNION ALL로 연결
  - 첫 번째 : 가장 상위 부서인 총무기획부
  - 두 번째 : 총무기획부 부서번호를 PARENT_ID 값으로 가진 부서
  - 세 번째 : 두 번째 쿼리 결과의 각 부서를 PARENT_ID 값으로 가진 부서
  - 네 번째 : 세 번째 쿼리 결과의 각 부서를 PARENT_ID 값으로 가진 부서
- 문제점
    1. 현 부서 테이블의 계층 구조는 총 4레벨
       - 레벨이 더 많아질 때마다 SELECT 문을 만들어 레벨 수 만큼 UNION ALL로 연결해야 한다.
    2. 레벨 수 자체를 단순하게 직접 코딩했다.
    3. 쿼리가 너무 복잡하여 작성한 사람조차도 이해하기가 힘들다.


## 계층형 쿼리
```sql
-- 작성 방법
SELECT EXPR1,
       EXPR2,
       ...
FROM 테이블
WHERE 조건
START WITH [최상위 조건]
CONNECT BY [NOCYCLE][PRIOR 계층형 구조 조건];
```
- START WITH 조건 : 계층형 구조에서 최상위 계층의 로우를 식별하는 조건을 명시
- CONNECT BY 조건 : 계층형 구조가 어떤 식으로 연결되는지를 기술하는 부분

### 예제
- 앞의 UNION ALL을 사용한 쿼리문을 변경한 내용
    ```sql
    SELECT DEPARTMENT_ID,
           LPAD(' ', 3 * (LEVEL - 1)) || DEPARTMENT_NAME,
           LEVEL
    FROM DEPARTMENTS
    START WITH PARENT_ID IS NULL
    CONNECT BY PRIOR DEPARTMENT_ID = PARENT_ID;
    ```
    - 가장 상위 부서는 PARENT_ID 값이 NULL 이므로 START WITH 절에 명시
    - CONNECT BY 절에 계층 구조 조건을 명시
    - 세 번째 컬럼으로 LEVEL을 명시
      - 계층형 쿼리에서만 사용할 수 있는 의사 컬럼
      - 계층형 구조에 따른 레벨 값을 자동으로 반환
      - 계층 구조가 한 눈에 들어오도록 LPAD 함수로 공백 문자를 삽입
- 계층 쿼리에서도 다양한 조건 명시와 조인이 가능하다.
- 