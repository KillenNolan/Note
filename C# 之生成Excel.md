# C# 之导出Excel

## 主要步骤

1. 数据库查询
2. 导入Excel

## 数据库查询

1. 分页
2. 多线程分页

```
fuser	Fuser#01
ap3		nsd0170ap3
web2	nsd_web2pw69
pm1		Pm1_1987
ap2		nsdap2logpd0522
efoxap	mes_nsdefox

```



## 生成集合



POI操作EXCEL对象
HSSF：操作Excel 97(.xls)格式
XSSF：操作Excel 2007 OOXML (.xlsx)格式，操作EXCEL内存占用高于HSSF
SXSSF:从POI3.8 beta3开始支持，基于XSSF，低内存占用。



如果未分页，一次查出上万条数据，甚至更高，**会长时间占用数据库连接**，导致系统并发量下降。另外、由于一次加载数据过多（数据库查询的数据通常会放入到java的一个集合），**会长时间占用虚拟机的堆内存**，直到生成EXCEL结束，GC才有可能对此集合对象进行回收，这样并发量大时，很容易造成堆内存溢出。在数据库分页查询过程中，每页建议在5000左右，实践证明，生成EXCEL过程，时间主要花费在了数据库查询上，写EXCEL是很快的，所以如果每页条数太小，要查询多次数据库，数据库查询是比较耗时的，这样整体生成EXCEL的时间要长。



