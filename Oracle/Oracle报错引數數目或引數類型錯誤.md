# Oracle报错:引數數目或引數類型錯誤

## ORA-06550
- 情景描述

在我通过C#调用Procedure(SP)时，报这个错误，之前用的是System.Data.OracleClient.dll，后代码升级至.NET Core 3.1，换了Oracle.ManagedDataAccess.Client.dll。

- 报错信息：ORA-06550: 第 1 行, 第 7 個欄位: \nPLS-00306: 呼叫 'xxx' 時使用的**引數數目或引數類型錯誤**\nORA-06550: 第 1 行, 第 7 個欄位: \nPL/SQL: Statement ignored
- 原因:OracleType.VarChar ==》OracleDbType.NVarchar2 ==》OracleDbType.Varchar2（最终OK）
- [参考文献](https://www.cnblogs.com/sumsen/archive/2012/05/30/2525735.html)

```C#
/*  Varchar2 和 NVarchar2
	Varchar2    SQL通用
    NVarchar2   Oracle独有 1.把空串等同于null处理 
*/
```

數字或值錯誤: 字元字串緩衝區太小



 //跑返回string的sp，未设置size导致输出报错：數字或值錯誤: 字元字串緩衝區太小
                oraParas1[“RES”].Size = 2000;



## ORA-24338: 未执行语句句柄

- 情景描述

  在我通过C#写的类调用执行SP的时候，报错。

- 报错代码

  -  EXISTS2：是我在网上百度的一个函数，用来判断有没有数据。有则返回1。		
  - RAISE l_exit：跳转到我打开有游标的位置（ OPEN user_cusor FOR l_strquery;）

  ```sql
  CREATE OR REPLACE FUNCTION EXISTS2 (IN_SQL IN VARCHAR2)
    RETURN NUMBER
  IS
    V_SQL VARCHAR2(4000);
    V_CNT NUMBER(1);
  BEGIN
    V_SQL := 'SELECT COUNT(*) FROM DUAL WHERE EXISTS (' || IN_SQL || ')';
    EXECUTE IMMEDIATE V_SQL INTO V_CNT;
    RETURN(V_CNT);
  END;
  /
  ```

  ```sql
  -- 原来的语句
  l_strquery := ' SELECT * FROM ' || g_table || ' where 1=1 ' || g_where;
  IF EXISTS2 (l_strquery) = 1
  	THEN
          l_o_message := 'Data already exists!';
        	RAISE l_exit; 
  END IF;
  ```

  ```sql
  --  修改后的语句
  l_strquery := ' SELECT * FROM ' || g_table || ' where 1=1 ' || g_where;
  IF EXISTS2 (l_strquery) = 1
  	THEN
           l_o_message := 'Data already exists!';
      ELSE
           l_o_message := 'OK';
  END IF;
  RAISE l_exit;
  ```

- 原因：因为我的返回参数是游标，原来的语句只有一种情况会返回游标，修改后就包含了所有情况都会返回游标。

- 参考文献[ORA-24338: 未执行语句句柄](https://www.cnblogs.com/furenjun/archive/2009/03/30/1425320.html)