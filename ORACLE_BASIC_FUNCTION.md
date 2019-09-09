# Function
- 내장 함수, 사용자 정의 함수
- 매개변수에 어떤 값을 전달하면, 내부적인 처리를 진행하고, 결과를 반환하도록 만들어진 코드.
- 단일행 함수(행당 결과 한 개)와 복수행 함수(여러개의 행을 투입하면 결과 한 개)로 구분된다.
- 단일행 함수는 문자함수, 숫자함수, 날짜함수, 변환함수, 일반함수로 구분.

<hr>
<br>

## SUBSTR()함수
- `SUBSTR(시작위치, 글자수)`
- 문자열의 일부만 추출해서 반환하는 함수.
- 주의) 시작 위치는 1부터 시작하며 시작 위치에 -값을 주면 오른쪽 끝에서부터 출발한다.
- 유사한 함수로 SUBSTRB() 함수가 있다. 바이트 수 기준으로 추출한다.

```sql
-- 앞에서부터 문자열을 자르는 방법
WITH MAP_O
     AS (SELECT '201909091324' EMP_ID
               ,'홍길동' EMP_NM
               ,'대조선국' ORG_ID
           FROM DUAL)
SELECT EMP_ID
      ,EMP_NM
      ,SUBSTR(EMP_NM, 2)
      ,SUBSTR(EMP_NM, 2, 3)
      ,ORG_ID
      ,SUBSTR(ORG_ID, 2, 1)
  FROM MAP_O;

-- 뒤에서부터 문자열을 자르는 방법
WITH MAP_O
     AS (SELECT '201909091324' EMP_ID
               ,'홍길동' EMP_NM
               ,'대조선국' ORG_ID
           FROM DUAL)
SELECT EMP_ID 
      ,EMP_NM
      ,SUBSTR(EMP_NM, -3)
      ,SUBSTR(EMP_NM, -3, 2)
      ,ORG_ID
      ,SUBSTR(ORG_ID, -3, 1)
  FROM MAP_O;
```

<hr>
<br>

## INSTR() 함수
- `INSTR(대상문자열, 찾는문자열, 시작위치, 특정번째)`. 특정번째는 생략 가능.
- 대상 문자열에서 특정 문자열이 존재하는지 여부를 확인하고, 위치를 반환하는 함수.
- 검색 결과가 존재하지 않는 경우 위치값 0 반환한다.
- 시작위치를 -1로 주면 뒤에서부터 순서를 찾는다.

```sql
-- 앞에서부터 첫번째 위치
SELECT INSTR('All fortune is to be conquered by bearing it.', 'e', 1, 1) 결과 FROM DUAL;

-- 뒤에서부터 첫번째 위치
SELECT INSTR('All fortune is to be conquered by bearing it.', 'e', -1, 1) 결과 FROM DUAL;
```

<hr>
<br>

## LPAD() 함수
- `LPAD(대상문자열, 전체자릿수, 채울문자열)`
- 특정 문자로 빈 공간을 채우는 함수. 채우는 위치는 LEFT.
- 유사한 함수로 RPAD() 함수가 있다. 채우는 위치는 RIGHT.

```sql
WITH MAP_O
     AS (SELECT '201909091324' EMP_ID
               ,'홍길동' EMP_NM
               ,'대조선국' ORG_ID
           FROM DUAL)
SELECT EMP_ID
      ,EMP_NM
      ,ORG_ID
      ,LPAD(ORG_ID, 15) --1
      ,LPAD(ORG_ID, 15, ' ') --2
      ,LPAD(ORG_ID, 15, '0') --3
      ,LPAD(ORG_ID, 15, 'A') --4
  FROM MAP_O;
```

<hr>
<br>

## RPAD() 함수
- `RPAD(대상문자열, 전체자릿수, 채울문자열)`
- 특정 문자로 빈 공간을 채우는 함수. 채우는 위치는 RIGHT.
- 유사한 함수로 LPAD() 함수가 있다. 채우는 위치는 LEFT.

```sql
WITH MAP_O
     AS (SELECT '201909091324' EMP_ID
               ,'홍길동' EMP_NM
               ,'대조선국' ORG_ID
           FROM DUAL)
SELECT EMP_ID
      ,EMP_NM
      ,ORG_ID
      ,RPAD(ORG_ID, 15) --1
      ,RPAD(ORG_ID, 15, ' ') --2
      ,RPAD(ORG_ID, 15, '0') --3
      ,RPAD(ORG_ID, 15, 'A') --4
  FROM MAP_O;
```

