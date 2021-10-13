# Oracle 查询分页

```sql
--0.067 
 SELECT *
  FROM (SELECT *
          FROM (SELECT ROW_NUMBER () OVER (ORDER BY track_code) AS rownumber, t.*
                  FROM (mes4.r_fixture_online_detail) t) p
         WHERE p.rownumber <= 4000) table_alias
 WHERE rownumber > 3990;
 
 
 --0.009
  SELECT *
  FROM (SELECT *
          FROM (SELECT ROW_NUMBER () OVER (ORDER BY track_code) AS rownumber, t.*
                  FROM (mes4.r_fixture_online_detail) t) p
         WHERE rownum <= 4000) table_alias
 WHERE rownumber > 3990;
 
 --0.105
  SELECT *
  FROM (SELECT *
          FROM (SELECT ROW_NUMBER () OVER (ORDER BY track_code) AS rownumber, t.*
                  FROM (mes4.r_fixture_online_detail) t) p
         WHERE p.rownumber <= 14000) table_alias
 WHERE rownumber > 13990;
 
 --0.114
  SELECT *
  FROM (SELECT *
          FROM (SELECT ROW_NUMBER () OVER (ORDER BY track_code) AS rownumber, t.*
                  FROM (mes4.r_fixture_online_detail) t) p
         WHERE rownum <= 14000) table_alias
 WHERE rownumber > 13990;
 
 
 --我的新宠，最好用有索引排序的栏位来排序，速度会快很多， 
 --单ROWNUM <= 6000000的上限大概是六百万，大于这个这个数，查询时间超过一秒
 --ROWNUM <= 6000000且table_alias.rowno > 599990，这两个数值越接近，查询速度越慢，两个条件同时满足，大概适用ROWNUM <= 600000（60万的数据），这样分页显示一页十行不超过一秒
 
  SELECT *
  FROM (SELECT tt.*, ROWNUM AS rowno
          FROM (  SELECT t.*
                    FROM LSS_FILE_ROUTE_LOG t  
                ORDER BY CREATEDT DESC) tt
         WHERE ROWNUM <= 600000 --and CREATEDT between to_char(sysdate-30,'yyyy-MM-dd') and to_char(sysdate,'yyyy-MM-dd') 
         ) table_alias
 WHERE table_alias.rowno > 599990 -- and message_id='FOC000000021907166' 
 ; 

```

