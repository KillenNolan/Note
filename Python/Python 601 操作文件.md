# Python 601 操作文件

## 操作文件

​		存储数据最容易的方方法都是将数据存储在一个***文本文件***中，当然也可以存储在***数据库***中，这里只讲存储在文本文件中。

​		Python提供了内置支持来实现文件的打开（open），处理（process）和关闭（close）。这种通用技术允许你打开一个文件，以某种方式处理其数据（读、写或追加数据），最后关闭文件。

```python
#查看目前你在哪个目录下写代码
>>> import os
>>> os.getcwd()
'D:\\KILLEN\\LEARN\\Example\\Python\\webapp'

#打开一个文件，处理写入I love python，最后关闭todos
>>> todos = open('todos.txt', 'a')
>>> print('I love python.', file = todos)
>>> todos.close()

#不出意外，open会返回一个流，我们将这个文件流赋给todos这个变量，后面我们就可以通过这个变量来操作这个文件
#'a':追加模式打开这个文件，不存在，则创建，没有'a'，会报错文件不存在
>>> todos1 = open('todos1.txt')
*** FileNotFoundError: [Errno 2] No such file or directory: 'todos1.txt'
#print('I love python.', file = todos)在文件todos中写入I love python
#close最后关闭这个文件流
```

​		***注意***：上面的写入数据，每个句子最好有标点符号，如果没有，走end=''的时候就会出点问题，但很遗憾我并不知道原因。

### open函数

>**'a'是打开一个文件来追加数据，那还有什么其他模式吗？**

​		open函数的第一个参数是要处理的文件名。第二个参数是可选的。它可以设置为很多不同的值，指示这个文件以什么**模式**打开。包括读、写和追加。

- 'r'：打开一个文件来**读**数据。这是默认模式，当没有提供第二个参数时，就假设'r'。另外，这个模式假设所读的文件已经存在。
- 'w'：打开一个文件来**写**数据。如果文件中已经包含数据，在继续写之前会清空文件中的数据。
- 'a'：打开一个文件来**追加**数据。保留文件的内容，项文件末尾新增数据（可与'w'比较）。
- 'x'：打开一个**新文件**来写数据。如果文件存在则失败（可与'w'，'a'作比较）。
- 'wb'：写进二进制数据。***"b"代表二进制***
- 'x+b'：读写一个新的二进制文件



 ### 额外换行

  >**输出中额外的换行是怎么来的？文中数据3行，for循环却生成了6行？**

		好了，我们已经知道如何往一个文件里写入数据，现在我们来读取数据吧！

```python
>>> tasks = open('taks.txt')
>>> for chore in tasks:
		print(chore)
... ... 
I love python.

go for a walk.

killen,

>>> tasks.close() 

```

​		***print函数***，它的默认行为是在屏幕上显示的任何内容后面追加一个换行。***文件中***的各行都以一个换行字符结束（二位这个换行符会作为数据行的一部分读取），所有最后会打印两个换行：一个来自文件，另一个来自print。告诉print不要包含第二个换行符，需要把print(chore)改成print(chore, end ='')。这样做就是抑制print追加换行符的默认行为，从而屏幕上不会再出现额外的换行。

```python
>>> tasks = open ('todos.txt')
>>> for chore in tasks:
		print(chore,end ='')
... ... 
I love python.
go for a walk.
killen,
>>> tasks.close()
```



### with语句管理上下文

> **上下文管理协议**

​		with语句符合Python中内置的一个约定编码，这称为**上下文管理协议**。处理文件时如果使用了with，这意味着你完全可以忘记调用close。with语句会管理其代码组运行的上下文，结合使用with和open时，解释器会为你完成收尾的清理工作，在需要时调用close。

```python
>>> with open('todos.txt') as tasks:
... 	for chore in tasks:
... 		print(chore, end = '')
... 
I love python.
go for a walk.
killen,
```



## 小结

- **2020-11-12**
- “读”是“open”函数的默认模式**。**
- Python支持“打开，处理，关闭”技术。不过大多数Python程序员更喜欢使用“with”语句。



> 来源：Head First Python(第二版)书籍