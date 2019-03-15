# Charset 변경

## 1단계
- SQLPLUS 접속 후 system 계정으로 로그인
  - 계정 로그인이 불가능 시
    ```sh
    C:\> sqlplus /nolog
    sql> conn /as sysdba
    sql> conn sys as sysdba
    ```

## 2단계
- CHARSET 확인 쿼리문
    ```sql
    select parameter, value from nls_database_parameters where parameter = 'NLS_CHARACTERSET'
    ```

<br>

- CHARSET 변경은 SYSTEM 계정으로 진행해야 한다.

<br>

### `KO16MSWIN949`
- SQL commit
    ```sql
    update sys.props$ set value$='KO16MSWIN949' where name='NLS_CHARACTERSET';

    update sys.props$ set value$='KO16MSWIN949' where  name='NLS_NCHAR_CHARACTERSET';

    update sys.props$ set value$='KOREAN_KOREA.KO16MSWIN949' where name='NLS_LANGUAGE';

    commit;
    ```

<br>

- CLI
    ```sql
    SHUTDOWN IMMEDIATE;

    STARTUP MOUNT;

    ALTER SYSTEM ENABLE RESTRICTED SESSION;

    ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0;

    ALTER SYSTEM SET AQ_TM_PROCESSES=0;

    ALTER DATABASE OPEN;

    ALTER DATABASE CHARACTER SET INTERNAL_USE KO16MSWIN949;

    SHUTDOWN IMMEDIATE;

    STARTUP;
    ```

<br>

### `UTF-8`
- SQL commit
    ```sql
    update sys.props$ set value$='UTF8' where name='NLS_CHARACTERSET';

    update sys.props$ set value$='UTF8' where  name='NLS_NCHAR_CHARACTERSET';

    update sys.props$ set value$='KOREAN_KOREA.UTF8' where name='NLS_LANGUAGE';

    commit;
    ```

<br>

- CLI
    ```sql
    SHUTDOWN IMMEDIATE;

    STARTUP MOUNT;

    ALTER SYSTEM ENABLE RESTRICTED SESSION;

    ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0;

    ALTER SYSTEM SET AQ_TM_PROCESSES=0;

    ALTER DATABASE OPEN;

    ALTER DATABASE CHARACTER SET INTERNAL_USE UTF8;

    SHUTDOWN IMMEDIATE;
    
    STARTUP;
    ```