<hr>
<br>

## TRIM() 함수
- `TRIM(문자열)`
- 문자열의 양쪽 공백을 제거하는 기본적인 함수. 
- 반복적인 문자나 특정 문자를 제거할 때 자주 사용한다.
- 유사한 함수 LTRIM(), RTRIM() 함수는 왼쪽과 오른쪽의 공백을 제거할 때 사용한다.

```sql
WITH MAP_O
     AS (SELECT '201909091324' EMP_ID
               ,'홍길동       ' EMP_NM
               ,'  대조선국  ' ORG_ID
           FROM DUAL)
SELECT EMP_ID
      ,EMP_NM
      ,TRIM(EMP_NM)
      ,ORG_ID
      ,TRIM(ORG_ID)
  FROM MAP_O;
```

<hr>
<br>

## LTRIM() 함수
- `LTRIM(대상문자열, 특정문자열)`. 특정문자열 생략 가능.
- 특정 문자의 빈 공간(특정 문자)을 제거하는 함수. 제거하는 위치는 LEFT.
- 유사한 함수로 RTRIM() 함수가 있다. 제거하는 위치는 RIGHT.

```sql
WITH MAP_O
     AS (SELECT '201909091324' EMP_ID
               ,'홍길동       ' EMP_NM
               ,'  대조선국  ' ORG_ID
           FROM DUAL)
SELECT EMP_ID
      ,LTRIM(EMP_ID, '0')
      ,EMP_NM
      ,LTRIM(EMP_NM, '홍길')
      ,ORG_ID
      ,LTRIM(ORG_ID)
  FROM MAP_O;
```

<hr>
<br>

## RTRIM() 함수
- `RTRIM(대상문자열, 특정문자열)`. 특정문자열 생략 가능.
- 특정 문자의 빈 공간(특정 문자)을 제거하는 함수. 제거하는 위치는 RIGHT;
- 유사한 함수로 LTRIM() 함수가 있다. 제거하는 위치는 LEFT.

```sql
WITH MAP_O
     AS (SELECT '201909091324' EMP_ID
               ,'홍길동       ' EMP_NM
               ,'  대조선국  ' ORG_ID
           FROM DUAL)
SELECT EMP_ID
      ,RTRIM(EMP_ID, '0')
      ,EMP_NM
      ,RTRIM(EMP_NM)
      ,ORG_ID
      ,RTRIM(ORG_ID, '국  ')
  FROM MAP_O;
```

<hr>
<br>

## REPLACE() 함수
- `REPLACE(대상문자열, 찾는문자열, 치환문자열)`
- 대상 문자열에서 특정 문자열을 다른 문자열로 치환하는 함수.
- 주의) 찾는 문자열이 없는 경우 치환이 일어나지 않는다.

```sql
SELECT REPLACE('All fortune is to be conquered by bearing it.', 'it.', 'it!') 결과 FROM DUAL;
```

<hr>
<br>

## 숫자형 함수
- NUMBER 데이터를 처리하기 위한 함수.

```sql
SELECT ROUND(123.456, 2) --반올림
      ,TRUNC(123.456, 1) --버림
      ,MOD(10, 3) --나머지
      ,CEIL(123.45) --해당 값보다 큰수중 가장 작은 정수(올림)
      ,FLOOR(123.45) --해당 값보다 작은수 중 가장 큰 정수(버림)
  FROM DUAL;
```

<br>

### ROUND() 함수
- `ROUND(대상숫자, 자릿수)`. 자릿수 지정 생략 가능.
- 반올림 처리 함수
- 유사한 함수로 TRUNC()함수가 있다.

<br>

### TRUNC() 함수
- `TRUNC(대상숫자, 자릿수)`. 자릿수 지정 생략 가능.
- 절삭 처리 함수
- 유사한 함수로 ROUND()함수가 있다. 반올림 기능.
- 몫 연산에도 사용 가능.

<br>

### MOD() 함수
- `MOD(대상숫자, 자릿수)`. 자릿수 지정 생략 가능
- 나머지 결과 반환하는 함수
- 유사한 함수로 TRUNC() 함수가 있다. 몫 연산 결과 반환.

