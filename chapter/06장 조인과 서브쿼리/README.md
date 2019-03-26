# 조인의 종류
- 조인 연산자에 따른 구분 : 동등 조인, 안티 조인
- 조인 대상에 따른 구분 : 셀프 조인
- 조인 조건에 따른 구분 : 내부 조인, 외부 조인, 세미 조인, 카타시안 조인
- 기타 : ANSI 조인

## 동등 조인(EQUI-JOIN)
- 가장 기본이자 일반적인 조인 방법
- WHERE 절에서 등호('=') 연산자를 사용해 2개 이상의 테이블이나 뷰를 연결하는 조인
- 등호 연산자를 사용하여 WHERE절 조건에 만족하는 데이터를 추출하는 조인
    ```sql
    -- 동등 조인
    SELECT A.EMPLOYEE_ID, A.EMP_NAME, A.DEPARTMENT_ID, B.DEPARTMENT_NAME
    FROM EMPLOYEES A,
        DEPARTMENTS B
    WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID;
    ```

## 세미 조인(SEMI-JOIN)
- 서브 쿼리로 서브 쿼리에 존재하는 데이터만 메인 쿼리에서 추출하는 조인
  - IN과 EXISTS 연산자 사용
- 