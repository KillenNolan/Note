# Oracle 之JOB

## 创建JOB

```mysql

DECLARE
  X NUMBER;
BEGIN
  SYS.DBMS_JOB.SUBMIT
  ( job       => X 
   ,what      => 'delete  SOM.R_CHECKDATA_LOG  where check_time < to_date(''2019/01/01'',''YYYY/MM/DD'')and rownum<10000 ; commit;'
   ,next_date => to_date('21/10/2020 19:13:02','dd/mm/yyyy hh24:mi:ss')
   ,interval  => 'SYSDATE+1/1440 '
   ,no_parse  => FALSE
   ,instance  => 1
  );
  SYS.DBMS_OUTPUT.PUT_LINE('Job Number is: ' || to_char(x));
COMMIT;
END;
/
```

## 执行JOB

```sql

BEGIN 
  SYS.DBMS_JOB.REMOVE(35);
COMMIT;
END;
/
```

## 查询已创建的JOB

```sql
--权限更大
select * from dba_jobs;

select * from DBA_JOBS_RUNNING;

select * from user_jobs;
```

