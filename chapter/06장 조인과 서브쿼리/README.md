# 조인의 종류
- 조인 연산자에 따른 구분 : 동등 조인, 안티 조인
- 조인 대상에 따른 구분 : 셀프 조인
- 조인 조건에 따른 구분 : 내부 조인, 외부 조인, 세미 조인, 카타시안 조인
- 기타 : ANSI 조인

# 내부 조인과 외부조인
## 동등 조인(EQUI-JOIN)
- 가장 기본이자 일반적인 조인 방법
- WHERE 절에서 등호('=') 연산자를 사용해 2개 이상의 테이블이나 뷰를 연결하는 조인
- 등호 연산자를 사용하여 WHERE 절 조건에 만족하는 데이터를 추출하는 조인
    ```sql
    -- 동등 조인
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           A.DEPARTMENT_ID,
           B.DEPARTMENT_NAME
    FROM EMPLOYEES A,
         DEPARTMENTS B
    WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID;
    ```

## 세미 조인(SEMI-JOIN)
- 서브 쿼리로 서브 쿼리에 존재하는 데이터만 메인 쿼리에서 추출하는 조인
- 서브 쿼리에 존재하는 메인 쿼리 데이터가 여러 건 존재해도 최종 반환되는 메인 쿼리 데이터에는 중복되는 건이 없다.
  - 일반 조인은 중복되는 건이 발생한다.
    ```sql
    SELECT A.DEPARTMENT_ID,
           A.DEPARTMENT_NAME
    FROM DEPARTMENTS A,
         EMPLOYEES B
    WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID
      AND B.SALARY > 3000
    ORDER BY A.DEPARTMENT_NAME;
    ```

### EXISTS 사용
- 조건에 만족하는 데이터가 한 건이라도 존재하면 결과를 즉시 반환한다.
    ```sql
    -- 세미 조인, EXISTS
    SELECT DEPARTMENT_ID,
           DEPARTMENT_NAME
    FROM DEPARTMENTS A
    WHERE EXISTS(
            SELECT *
            FROM EMPLOYEES B
            WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID
              AND B.SALARY > 3000
            )
    ORDER BY A.DEPARTMENT_NAME;
    ```

### IN 사용
- OR 조건으로 변환할 수 있는데 "이것이거나 저것이거나"로 풀어 쓸 수 있다.
    ```sql
    -- 세미 조인, IN
    SELECT DEPARTMENT_ID,
           DEPARTMENT_NAME
    FROM DEPARTMENTS A
    WHERE A.DEPARTMENT_ID IN (
                            SELECT B.DEPARTMENT_ID
                            FROM EMPLOYEES B
                            WHERE B.SALARY > 3000
                            )
    ORDER BY DEPARTMENT_NAME;
    ```

## 안티 조인(ANTI-JOIN)
- 서브 쿼리의 B 테이블에 없는 메인 쿼리 A 테이블의 데이터만 추출하는 조인
- 한쪽 테이블에만 있는 데이터를 추출하는 것
  - 조회 조건에 NOT IN, NOT EXISTS 연산자를 사용
  - 세미 조인과 반대 개념

### NOT IN 사용
```sql
--- 안티 조인, NOT IN
SELECT A.EMPLOYEE_ID,
       A.EMP_NAME,
       A.DEPARTMENT_ID,
       B.DEPARTMENT_NAME
FROM EMPLOYEES A,
     DEPARTMENTS B
WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID
AND A.DEPARTMENT_ID NOT IN (
                        SELECT DEPARTMENT_ID
                        FROM DEPARTMENTS
                        WHERE MANAGER_ID IS NULL
                    );
```

### NOT EXISTS 사용
```sql
SELECT COUNT(*)
FROM EMPLOYEES A
WHERE NOT EXISTS(
            SELECT 1
            FROM DEPARTMENTS C
            WHERE A.DEPARTMENT_ID = C.DEPARTMENT_ID
              AND MANAGER_ID IS NULL
            );
```

## 셀프 조인(SELF-JOIN)
- 서로 다른 두 테이블이 아닌 동일한 한 테이블을 사용해 조인하는 방법
    ```sql
    -- 셀프 조인
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.EMPLOYEE_ID,
           B.EMP_NAME,
           A.DEPARTMENT_ID
    FROM EMPLOYEES A,
         EMPLOYEES B
    WHERE A.EMPLOYEE_ID < B.EMPLOYEE_ID
      AND A.DEPARTMENT_ID = B.DEPARTMENT_ID
      AND A.DEPARTMENT_ID = 20;
    ```