<br>

### CEIL() 함수
- `CEIL(대상숫자)`
- 올림 함수
- 대상숫자보다 크거나 같은 정수 중 제일 작은 수
- 유사한 함수로 FLOOR() 함수가 있다.

<br>

### FLOOR() 함수
- `FLOOR(대상숫자)`
- 버림 함수
- 대상숫자보다 작거나 같은 정수 중 제일 큰 수
- 유사한 함수로 CEIL() 함수가 있다.

<hr>
<br>

## 날짜 함수
- 날짜를 더하거나 빼는 연산에 사용한다.
- SYSDATE, MONTHS_BETWEEN() 등이 있다.

<br>

### SYSDATE 함수
- 시스템의 오늘 날짜(시간)를 반환하는 함수
- 주의) 매개변수 전달값이 없기 때문에 ()를 표기하지 않는다.
- 날짜 자료를 가지고 산술 연산 가능. 날짜 단위로 계산된다.

```sql
SELECT SYSDATE
      ,SYSDATE + 1 하루더하기
      ,SYSDATE + (1 / 24) 한시간더하기
      ,SYSDATE + ((1 / 24) / 60) 일분더하기
      ,SYSDATE + (((1 / 24) / 60) / 60) 일초더하기
  FROM DUAL;
```

<br>

### MONTHS_BETWEEN() 함수
- `MONTHS_BETWEEN(첫번째 날짜, 두번째 날짜)`
- 날짜 자료를 가지고 산술 연산(빼기 연산) 가능. 월(개월) 단위로 계산된다.
- 유사한 함수로 ADD_MONTHS() 함수가 있다. 더하기 연산.

```sql
SELECT -- 31일 이후 날짜에서 오늘 날짜와의 개월 수를 구한 값(양수)
       MONTHS_BETWEEN(SYSDATE + 31, SYSDATE) RESULT1
      , -- 오늘 날짜에서 31일 이후 날짜와의 개월 수를 구한 값(음수)
       MONTHS_BETWEEN(SYSDATE, SYSDATE + 31) RESULT2
      , -- 1달이 안되는 날짜에 대해서 개월 수를 구한 값
       MONTHS_BETWEEN(SYSDATE + 1, SYSDATE - 31) RESULT3
  FROM DUAL;
```

<br>

### ADD_MONTHS() 함수
- `ADD_MONTHS(첫번째 날짜, 두번째 날짜)`
- 날짜 자료를 가지고 산술 연산(더하기 연산) 가능. 월(개월) 단위로 계산된다.
- 유사한 함수로 MONTHS_BETWEEN() 함수가 있다. 빼기 연산.

```sql
SELECT SYSDATE
      ,ADD_MONTHS(SYSDATE, 1) 한달더하기
      ,ADD_MONTHS(SYSDATE, -1) 한달빼기
  FROM DUAL;
```

<br>

### NEXT_DAY() 함수
- `NEXT_DAY(날짜, 요일에 해당되는 날짜)`
- 해당 날짜 기준으로 명시된 요일에 해당하는 날짜를 구하는 함수.
- 명시된 날짜는 일, 월, 화, 수, 목, 금, 토
- 명시된 날짜는 숫자로도 가능(1:일요일, 2:월요일 ... 7:토요일)

```sql
SELECT SYSDATE
      ,NEXT_DAY(SYSDATE, '수') 수요일구하기
      ,NEXT_DAY(SYSDATE, 5) 금요일구하기
  FROM DUAL;
```

<br>

### LAST_DAY() 함수
- `LAST_DAY(날짜)`
- 해당 날짜가 속한 달의 마지막 날짜를 반환하는 함수.

```sql
SELECT SYSDATE
      ,LAST_DAY(SYSDATE) 마지막날짜구하기
  FROM DUAL;
```

<hr>
<br>

## TO_CHAR() 함수
- `TO_CHAR(날짜 또는 숫자)`
- 날짜, 숫자 자료를 문자 자료로 형변환하는 함수.
- 서식 지정 내용에 따라서 여러가지 형태의 문자열로 변환할 수 있다.
- 날짜 서식은 YYYY, MM, DD, DAY, HH24, MI, SS 등이 있다.
- 숫자 서식은 9, 0, .(dot), ,(comma) 등의 서식 사용.

