# 표준 조인(STANDARD JOIN)
## 표준 조인 개요
- 일반 집합 연산자

    ![](images/SQL_075.jpg)

    - UNION : 합집합. 공통 교집합의 중복을 없애기 위해 사전 작업으로 정렬 작업 발생
    - UNION ALL : 합집합. 공통 교집합을 중복해서 출력하므로 정렬 작업 미발생
    - INTERSECTION : 교집합. 두 집합의 공통집합을 추출
    - DIFFERENCE : (오라클은 MINUS) 차집합. 첫 번째 집합에서 두 번째 집합과의 공통집합을 제외
    - PRODUCT : CROSS(ANIS/ISO 표준) PRODUCT라고 불리는 곱집합. 양쪽 집합의 M*N 건의 데이터 조합이 발생

- 순수 관계 연산자

    ![](images/SQL_076.jpg)

    - SELECT 연산은 WHERE 절 기능으로 구현되었다.
    - PROJECT 연산은 SELECT 절의 컬럼 선택 기능으로 구현되었다.
    - JOIN 연산은 여러 JOIN 기능으로 가장 다양하게 구현되었다.
    - DIVIDE 연산은 현재 사용되지 않는다.
    - 관계형 데이터베이스의 경우 엔터티 확정 및 정규화 과정, 그리고 M:M (다대다) 관계를 분해하는 절차를 거치게 된다.
    - 정규화는 데이터 정합성과 데이터 저장 공간의 절약을 위해 엔터티를 최대한 분리하는 작업이며 일반적으로 3차 정규형이나 보이스코드 정규화까지 진행한다.
    - 정규화를 거치면 하나의 주제에 관련 있는 엔터티가 여러 개로 나누어지게 된다.
      - 해당 엔터티들이 주로 테이블이 되는데 흩어진 데이터를 연결해서 원하는 데이터를 가져오는 작업이 JOIN 이다.

<br>

## FROM 절 JOIN
### INNER JOIN
- OUTER JOIN과 반대로 내부 JOIN이라고 하며 JOIN 조건에서 동일한 값이 있는 행만 반환
    ```sql
    -- 1과 2는 같은 결과를 반환
    -- 1
    SELECT A.DEPTNO, A.EMPNO, A.ENAME, B.DNAME
    FROM   EMP A, DEPT B
    WHERE  A.DEPTNO = B.DEPTNO ;

    -- 2
    SELECT A.DEPTNO, A.EMPNO, A.ENAME, B.DNAME
    FROM   EMP A INNER JOIN DEPT B
    WHERE  A.DEPTNO = B.DEPTNO ;

    -- INNER 문구는 생략 가능
    SELECT A.DEPTNO, A.EMPNO, A.ENAME, B.DNAME
    FROM   EMP A  JOIN DEPT B
    WHERE  A.DEPTNO = B.DEPTNO ;
    ```

<br>

### NATURAL JOIN
- 두 테이블 간 동일한 이름을 갖는 모든 칼럼들에 대해 EQUI(=) JOIN을 수행
    ```sql
    SELECT DEPTNO, EMPNO, ENAME, DNAME
    FROM   EMP NATURAL JOIN DEPT ;
    ```

- SQL Server에서는 지원하지 않는다.

<br>

### USNIG 조건절
- 같은 이름을 가진 칼럼들 중에서 원하는 칼럼에 대해서만 선택적으로 EQUI JOIN 가능
    ```sql
    -- USING절에 명시된 컬럼이 기준이 되어 동일한 컬럼들을 조인
    -- 1
    SELECT *
    FROM DEPT JOIN DEPT_TEMP
    USING (DEPTNO);

    -- 2
    SELECT *
    FROM DEPT JOIN DEPT_TEMP
    USING (LOC, DEPTNO);
    ```

- SQL Server에서는 지원하지 않는다.

<br>

### ON 조건절
- JOIN 서술부(ON 조건절)와 비 JOIN 서술부(WHERE 조건절)를 분리, 칼럼명이 다르더라도 JOIN 조건을 사용할 수 있다.
    ```sql
    SELECT E.EMPNO, E.NAME, E.DEPTNO, D.DNAME
    FROM   EMP E JOIN DEPT D
    ON     (E.DEPTNO = D.DEPTNO);
    ```