> 여기까지 내부 조인이다.

## 외부 조인(OUTER-JOIN)
- 일반 조인을 확장한 개념
- 조인 조건에 만족하는 데이터 뿐만 아니라 어느 한 쪽 테이블에 조인 조건에 명시된 컬럼에 값이 없거나(NULL 이더라도) 해당 Row가 아예 없더라도 데이터를 모두 추출한다.

### 차이점
- 일반 조인
    ```sql
    -- 일반 조인
    SELECT A.DEPARTMENT_ID,
           A.DEPARTMENT_NAME,
           B.JOB_ID,
           B.DEPARTMENT_ID
    FROM DEPARTMENTS A,
         JOB_HISTORY B
    WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID;
    ```

- 외부 조인
  - 조인 조건에서 데이터가 없는 테이블의 컬럼에 (+)를 붙이면 된다.
    ```sql
    -- 외부 조인
    SELECT A.DEPARTMENT_ID,
           A.DEPARTMENT_NAME,
           B.JOB_ID,
           B.DEPARTMENT_ID
    FROM DEPARTMENTS A,
         JOB_HISTORY B
    WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID(+);
    ```
  - 외부 조인은 조건에 해당하는 조인 조건 모두에 (+)를 붙여야 한다.  
    ```sql
    -- 붙이기 전
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.JOB_ID,
           B.DEPARTMENT_ID
    FROM EMPLOYEES A,
         JOB_HISTORY B
    WHERE A.EMPLOYEE_ID = B.EMPLOYEE_ID(+)
      AND A.DEPARTMENT_ID = B.DEPARTMENT_ID;
    ```
    ```sql
    -- 붙인 후
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.JOB_ID,
           B.DEPARTMENT_ID
    FROM EMPLOYEES A,
         JOB_HISTORY B
    WHERE A.EMPLOYEE_ID = B.EMPLOYEE_ID(+)
      AND A.DEPARTMENT_ID = B.DEPARTMENT_ID(+);
    ```

### 외부 조인 정리
- 조인 대상 테이블 중 데이터가 없는 테이블 조인 조건에 (+)를 붙인다.
- 외부 조인의 조인 조건이 여러 개일 때 모든 조건에 (+)를 붙인다.
- 한 번에 한 테이블에만 외부 조인을 할 수 있다.
  - 조인 대상 테이블이 A, B, C 3개일 경우
    - A를 기준으로 B 테이블을 외부 조인으로 연결
    - 동시에 C를 기준으로 B 테이블에 외부 조인을 걸 수 없음
- (+) 연산자가 붙은 조건과 OR을 같이 사용할 수 없다.
- (+) 연산자가 붙은 조건에는 IN 연산자를 같이 사용할 수 없다.
  - 단, IN절 포함되는 값이 1개일 떄는 가능

## 카타시안 조인(CATASIAN PRODUCT)
- WHERE 절에 조인 조건이 없는 조인
  - FROM 절에 테이블을 명시했으나 두 테이블 간 조인 조건이 없다.
- 조인 조건이 없으므로 쿼리의 결과는 두 테이블 건수의 곱이다.
  - A 테이블 건수가 n1, B 테이블일 건수가 n2라면 결과는 "n1 * n2" 이다.
    ```sql
    -- EMPLOYEES 107건
    -- DEPARTMENTS 27건
    -- 107 * 27 = 2,889건
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.DEPARTMENT_ID,
           B.DEPARTMENT_NAME
    FROM EMPLOYEES A,
         DEPARTMENTS B;
    ```

# ANSI 조인
- ANSI SQL 문법을 사용한 조인
  - 모든 조인은 ANSI SQL을 사용해 변환이 가능하다.
- 기존 문법과는 달리 WHERE 절이 아닌 FROM 절에 조인 조건이 위치한다.