```sql
-- 년도 출력시 2자리 사용하는 경우
SELECT '2060-01-01'
      ,'1905-01-01'
      ,TO_CHAR(TO_DATE('2060-01-01'), 'YY/MM/DD')
      ,TO_CHAR(TO_DATE('1905-01-01'), 'YY/MM/DD')
      ,TO_DATE(TO_CHAR(TO_DATE('1905-01-01'), 'YY/MM/DD'))
      ,TO_DATE(TO_CHAR(TO_DATE('1905-01-01'), 'YYYY/MM/DD'))
  FROM DUAL;

-- 숫자 출력시 서식 지정
SELECT 12345.67
      ,TO_CHAR(12345.67, '99999')
      ,TO_CHAR(12345.67, '99999.99')
      ,TO_CHAR(12345.67, '99,999.99')
      ,CONCAT('A', LTRIM(TO_CHAR(1, '0000'))) NEWNUM
  FROM DUAL;
  ```

<hr>
<br>

## TO_NUMBER() 함수
- `TO_NUMBER(숫자 형태의 문자열)`
- (숫자 형태의)문자 자료를 숫자 자료로 형변환하는 함수

```sql
SELECT '20600101'
      ,'19050101'
      ,TO_NUMBER('20600101')
      ,TO_NUMBER('19050101')
  FROM DUAL;
```

<hr>
<br>

## TO_DATE() 함수
- `TO_DATE(날짜 형태의 문자열)`
- (날짜 형태의)문자 자료를 날짜 자료로 형변환하는 함수

```sql
SELECT '2060-01-01'
      ,'1905-01-01'
      ,TO_DATE('2060-01-01')
      ,TO_DATE('1905-01-01')
  FROM DUAL;
```

<hr>
<br>

## NVL() 함수
- `NVL(값, 지정값)`
- NULL 값을 다른 값으로 대체해서 반환하는 함수
- NULL 값을 가진 컬럼을 연산에 참여하는 경우 최종 결과 전체가 NULL이 된다. NULL 대신 연산 가능한 값으로 대체해야하는 경우가 있다.
- 유사한 함수로 NVL2() 함수가 있다. NULL일때의 대체값과 NULL이 아닐때의 대체값이 별도 존재.

```sql
WITH MAP_O
     AS (SELECT '201909091324' EMP_ID
               ,'홍길동       ' EMP_NM
               ,'  대조선국  ' ORG_ID
               ,NULL MGR
           FROM DUAL)
SELECT MGR
      ,NVL(MGR, 0)
  FROM MAP_O;
```

<hr>
<br>

## NVL2() 함수
- `NVL2(값, 지정값1, 지정값2)`
- NULL 값을 다른 값으로 대체해서 반환하는 함수. NULL이 아닌 경우에 대한 대체값도 있다.
- NULL 값을 가진 컬럼을 연산에 참여하는 경우 최종 결과 전체가 NULL이 된다. NULL 대신 연산 가능한 값으로 대체해야하는 경우가 있다.
- 유사한 함수로 NVL() 함수가 있다. NULL일때의 대체값만 존재.

```sql
WITH MAP_O
     AS (SELECT '201909091324' EMP_ID
               ,'홍길동       ' EMP_NM
               ,'  대조선국  ' ORG_ID
               ,NULL MGR
               ,NULL MON
           FROM DUAL)
SELECT MGR
      ,MON
      ,NVL2(MGR, '매니저', '사원')
      ,NVL2(MON, '지급', '미지급')
  FROM MAP_O;
```

<hr>
<br>

## DECODE() 함수
- `DECOCE(컬럼, 비교값1, 결과값1, 비교값2, 결과값2,….)`
- `DECOCE(컬럼, 비교값1, 결과값1)`
- `DECOCE(컬럼, 비교값1, 결과값1, 비교값2, 결과값2)`
- `DECOCE(컬럼, 비교값1, 결과값1, 비교값2, 결과값2, 결과값3)`
- IF ~ ELSE IF ~ ELSE 문 역할을 하는 함수.
- 특정 조건(일치하는 경우만 검사 가능)을 만족하는지에 따라서 다른 결과를 반환하는 함수.
- ELSE 부분은 생략이 가능하며 해당 조건이 없으면 NULL이다.

