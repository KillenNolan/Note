# Windows之安装软件(install)

### Windows10下安装软件报2503和2502
Win10下无法安装SVN客户端解决办法
环境：
win10 64bits + TortoiseSVN-1.10.0.28176-x64-svn-1.10.0.msi

安装时所报错码：
2503和2502，出现这两个错误提示框后，安装就自动退出。

解决办法：

1. 右击左下角的开始菜单，先择“命令行提示符(管理员)”
2. 在弹出的DOS界面
   msiexec /package “D:\tools\TortoiseSVN-1.10.0.28176-x64-svn-1.10.0.msi” (这里的路径换成自己的就行)
3. SVN客户端安装开始，这时再下一步式安装，就举弹出出错框了。 
   原文链接：https://blog.csdn.net/linuxandroidwince/article/details/80793075

