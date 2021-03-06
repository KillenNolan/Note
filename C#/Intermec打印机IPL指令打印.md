# Intermec打印机IPL指令打印

### 介绍

打印指令有ZPL,IPL

Intermec PM43打印机只支持IPL指令打印。我在网上搜索的关于先将图像画好，在发送打印指令，结果没反应。



连接打印机的方式：LPT/COM/USB/TCP四种模式，适用于标签、票据、条码打印。
打印指令语言：ZPL、EPL、CPCL、TCP指令



### 共享打印指令

```
cmd下获取电脑名称
hostname
MyComputerName

cmd下打印指令

方式一：copy zpl.prn \\127.0.0.1\Zebra110Xi4
方式二：copy zpl.prn \\MyComputerName\Zebra110Xi4
```





# 网络打印机的优势及配置

http://www.beecloudnet.cn/baike/265.html



1. USB连接（打印机通过局域网中的一台电脑（主机）来获取接收其他电脑（客户端）的输入指令的连结方式）

   目前主要用于针式打印机、激光打印机、喷墨打印机连接。

2. 网线连接（打印机成为局域网中的一台“PC”，是不依赖计算机独立存在的，通过网线接入局域网，拥有自己的IP地址）

   目前主要用于办公使用的激光打印机、喷墨打印机。

3. 无线连接

   主要用于没有安全要求的激光打印机、喷墨打印机。

**共享打印机（本地打印机）：**打印机通过局域网中的一台电脑（主机）来获取接收其他电脑（客户端）的输入指令的连结方式，通常打印机直接连结主机，利用微软提供的“文件和打印机共享服务”来与客户端通讯。类似于共享文件夹的性质，它的优缺点也显而易见。

**优点：**只要用户使用的局域网环境为微软提供的Windows平台，那么可以在无须添加任何其他硬件的情况下，通过软件设置实现打印资源的共享；

**缺点：**需要一台电脑配置为打印服务器，提供这项服务，且当打印服务器关闭时，其他的电脑均无法访问打印资源。配置和维护打印服务器需要专业人员－具备相当的Windows网络管理的知识，不能依赖打印设备提供商，而需要由微软提供服务所需的支持。跨操作系统平台共享使用需要的维护开销将更大，相关支持也要由不同的软件系统厂商分别提供。

**点对点打印：**使用专门的“网络打印服务器”设备将打印机直接接入局域网。可以说这才是真正意义上的网络打印，实现了电脑对打印机的点对点访问，局域网里的每台电脑可以直接指令控制打印机而不会影响到其他人。这里的打印服务器和上面的不是一个概念，它指的是一种专用的硬件设备，对打印机提供了类似电脑的“网卡”所提供的功能——直接连结到网络。它的优缺点也很明显。优点：使用专门的设备来替代了一台电脑，节省了大量的资源。因为是端到端访问，各个电脑均不会彼此影响。安装和配置方法及其简单，一般人只需要具备基本的电脑操作能力就可以使用。维护上由设备的供应商提供统一支持。支持基于硬件的跨操作平台使用。尤其内置的打印服务器，在传输速度上较传统的连结方式快出很多缺点：需要投入相应的硬件设备添置费用