```sql
WITH MAP_O
     AS (SELECT 'M' GENDER FROM DUAL
         UNION ALL
         SELECT 'F' GENDER FROM DUAL
         UNION ALL
         SELECT 'X' GENDER FROM DUAL)
SELECT GENDER
      ,DECODE(GENDER,  'M', '남자',  'F', '여자') GENDER2
  FROM MAP_O
```

<hr>
<br>

## CASE ~ END 구문
- 연산자가 =인 경우
  - `CASE 컬럼 WHEN 값1 THEN 결과1 WHEN 값2 THEN 결과2 WHEN 값3 THEN 결과3 … ELSE 결과 END`
- 연산자가 =이 아닌 경우
  - `CASE WHEN 조건식1 THEN 결과1 WHEN 조건식2 THEN 결과2 WHEN 조건식3 THEN 결과3 … ELSE 결과 END`
- IF ~ ELSE IF ~ ELSE 문 역할을 하는 구문.
- 특정 조건을 만족하는지에 따라서 다른 결과를 반환하는 구문.

```sql
WITH MAP_O
     AS (SELECT 'M' GENDER FROM DUAL
         UNION ALL
         SELECT 'F' GENDER FROM DUAL
         UNION ALL
         SELECT 'X' GENDER FROM DUAL)
SELECT GENDER
      ,CASE WHEN GENDER = 'M' THEN '남자' WHEN GENDER = 'F' THEN '여자' ELSE '?' END GENDER2
      ,CASE GENDER WHEN 'M' THEN 'MALE' WHEN 'F' THEN 'FEMALE' ELSE '?' END GENDER3
  FROM MAP_O
```

<hr>
<br>

## 정규식 함수
- 정규식 표현을 이용해서 검색, 치환 등을 진행하는 함수.
- *이 항목은 데이터가 많으므로 상세한 설명을 생략한다.*

<br>

### REGEXP_LIKE() 함수
- 단일 LIKE : `REGEXP_LIKE(컬럼명, 입력값 또는 정규식, [매치옵션])`
- 다중 LIKE : `REGEXP_LIKE(컬럼명, 입력값 또는 정규식 | 입력값 또는 정규식)`
- 특정 패턴(정규표현식)과 매칭되는 결과를 반환하는 함수
- 매치옵션은 생략가능하다.

<br>

### REGEXP_REPLACE() 함수
- 특정 패턴(정규표현식)과 매칭되는 결과를 치환하는 함수

<br>

### REGEXP_SUBSTR() 함수
- 특정 패턴(정규표현식)과 매칭되는 결과에서 부분 문자열을 반환하는 함수

<br>

## 복수행 함수
- 여러개의 행을 투입해서 한 개의 결과를 반환하는 함수
- NULL 값을 가진 자료는 복수행 함수 연산시 제외된다. 예를 들어, SUM() 함수 사용시 NULL 값이 들어오면 자동으로 제외된다.
- 함수 사용시 단일행 함수, 일반 컬럼과 복수행 함수를 혼용할 수 없다.
- COUNT(), SUM(), AVG(), MAX(), MIN() 등이 있다.

<br>

### COUNT() 함수
- `COUNT(컬럼 또는 집합)`
- 투입된 행의 갯수 반환하는 함수

```sql
WITH MAP_O
     AS (SELECT 123 COUNT_TEST FROM DUAL
         UNION ALL
         SELECT 4567 COUNT_TEST FROM DUAL
         UNION ALL
         SELECT 89 COUNT_TEST FROM DUAL)
SELECT COUNT(*) 행갯수
  FROM MAP_O
```

<br>

### SUM() 함수
- `SUM(컬럼 또는 집합)`
- 투입된 행의 값(숫자 자료형)들을 모두 합산한 결과 반환.

```sql
WITH MAP_O
     AS (SELECT 123 SUM_TEST FROM DUAL
         UNION ALL
         SELECT 4567 SUM_TEST FROM DUAL
         UNION ALL
         SELECT 89 SUM_TEST FROM DUAL)
SELECT SUM(SUM_TEST) 더한값
  FROM MAP_O
```

<br>

### AVG() 함수
- `AVG(컬럼 또는 집합)`
- 투입된 행의 값(숫자 자료형)들의 평균 결과 반환.
- NULL 값은 제외되기 때문에 평균 계산 결과가 다르게 나올 수 있다.

