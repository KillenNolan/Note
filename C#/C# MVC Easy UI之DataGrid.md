# C# MVC Easy UI之DataGrid

## 加载数据

```js
# $('#' + tabid)你可修改成你的 $('#tabMfrConfig')
// 加载数据到页面，此方法会导致插件分页失效 data:array [{},{}]
$('#' + tabid).datagrid({}).datagrid('loadData', data); 

$('#' + tabid).datagrid({}).datagrid('loadData', data); 



```





## 操作数据行



### 选择

```js
# 获取当前页的所有行
$("#" + tabid).datagrid("getRows");

#获取当前页選中的数据行数 array [{},{}]
$("#" + tabid).datagrid('getSelections')
```











