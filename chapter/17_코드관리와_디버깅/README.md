# 소스 관리
## 데이터 사전(Data Dictionary)
- 데이터 사전(Data Dictionary)이란 대부분 읽기전용으로 제공되는 테이블 및 뷰들의 집합으로 데이터베이스 전반에 대한 정보를 제공 한다.
- 오라클 데이터베이스는 명령이 실행 될 때 마다 데이터 사전을 Access 한다.
- DB작업동안 Oracle은 데이터 사전을 읽어 객체의 존재여부와 사용자에게 적합한 Access 권한이 있는지를 확인 한다. 또한 Oracle은 데이터 사전을 계속 갱신하여 DATABASE 구조, 감사, 사용자권한, 데이터등의 변경 사항을 반영 한다.

### 데이터 사전에 저장되는 내용
- 오라클의 사용자 정보
- 오라클 권한과 ROLE 정보
- 데이터베이스 스키마 객체(TABLE, VIEW, INDEX, CLUSTER, SYNONYM, SEQUENCE..) 정보
- 무결성 제약조건에 관한 정보
- 데이터베이스의 구조 정보
- 오라클 데이터베이스의 함수 와 프로지저 및 트리거에 대한 정보
- 기타 일반적인 DATABASE 정보

### 데이터 사전의 분류
#### *ALL_XXXX*
- ALL_로 시작하는 데이터 사전으로, 한 특정 사용자가 조회 가능한 모든 데이터사전을 의미 한다.
- 자신이 조회하려는 객체의 주인이 아니더라도 그 객체에 접근 할 수 있는 권한을 가지고 있다면 ALL_XXXX 뷰를 통하여 조회가 가능 하다.
    ```sql
    -- scott 사용자로 접속하여 아래 SQL문장을 실행해 보자.    
    SELECT table_name, tablespace_name 
    FROM ALL_TABLES;
    ```

#### *USER_XXXX*
- USER_로 시작하는 데이터 사전으로, 한 특정 사용자에게 종속되어 있고, 그 사용자가 조회 가능한 데이터 사전 뷰들로 ALL_XXXX 데이터 사전의 모든 정보의 부분 집합이다.
    ```sql
    -- scott 사용자로 접속하여 아래 SQL문장을 실행해 보자.    
    SELECT table_name, tablespace_name 
    FROM USER_TABLES;
    ```

#### *DBA_XXXX*
- DBA권한을 가진 사용자 만이 조회할 수 있는 데이터 사전으로서 모든 오라클 데이터베이스 객체에 대한 정보를 볼 수 있다.
- SELECT ANY TABLE 권한이 있는 사용자 또한 질의가 가능 하며 다른 사용자가 질의 하려면 앞에 SYS.이라는 접두어를 붙여야 한다.
    ```sql
    -- SYS계정으로 접속하여 아래 문장을 실행해 보자
    SELECT OWNER, OBJECT_NAME 
    FROM SYS.DBA_OBJECTS;
    ```

#### *V$_XXXX*
- Dynamic Performance View라고도 하며, 현재 Database의 상태에 관한 정보로 주로 DBA에게만 액세스가 허용되어 있다.
- 주로 DBA의 모니터링 작업용 정보를 제공하며, X$ 테이블을 베이스로 하는 뷰이다.

#### *X$_XXXX*
- X$ 뷰는 V$ 뷰가 보여주지 않는 정보를 보여준다.
- X$ 테이블은 오라클의 메모리정보를 볼 수있는 SQL 인터페이스 뷰들로 Oracle 데이터베이스의 가장 숨겨진 영역 중 하나이다.

### Reference
- [Oracle Database 전체 데이터사전](http://docs.oracle.com/cd/B28359_01/server.111/b28320/index.htm)