```sql
WITH MAP_O
     AS (SELECT 123 AVG_TEST FROM DUAL
         UNION ALL
         SELECT 4567 AVG_TEST FROM DUAL
         UNION ALL
         SELECT 89 AVG_TEST FROM DUAL)
SELECT AVG(AVG_TEST) 평균값 
  FROM MAP_O
```

<br>

### MAX(), MIN() 함수
- `MAX(컬럼 또는 집합)`
- `MIN(컬럼 또는 집합)`
- 최대값, 최소값을 반환하는 함수

```sql
WITH MAP_O
     AS (SELECT 123 TEST_DATA FROM DUAL
         UNION ALL
         SELECT 4567 TEST_DATA FROM DUAL
         UNION ALL
         SELECT 89 TEST_DATA FROM DUAL)
SELECT MAX(TEST_DATA) 최대값
      ,MIN(TEST_DATA) 최솟값
  FROM MAP_O
```

<hr>
<br>

## DISTINCT() 함수
- `DISTINCT(컬럼 또는 집합)`
- 중복을 제거하여 반환

```sql
WITH MAP_O
     AS (SELECT 123 TEST_DATA FROM DUAL
         UNION ALL
         SELECT 4567 TEST_DATA FROM DUAL
         UNION ALL
         SELECT 89 TEST_DATA FROM DUAL
         UNION ALL
         SELECT 89 TEST_DATA FROM DUAL)
SELECT DISTINCT(TEST_DATA) 중복제거 
  FROM MAP_O
```

<hr>
<br>

## 분석함수
- 행끼리 연산이나 비교를 쉽게 지원해주기 위한 함수.
- ROLLUP(), CUBE(), GROUPING SETS(), LISTAGG(), PIVOT(), LAG() 등이 있다.

<br>

### ROLLUP() 함수
- 기준별(GROUP BY) 소계(COUNT, SUM)를 요약해서 보여주는 함수.

```sql
  --EHR.PA1020_V 테이블(뷰)에서 조직별(ORG_ID)로 인원수 출력
  SELECT ORG_ID
        ,STAT_CD
        ,COUNT(*)
    FROM PA1020_V
   WHERE C_CD = '10'
     AND LAST_YN = 'Y'
     AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD
GROUP BY ROLLUP(ORG_ID)
        ,STAT_CD
ORDER BY ORG_ID
        ,STAT_CD;
```

<br>

### CUBE() 함수
- 기준별(GROUP BY) 소계(COUNT, SUM)및 전체합계를 요약해서 보여주는 함수.

```sql
  SELECT ORG_ID
        ,STAT_CD
        ,COUNT(*)
    FROM PA1020_V
   WHERE C_CD = '10'
     AND LAST_YN = 'Y'
     AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD
GROUP BY CUBE(ORG_ID)
        ,STAT_CD
ORDER BY ORG_ID
        ,STAT_CD;
```

<br>

### GROUPING SETS() 함수
- 기준별(GROUP BY) 소계(COUNT, SUM)및 전체합계를 요약해서 보여주는 함수.

```sql
  SELECT ORG_ID
        ,STAT_CD
        ,COUNT(*)
    FROM PA1020_V
   WHERE C_CD = '10'
     AND LAST_YN = 'Y'
     AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD
GROUP BY GROUPING SETS(ORG_ID
                      ,STAT_CD)
ORDER BY ORG_ID
        ,STAT_CD;
```

<br>

### LISTAGG() 함수
- 출력시 자료를 하나의 문자열로 통합 출력.

```sql
  --EHR.PA1020_V 테이블(뷰)에서 조직별(ORG_ID) 인원수 및 명단(EMP_ID) 출력
  SELECT ORG_ID
        ,COUNT(*)
        ,LISTAGG(EMP_ID, '/') WITHIN GROUP (ORDER BY EMP_ID) EMP_IDS
    FROM PA1020_V
   WHERE C_CD = '10'
     AND LAST_YN = 'Y'
     AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD
GROUP BY ORG_ID;
```

<br>

### PIVOT() 함수
- 출력시 가로 형태를 세로 형태로 바꿔주는 함수.