- WHERE 절과 혼용하여 사용
    ```sql
    SELECT E.EMPNO, E.NAME, E.DEPTNO, D.DNAME
    FROM   EMP E JOIN DEPT D
    ON     (E.DEPTNO = D.DEPTNO)
    WHERE  E.DEPTNO = 30;
    ```

- ON 조건절 + 데이터 검증 조건 추가
    ```sql
    -- 1과 2는 같은 결과를 반환
    -- 1
    SELECT E.EMPNO, E.NAME, E.DEPTNO, D.DNAME
    FROM   EMP E JOIN DEPT D
    ON     (E.DEPTNO = D.DEPTNO AND E.DEPTNO = 30);

    -- 2
    SELECT E.EMPNO, E.NAME, E.DEPTNO, D.DNAME
    FROM   EMP E JOIN DEPT D
    ON     (E.DEPTNO = D.DEPTNO )
    WHERE   E.DEPTNO = 30;
    ```

- 다중 테이블 JOIN
    ```sql
    -- 1과 2는 같은 결과를 반환
    -- 1
    SELECT E.EMPNO, D.DEPTNO, D.DNAME, T.DNAME NEW_NAME
    FROM   EMP E JOIN DEPT D
    ON     (E.DEPTNO = D.DEPTNO)
                 JOIN DEPT_TEMP T
    ON     (E.DEPTNO = T.DEPTNO);

    -- 2
    SELECT E.EMPNO, D.DEPTNO, D.DNAME, T.DNAME NEW_NAME
    FROM   EMP E ,DEPT D, DEPT_TEMP T
    WHERE  E.DEPTNO = D.DEPTNO 
    AND    E.DEPTNO = T.DEPTNO;
    ```

<br>

### CROSS JOIN
- 일반 집합 연산자의 PRODUCT의 개념
- 테이블 간 JOIN 조건이 없는 경우 생길 수 있는 모든 데이터의 조합 출력
  - 두 개의 테이블에 대한 CARTESIAN PRODUCT 또는 CROSS PRODUCT와 같은 표현
  - 결과는 양쪽 집합의 M*N 건의 데이터 조합이 발생한다.
        ```sql
        -- EMP 14건 * DEPT 4건의 데이터로 56건 출력
        SELECT ENAME,  DNAME
        FROM   EMP CROSS JOIN DEPT
        ORDER  BY ENAME;
        ```

<br>

### OUTER JOIN
- INNER JOIN과 반대로 외부 JOIN이라고 하며 JOIN 조건에서 동일한 값이 없는 행도 반환

<div align=center>

![](images/SQL_077.jpg)

</div>

- LEFT OUTER JOIN
    ```sql
    -- DEPT에 속하지 않은 EMP의 정보 반환
    SELECT *
    FROM   EMP E LEFT OUTER JOIN DEPT D
    ON     E.DEPTNO = D.DEPTNO;
    ```

- RIGHT OUTER JOIN
    ```sql
    -- EMP에 속하지 않은 DEPT 정보 반환
    SELECT *
    FROM   EMP E RIGHT OUTER JOIN DEPT D
    ON     E.DEPTNO = D.DEPTNO;
    ```

- FULL OUTER JOIN
    ```sql
    -- 1과 2는 같은 결과를 반환(OUTER 생략 가능)
    -- 1
    SELECT *
    FROM   EMP E FULL OUTER JOIN DEPT D
    ON     E.DEPTNO = D.DEPTNO;

    -- 2
    SELECT *
    FROM   EMP E LEFT OUTER JOIN DEPT D
    ON     E.DEPTNO = D.DEPTNO
    UNION
    SELECT *
    FROM   EMP E RIGHT OUTER JOIN DEPT D
    ON     E.DEPTNO = D.DEPTNO;
    ```

<br>

### INNER vs OUTER vs CROSS JOIN 비교

<div align=center>

![](images/SQL_078.jpg)

</div>

- INNER JOIN : 2건 출력

- LEFT OUTER JOIN : 4건 출력

- RIGHT OUTER JOIN : 3건 출력

- FULL OUTER JOIN : 5건 출력

- CROSS JOIN : 12건 출력

<br>