# 분할해서 정복하기
## 출력 항목
```
-------------------------------------------
     연도 | 최대 매출사원명 | 최대매출액     
-------------------------------------------
```
## 필요 테이블
- 이탈리아 찾기 : COUNTRIES
- 이탈리아 고객 찾기 : CUSTOMERS
- 매출 : SALES
- 사원정보 : EMPLOYEES

## 단위 분할

### **①** 연도, 사원별 이탈리아 매출액 구하기
- 이탈리아 고객 찾기 : CUSTOMERS, COUNTRIES를 COUNTRY_ID로 조인
  - COUNTRY_NAME이 'Italy'인 것 찾기
- 이탈리아 매출 찾기 : 위 결과와 SALES 테이블을 CUST_ID로 조인
- 최대 매출액을 구하려면 MAX 함수를 사용하고 연도별로 GROUP BY 필요
```sql
SELECT SUBSTR(A.SALES_MONTH, 1, 4) YEARS,
       A.EMPLOYEE_ID,
       SUM(A.AMOUNT_SOLD)          AMOUNT_SOLD
FROM SALES A,
     CUSTOMERS B,
     COUNTRIES C
WHERE A.CUST_ID = B.CUST_ID
  AND B.COUNTRY_ID = C.COUNTRY_ID
  AND C.COUNTRY_NAME = 'Italy'
GROUP BY SUBSTR(A.SALES_MONTH, 1, 4), A.EMPLOYEE_ID
;
```

### **②** ①에서 구한 결과에 연도별 최대, 최소 매출액 구하기
```sql
SELECT YEARS,
       MAX(AMOUNT_SOLD) MAX_SOLD
FROM (SELECT SUBSTR(A.SALES_MONTH, 1, 4) YEARS,
             A.EMPLOYEE_ID,
             SUM(A.AMOUNT_SOLD)          AMOUNT_SOLD
      FROM SALES A,
           CUSTOMERS B,
           COUNTRIES C
      WHERE A.CUST_ID = B.CUST_ID
        AND B.COUNTRY_ID = C.COUNTRY_ID
        AND C.COUNTRY_NAME = 'Italy'
      GROUP BY SUBSTR(A.SALES_MONTH, 1, 4), A.EMPLOYEE_ID) K
GROUP BY YEARS
ORDER BY YEARS;
```

### **③** ①과 ②의 결과를 인라인 뷰로 만들기
- ①과 ②를 조인해서 최대 매출, 최소 매출액을 일으킨 사원을 찾아야 하기 떄문
```sql
SELECT EMP.YEARS,
       EMP.EMPLOYEE_ID,
       EMP.AMOUNT_SOLD
FROM (SELECT SUBSTR(A.SALES_MONTH, 1, 4) YEARS,
             A.EMPLOYEE_ID,
             SUM(A.AMOUNT_SOLD)          AMOUNT_SOLD
      FROM SALES A,
           CUSTOMERS B,
           COUNTRIES C
      WHERE A.CUST_ID = B.CUST_ID
        AND B.COUNTRY_ID = C.COUNTRY_ID
        AND C.COUNTRY_NAME = 'Italy'
      GROUP BY SUBSTR(A.SALES_MONTH, 1, 4), A.EMPLOYEE_ID) EMP,
     (SELECT YEARS,
             MAX(AMOUNT_SOLD) MAX_SOLD
      FROM (SELECT SUBSTR(A.SALES_MONTH, 1, 4) YEARS,
                   A.EMPLOYEE_ID,
                   SUM(A.AMOUNT_SOLD)          AMOUNT_SOLD
            FROM SALES A,
                 CUSTOMERS B,
                 COUNTRIES C
            WHERE A.CUST_ID = B.CUST_ID
              AND B.COUNTRY_ID = C.COUNTRY_ID
              AND C.COUNTRY_NAME = 'Italy'
            GROUP BY SUBSTR(A.SALES_MONTH, 1, 4), A.EMPLOYEE_ID) K
      GROUP BY YEARS) SALE
WHERE EMP.YEARS = SALE.YEARS
  AND EMP.AMOUNT_SOLD = SALE.MAX_SOLD
ORDER BY EMP.YEARS;
```

### **④** ③의 결과와 사원 테이블을 조인해서 사원 이름을 가져오기
```sql
SELECT EMP.YEARS,
       EMP.EMPLOYEE_ID,
       EMP2.EMP_NAME,
       EMP.AMOUNT_SOLD
FROM (SELECT SUBSTR(A.SALES_MONTH, 1, 4) YEARS,
             A.EMPLOYEE_ID,
             SUM(A.AMOUNT_SOLD)          AMOUNT_SOLD
      FROM SALES A,
           CUSTOMERS B,
           COUNTRIES C
      WHERE A.CUST_ID = B.CUST_ID
        AND B.COUNTRY_ID = C.COUNTRY_ID
        AND C.COUNTRY_NAME = 'Italy'
      GROUP BY SUBSTR(A.SALES_MONTH, 1, 4), A.EMPLOYEE_ID) EMP,
     (SELECT YEARS,
             MAX(AMOUNT_SOLD) MAX_SOLD
      FROM (SELECT SUBSTR(A.SALES_MONTH, 1, 4) YEARS,
                   A.EMPLOYEE_ID,
                   SUM(A.AMOUNT_SOLD)          AMOUNT_SOLD
            FROM SALES A,
                 CUSTOMERS B,
                 COUNTRIES C
            WHERE A.CUST_ID = B.CUST_ID
              AND B.COUNTRY_ID = C.COUNTRY_ID
              AND C.COUNTRY_NAME = 'Italy'
            GROUP BY SUBSTR(A.SALES_MONTH, 1, 4), A.EMPLOYEE_ID) K
      GROUP BY YEARS) SALE,
     EMPLOYEES EMP2
WHERE EMP.YEARS = SALE.YEARS
  AND EMP.AMOUNT_SOLD = SALE.MAX_SOLD
  AND EMP.EMPLOYEE_ID = emp2.EMPLOYEE_ID
ORDER BY EMP.YEARS;
```