```sql
-- 원본 출력 데이터
  SELECT ORG_ID
        ,COUNT(*)
        ,COUNT(DECODE(ORG_ID, '10000', 1)) 결과1
        ,COUNT(DECODE(ORG_ID, '322105', 1)) 결과2
        ,COUNT(DECODE(ORG_ID, '311098', 1)) 결과3
    FROM PA1020_V
   WHERE C_CD = '10'
     AND LAST_YN = 'Y'
     AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD
GROUP BY ORG_ID;

-- 세로 형태로 바꿔 출력한 데이터
SELECT *
  FROM (SELECT EMP_ID
              ,ORG_ID
          FROM PA1020_V
         WHERE C_CD = '10'
           AND LAST_YN = 'Y'
           AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD)
  PIVOT (COUNT(EMP_ID) FOR ORG_ID IN ('10000', '322105', '311098'))
```

<br>

### LAG() 함수
- 이전 행 값을 가져오는 함수.

```sql
  -- EHR.PA1020_V 테이블(뷰)에서 입사일(STA_YMD)를 출력하되 이전 행의 입사일(STA_YMD)를 같이 출력
  SELECT EMP_ID
        ,STA_YMD
        ,END_YMD
        ,LAG(STA_YMD, 1, 0) OVER (ORDER BY STA_YMD DESC) LAG_
        ,STA_YMD - LAG(STA_YMD, 1, 0) OVER (ORDER BY STA_YMD DESC) LAG2_
    FROM PA1020_V
   WHERE C_CD = '10'
     AND LAST_YN = 'Y'
     AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD
ORDER BY STA_YMD DESC;
```

<br>

### LEAD() 함수
- 이후 행 값을 가져오는 함수.

```sql
-- EHR.PA1020_V 테이블(뷰)에서 입사일(STA_YMD)를 출력하되 이후 행의 입사일(STA_YMD)를 같이 출력
  SELECT EMP_ID
        ,STA_YMD
        ,END_YMD
        ,LEAD(STA_YMD, 1, 0) OVER (ORDER BY STA_YMD DESC) LEAD_
        ,STA_YMD - LEAD(STA_YMD, 1, 0) OVER (ORDER BY STA_YMD DESC) LEAD2_
    FROM PA1020_V
   WHERE C_CD = '10'
     AND LAST_YN = 'Y'
     AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD
ORDER BY STA_YMD DESC;
```

<br>

### RANK() 함수
- 전체 순위 부여 함수.
- 동일한 값이면 중복 순위를 부여하고 다음 순위는 해당 개수만큼 건너뛰고 반환한다.

```sql
--EHR.PA1020_V 테이블(뷰)에서 입사일(STA_YMD)가 빠른 순에서 5번째까지
SELECT *
  FROM (SELECT EMP_ID
              ,ORG_ID
              ,STA_YMD
              ,RANK() OVER (ORDER BY STA_YMD ASC) RANK_
          FROM PA1020_V
         WHERE C_CD = '10'
           AND LAST_YN = 'Y'
           AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD)
 WHERE RANK_ <= 5;
```

<br>

### DENSE_RANK() 함수
- 전체 순위 부여 함수.
- 동일한 값이면 중복 순위를 부여하고 다음 순위는 중복 순위와 상관 없이 순차적으로 반환한다.

```sql
SELECT EMP_ID
      ,ORG_ID
      ,STA_YMD
      ,DENSE_RANK() OVER (ORDER BY STA_YMD DESC) RANK_
  FROM PA1020_V
 WHERE C_CD = '10'
   AND LAST_YN = 'Y'
   AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD
```

<br>

### ROW_NUMBER() 함수
- 고유 번호를 순번대로 부여하는 함수.
- 중복에 상관없이 순차적으로 순위를 반환한다.
- 동일한 데이터여도 중복 순위가 없다.

```sql
SELECT EMP_ID
      ,ORG_ID
      ,STA_YMD
      ,ROW_NUMBER() OVER (ORDER BY STA_YMD DESC) RN_
  FROM PA1020_V
 WHERE C_CD = '10'
   AND LAST_YN = 'Y'
   AND TO_CHAR(SYSDATE, 'YYYYMMDD') BETWEEN STA_YMD AND END_YMD
```

<br>

### SUM() OVER() 함수
- 그룹별 누계를 출력해주는 함수.

