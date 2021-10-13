# Python 502 创建一个Web应用

## Web应用观察

```python
#第一步：我在浏览器的地址栏键入了Web地址或单击一个按钮，然后按了回车...
#第二步：Web浏览器将用户的动作转换为一个Web请求，通过 互联网 将它发送到一个服务器。
#第三步：Web服务器接收到这Web请求，必须决定接下来做什么...
	#请求静态内容：很简单，只需从Web服务器的的硬盘读取（静态文件：如html文件、图像...）
    #请求动态内容：会有更多子步骤，比较复杂，通过运行一些代码来生成Web响应（包括：Web服务器运行代码，然后捕获程序输出作为Web响应，再把这个响应发回给正在等待的Web浏览器）
#第四步：Web服务器通过 互联网 将响应发回给正在等待的Web浏览器。
#第五步：Web浏览器接收到Web响应，将它显示在用户的屏幕上。

```

## 我们希望的Web应用

```python
#回忆最新版本的涩archletters函数的def行
#我们的界面

Welcome to search4letters on the Web!

Use this form to submit a search request:
    Phrase:
    Letters:aeiou
When you are ready, click this button:

    	Do it!

#点击按钮后的界面
Here are your results:

You submited the following data:
    
    Phrase:	hitch-hiker
    Letters: aeiou
        
When "hitch-hiker" is searched for "aeiou", the following results are returned:{'e', 'i'}

```

## Web服务器发生了什么？

```python
#用户单击Do it!按钮时
#浏览器将这个数据发送给正在等待的Web服务器，他会抽取phrase和letters的值
#===>代表正在等待的用户调用search4letters函数

#这个函数的结果会作为另一个Web页面返回到用户浏览器
#我假设用户输入的是 hitch-hiker 作为phrase
#保留letters的默认值为 aeiou 

```

## 我们需要做什么？

```python
#除了我们已经掌握的Python知识，要构建一个能实际运行的服务器端Web应用。
#需要了解 Web应用框架 ，它提供了一组通用的基础技术，可以基于这些技术构建你的Web应用。
#我们目前要完成的Web应用，可以用 Flask 框架。

```

### 安装Flask

```python
#Flask是一个第三方框架
#Python社区维护了一个集中管理带三方模块的网站，名为PyPI，可以在 https://pypi.org/ 中找到PyPI
#用pip从命令行安装Flask

	#rl+R===>键入cmd打开Windows的命令提示窗口，作为管理员运行以下命令
	    #y -3 -m pip install flask
        #这个命令会连接到PyPI网站，然后下载和安装Flask模块以及Flask模块依赖的另外四个模块：Werkzeug，MarkupSafe，Jinja2和itsdangerous以及它们的版本
        #安装成功最后一行会看到Successfully installed Jinja2...
        
1.输入python 
2.输入import flask
3.输入flask.__version__

    
```

### Flask如何工作

​		Flask提供了一组模块，可以帮助你构建服务器端Web应用。这是一个微Web框架，他只提供了完成这个任务所需的最基本的一组技术。

Flask框架没有其他框架那么全面，如Django（这是所有Python Web框架之母），不过Flask是轻量级的，规模很小而且易于使用（根据你的需求而定）。

```python
#新建一个hello_flak.py文件，保存在webapp文件夹下（看你自己意愿），键入以下代码
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello()->str:
    return 'Hello world from Flask'

app.run()

#从你的操作系统命令行运行Flask
#不要在你的IDLE中运行，IDLE非常适合运行试验小的代码
#运行应用的，最好在操作系统的命令窗口直接通过解释器运行代码

#进入到webapp目录运行 py -3 hello_flask.py
#出现这一句 Rnning on http://127.0.0.1:5000/ (Press CTRL+C to quit) 表示一切正常
    #端口协议内容可以访问网址 https://en.wikipedia.org/wiki/Port_(computer_networking) 来了解
    #Web地址===>127.0.0.1===>也就是localhost（我的计算机）
    #协议端口===>5000
#把这个输入到浏览器中===>http://127.0.0.1:5000/
```

![image-20201110095503066](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110095503066.png)

![访问页面](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110095558330.png)

![image-20201110095701122](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110095701122.png)

![image-20201110110803085](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110110803085.png)

### 发生了什么？

```python
#上图从Flask Web服务器返回了一些消息
#导入flask模块的Flask类
from flask import Flask
	# flask ===> 这是模块的名字
    # Flask ===> 这是类名
    
#创建Flask对象的一个实例，并把它赋给 app
app = Flask(__name__)
	# __name__ 的值由Python解释器维护
	    #扩展："双下划线，name，双下划线" 别名dunder name，这只是一个简称，表示同一个东西；用了上下划线，统称dunders
        #扩展：使用单下划线字符作为某些变量名的前缀，例如 _number
        
#通过app变量使用Flask的route修饰符   
@app.route('/')
    #route函数修饰符 ===>它有一个@符号前缀
    	#修饰符不仅可以用来修饰函数，也可以用于类
    #('/') ===> URL
    
#普通的Python函数，调用时返回一个字符串（'->str':注解）
def hello()->str:
    return 'Hello world from Flask'

#开始运行
app.run()
```

## 为Web提供功能

​		修改hello_flask.py来包含第二个url:/search4。编写代码将这个URL与一个名为do_search的函数关联。它会（从我们的vsearch模块，之前我们自己写好安装的模块）调用search4letters函数。

```python
from flask import Flask

from vsearch import search4letters

app = Flask(__name__)

@app.route('/')
def hello()->str:
    return 'Hello world from Flask'

@app.route('/search4')
def do_search()->str:
    return str(serach4letters('life, the universe, and wverything!', 'eiru,!'))

app.run()
```

​		Ctrl+C来停止Web应用，然后重启运行，在浏览器中键入url。

![image-20201110111754001](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110111754001.png)

​		

> 来源：Head First Python(第二版)书籍

