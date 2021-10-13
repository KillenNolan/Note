# DOS 之端口



## 端口介绍

服务端口的状态变化：LISTENING侦听====>ESTABLISHED===>TIME_WAIT访问结束
客户端口的状态变化：SYN_SENT请求连接====>ESTABLISHED===>TIME_WAIT浏览网页完毕

## 查找端口

根据端口号查找对应的进程号 netstat -aon|findstr "1903"

据 (PID/进程号)寻找进程名称  tasklist | findstr 26396

## 杀死进程



强制关闭某个进程：taskkill -PID 26396 -F
 結束進程：taskkill /f /t /im Tencentdl.exe

## 端口无法使用

> 端口 8888 無法使用

 因為其他的電腦或設備正在使用此端口
尋找使用 8888 端口的設備

1. 進入 command prompt
2. 查看端口状态： netstat -ano|more
3. 查看是哪個 IP address 在使用 :8888 端口 
根据端口号查找对应的进程号：netstat -aon|findstr “1903”
据 (PID/进程号)寻找进程名称 ：tasklist | findstr 26396
强制关闭某个进程：taskkill -PID 26396 -F
 結束進程：taskkill /f /t /im Tencentdl.exe

4. 找出來後 將使用此 IP 的設備暫時關閉。