```sql
WITH MAP_O
     AS (SELECT 123 SUM_TEST
               ,'GROUP_B' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 4567 SUM_TEST
               ,'GROUP_B' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 89 SUM_TEST
               ,'GROUP_A' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 4091 SUM_TEST
               ,'GROUP_A' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 13312 SUM_TEST
               ,'GROUP_A' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 56 SUM_TEST
               ,'GROUP_B' SUM_TEST2
           FROM DUAL)
SELECT SUM_TEST, SUM(SUM_TEST) OVER (PARTITION BY SUM_TEST2) 그룹별누계
  FROM MAP_O;
```

<br>

### RATIO_TO_REPORT() 함수
- 그룹별 비중을 출력해주는 함수.

```sql
WITH MAP_O
     AS (SELECT 123 SUM_TEST
               ,'GROUP_B' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 4567 SUM_TEST
               ,'GROUP_B' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 89 SUM_TEST
               ,'GROUP_A' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 4091 SUM_TEST
               ,'GROUP_A' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 13312 SUM_TEST
               ,'GROUP_A' SUM_TEST2
           FROM DUAL
         UNION ALL
         SELECT 56 SUM_TEST
               ,'GROUP_B' SUM_TEST2
           FROM DUAL)
SELECT SUM_TEST
      ,SUM(SUM_TEST) OVER (PARTITION BY SUM_TEST2) 그룹별누계
      ,ROUND((SUM_TEST / SUM(SUM_TEST) OVER (PARTITION BY SUM_TEST2)) * 100, 1) "RATIO_%" -- 연산식을 사용한 비중 출력
      ,ROUND(RATIO_TO_REPORT(SUM_TEST) OVER (PARTITION BY SUM_TEST2) * 100, 1) "RATIO2_%" -- 함수를 사용한 비중 출력
  FROM MAP_O;
```

<hr>
<br>

## 서브쿼리(SubQuery)
- 메인 쿼리 내에 SELECT 쿼리가 포함된 상태인 쿼리
- 서브쿼리는 ()로 감싸야 하며 서브쿼리를 먼저 실행한 후 메인쿼리가 실행된다.

```sql
SELECT SELECT_LIST
  FROM TABLE_LIST
 WHERE 컬럼 연산자 (서브쿼리);
```

<hr>
<br>

## 단일 행 서브쿼리
- 서브쿼리 결과가 1개의 행만 나오는 경우에 사용
- 연산자는 비교 연산자(=, !=, >, <, >=, <=)를 사용한다.

```sql
SELECT SELECT_LIST
  FROM TABLE_LIST
 WHERE 컬럼 = (결과값);
```

<hr>
<br>

## 다중 행 서브쿼리
- 서브쿼리 결과가 여러개의 행이 나오는 경우에 사용
- 연산자는 IN, EXISTS, > ALL, > ANY, < ALL, < ANY 를 사용한다.

```sql
SELECT SELECT_LIST
  FROM TABLE_LIST
 WHERE 컬럼 IN (결과값1, 결과값2, ...);
```

<hr>
<br>

## 다중 컬럼 서브쿼리
- 결과가 여러 컬럼으로 구성된 경우에 사용한다.

```sql
SELECT SELECT_LIST
  FROM TABLE_LIST
 WHERE (컬럼1, 컬럼2, ...) IN (컬럼1_결과값, 컬럼2_결과값, ...);
```

<hr>
<br>

## 상호 연관 서브쿼리
- 메인 쿼리의 결과가 서브쿼리에 참여하고, 서브쿼리의 결과가 메인 쿼리에 참여하는 경우에 사용한다.
- 메인 쿼리의 테이블에 대해서 반드시 별칭 사용하여 구분한다.

```sql
SELECT SELECT_LIST
  FROM TABLE_LIST 별칭
 WHERE 컬럼 연산자 (서브쿼리의 조건식에 메인쿼리 컬럼 정보 참여);
```

<hr>
<br>

## 스칼라 서브쿼리
- 메인 쿼리의 결과가 서브쿼리에 참여하고, 서브쿼리의 결과가 메인 쿼리에 참여하는 경우에 사용한다.
- 메인 쿼리의 테이블에 대해서 반드시 별칭 사용하여 구분한다.
- OUTER JOIN과 같은 결과이며 JOIN 구문을 권장한다.

```sql
SELECT SELECT_LIST
      ,(서브쿼리의 조건식에 메인쿼리 컬럼 정보 참여)
FROM TABLE_LIST 별칭;
```

<hr>
<br>