## ANSI 내부 조인
- 기존 문법
    ```sql
    -- 작성 방법
    SELECT A.컬럼1,
           A.컬럼2,
           B.컬럼1,
           B.컬럼2 ...
    FROM 테이블 A,
         테이블 B
    WHERE A.컬럼1 = B.컬럼1 → 조인조건
    ... ;
    ```
    ```sql
    -- 작성 예시
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.DEPARTMENT_ID,
           B.DEPARTMENT_NAME
    FROM EMPLOYEES A,
         DEPARTMENTS B
    WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID
      AND A.HIRE_DATE >= TO_DATE('2003-01-01', 'YYYY-MM-DD');
    ```

- ANSI 문법
    ```sql
    -- 작성 방법
    SELECT A.컬럼1,
           A.컬럼2,
           B.컬럼1,
           B.컬럼2 ...
    FROM 테이블 A
        INNER JOIN 테이블 B
                ON ( A.컬럼1 = B.컬럼1 ) → 조인조건
    WHERE ... ;
    ```
    ```sql
    -- 작성 예시
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.DEPARTMENT_ID,
           B.DEPARTMENT_NAME
    FROM EMPLOYEES A
        INNER JOIN DEPARTMENTS B
                ON (A.DEPARTMENT_ID = B.DEPARTMENT_ID)
    WHERE A.HIRE_DATE >= TO_DATE('2003-01-01', 'YYYY-MM-DD');
    ```

- ANSI 내부 조인은 FROM 절에서 INNER JOIN 구문을 쓴다.
  - 조인 조건은 ON 절에, 그 외의 조건은 기존대로 WHERE 절에 명시한다.
- 조인 조건 컬럼이 두 테이블 모두 동일하다면 ON 대신 USING 사용이 가능하다.
  - 이때는 SELECT 절에서 조인 조건에 포함된 컬럼명을 **테이블명.컬럼명** 형태가 아닌 **컬럼명**만 기술해야한다.
    ```sql
    -- 잘 작성 되지 못한 경우
    SELECT A.EMPLOYEE_ID,
        A.EMP_NAME,
        B.DEPARTMENT_ID,
        B.DEPARTMENT_NAME
    FROM EMPLOYEES A
        INNER JOIN DEPARTMENTS B
             USING (DEPARTMENT_ID)
    WHERE A.HIRE_DATE >= TO_DATE('2003-01-01', 'YYYY-MM-DD');
    ```
    ```sql
    -- 잘 작성 된 경우
    SELECT A.EMPLOYEE_ID,
        A.EMP_NAME,
        DEPARTMENT_ID,
        B.DEPARTMENT_NAME
    FROM EMPLOYEES A
        INNER JOIN DEPARTMENTS B
             USING (DEPARTMENT_ID)
    WHERE A.HIRE_DATE >= TO_DATE('2003-01-01', 'YYYY-MM-DD');
    ```
    - 두 테이블 간 조인 조건에 사용되는 컬럼 명이 동일한 경우가 많다.
      - 다를 때도 있으므로 USING 대신 ON 절을 사용하도록 하자.

## ANSI 외부 조인
- ANSI 외부 조인도 내부 조인과 비슷하다.
- 기존 문법
    ```sql
    -- 작성 방법
    SELECT A.컬럼1,
           A.컬럼2,
           B.컬럼1,
           B.컬럼2 ...
    FROM 테이블 A,
         테이블 B
    WHERE A.컬럼1 = B.컬럼1(+)
    ... ;
    ```
    ```sql
    -- 작성 예시
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.JOB_ID,
           B.DEPARTMENT_ID
    FROM EMPLOYEES A,
         JOB_HISTORY B
    WHERE A.EMPLOYEE_ID = B.EMPLOYEE_ID
      AND A.DEPARTMENT_ID = B.DEPARTMENT_ID(+);
    ```
