# Oracle工具函数

## 判读是否存在数据的函数

- 背景：我写了一个基础通用配置网页，但忘了Check这笔数据是否已存在，且因为我的配置SQL是动态的，不好直接Check。

- 代码

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

- 用法

  ```sql
  BEGIN
      l_strquery := ' SELECT * FROM ' || g_table || ' where 1=1 ' || g_where;
      IF EXISTS2 (l_strquery) = 1
          THEN
               l_o_message := 'Data already exists!';
          ELSE
               l_o_message := 'OK';
      END IF;
      RAISE l_exit;
  END;
  ```

-   参考文章 [IF EXISTS用法](https://blog.csdn.net/weixin_43347659/article/details/107377389)

