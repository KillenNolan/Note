# Python 重要模块

## 导入模块的两种方式

1. **import sys** 導入模塊sys
2. **from sys import platform** 導入模塊sys的指定函數

```python
#導入模塊 运行解释器的系统
import sys 
print(sys.platform) #你的操作系统	win32
print(sys.version) #Python版本的信息 3.3.6 (default, Mar 29 2018, 23:29:00) [MSC v.1600 32 bit (Intel)]
 
    
    
#這個模塊提供了一種平台獨立的方式 來與底層操作系統交互
import os 
print(os.getcwd()) #返回所在文件夹的名字 E:\SOFTWARE\Develope\Sublime\Sublime Text Build 3211\Data\Packages\Nodejs
print(os.environ) #系统全部的环境变量
print(os.getenv('PATH')) #系统PATH属性的环境变量



import datetime


import html


```



 