- ANSI 문법
    ```sql
    -- 작성 방법
    SELECT A.컬럼1,
           A.컬럼2,
           B.컬럼1,
           B.컬럼2 ...
    FROM 테이블 A
         LEFT(RIGHT) [OUTER] JOIN 테이블 B
                               ON ( A.컬럼1 = B.컬럼1 )
    WHERE ... ;
    ```
    ```sql
    -- 작성 예시(LEFT OUTER)
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.JOB_ID,
           B.DEPARTMENT_ID
    FROM EMPLOYEES A
        LEFT OUTER JOIN JOB_HISTORY B
                     ON (
                         A.EMPLOYEE_ID = B.EMPLOYEE_ID
                         AND A.DEPARTMENT_ID = B.DEPARTMENT_ID
                        );
    ```
    ```sql
    -- 작성 예시(RIGHT OUTER)
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.JOB_ID,
           B.DEPARTMENT_ID
    FROM JOB_HISTORY B
        RIGHT OUTER JOIN EMPLOYEES A
                      ON (
                          A.EMPLOYEE_ID = B.EMPLOYEE_ID
                          AND A.DEPARTMENT_ID = B.DEPARTMENT_ID
                         );
    ```
    - ANSI 외부 조인은 FROM 절에 명시된 테이블 순서에 입각한다.
      - 먼저 명시된 테이블 기준으로 LEFT 또는 RIGHT를 붙인다.
    - 외부 조인은 OUTER라는 키워드를 붙이는데, 생략이 가능하다.
        ```sql
        -- 작성 예시(LEFT OUTER)
        SELECT A.EMPLOYEE_ID,
            A.EMP_NAME,
            B.JOB_ID,
            B.DEPARTMENT_ID
        FROM EMPLOYEES A
            LEFT JOIN JOB_HISTORY B
                   ON (
                       A.EMPLOYEE_ID = B.EMPLOYEE_ID
                       AND A.DEPARTMENT_ID = B.DEPARTMENT_ID
                      );
        ```

## CROSS 조인
- 카타시안 조인을 ANSI 조인에서는 CROSS 조인이라고 한다.
- 기존 문법
    ```sql
    -- 카타시안 조인
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.DEPARTMENT_ID,
           B.DEPARTMENT_NAME
    FROM EMPLOYEES A,
         DEPARTMENTS B;
    ```
- ANSI 문법
    ```sql
    -- 크로스 조인
    SELECT A.EMPLOYEE_ID,
           A.EMP_NAME,
           B.DEPARTMENT_ID,
           B.DEPARTMENT_NAME
    FROM EMPLOYEES A
        CROSS JOIN DEPARTMENTS B;
    ```

## FULL OUTER 조인
- 외부 조인의 하나로 ANSI 조인에서만 제공한다.
  - 기존 문법으로 사용할 수 없다.
- 외부 조인으로 해결 할 수 없는 조회
    ```sql
    -- 테스트 테이블 작성
    CREATE TABLE TEST_A ( EMP_ID INT);

    INSERT INTO TEST_A VALUES ( 10);
    INSERT INTO TEST_A VALUES ( 20);
    INSERT INTO TEST_A VALUES ( 30);

    CREATE TABLE TEST_B ( EMP_ID INT);

    INSERT INTO TEST_B VALUES ( 10);
    INSERT INTO TEST_B VALUES ( 20);
    INSERT INTO TEST_B VALUES ( 40);
    ```
  - 위 테이블을 아래와 같이 조회하고 싶을 때
    |EMP_ID_A|EMP_ID_B|
    |:----:|:----:|
    |10|10|
    |20|20|
    |30||
    ||40|
  - 일반적인 외부 조인으로는 한 문장으로 처리가 불가능하다.
    - 집합 연산자를 사용하면 가능하다.
    ```sql
    -- 일반적인 외부 조인, 오류 발생
    SELECT A.EMP_ID,
           B.EMP_ID
    FROM TEST_A A,
         TEST_B B
    WHERE A.EMP_ID(+) = B.EMP_ID(+);
    ```
    ```
    SQL 오류 : ORA-01468: outer-join된 테이블은 1개만 지정할 수 있습니다.
    ```
  - 외부 조인 조건에서는 한 쪽에만 (+)를 붙일 수 있다.
    - 해결하기 위해선 FULL OUTER 조인을 사용해야 한다.
    - 이를 사용하면 두 테이블 모두를 외부 조인 대상에 넣을 수 있다.
    ```sql
    -- 오류 발생 없이 원하는 결과 출력
    SELECT A.EMP_ID,
           B.EMP_ID
    FROM TEST_A A
        FULL OUTER JOIN TEST_B B
                     ON ( A.EMP_ID = B.EMP_ID );
    ```

> 모든 조인은 기존 오라클 문법과 ANSI 문법을 사용해 모두 구현할 수 있다.
> > FULL OUTER JOIN은 ANSI 문법을 사용해야 한다.

# 서브 쿼리