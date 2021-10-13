# Python 503 构建HTML表单

## 创建html，然后发送至浏览器

​		我们需要的html表单并不复杂。除了文字描述，这个表单还包括两个输入框和一个按钮。从我们的Flask Web应用创建html文本，有两种不同选择。

1. 放在大字符串里

2. 使用模板引擎，便于重用

​		利用模板引擎，程序员可以应用面向对象的继承和重用概念来生成文本数据。网站的外观可以在一个顶层html模板中定义，这成为**基模板**，然后其他html页面继承这个模板。如果修改基模板，这个修改也会在继承该模板的所有html页面中展示。

基模板可以在以下网址中下载 ===> **模板保存在templates文件夹中，.css文件保存在static文件夹中，最好建在webapp目录下**

***[下载地址](http://python.itcarlow.ie/ed2/)***

![代码](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110112559941.png)



```html
//基模板，保存为 base.html 保存在templates文件夹中

<!DOCTYPE html>

<html  >
    <head>
        <title>{{the_title}}</title>
        <link rel="stylesheet" href="static/hf.css" />
    </head>
    <body>
        {% block body %}
        {% endblock %} 
    </body>

</html>

// <!DOCTYPE html> ===> html5的标志
// {{the_title}} ===> 这是Jinja2指令，指令将在呈现之前提供的一个值（可以把它想象成是 模板 的一个 参数 ）
// static/hf.css ===> 这个文件定义了所有Web页面的外观（样式）
// {% block body %}  {% endblock %}  ===> 这些Jinja2指令指示要在呈现之前替换这里的 HTML块，这要有 继承这个模板的页面来提供。 

```

```html
//有了基模板之后，就可以使用Jinja的extends指令继承这个基模板（base.html）
// **其实我是觉得基模板应该可以不止一个，但一个的话，是比较好维护的**
//第一个调用search4letters函数的网页页面 entry.html 保存在templates文件夹中


{% extends 'base.html' %}
{% block body %}

<h2>{{ the_title }}</h2>
<form method="POST" action="/search4">
	<table>
		<p>Use this form to submit a search request:</p>
		<tr>
			<td>Phrase:</td>
			<td><input type="text" name="phrase" width="60"></td>
		</tr>
		<tr>
			<td>Letters:</td>
			<td><input type="text" name="letters" value="aeiou"></td>
		</tr>
	</table>
	<p>when you're ready, click this button:</p>
	<p><input type="submit" value="Do it!"></p>	
</form>

{% endblock %}


//{% extends 'base.html' %} {% block body %}...{% endblock %}===> 这个模板继承了基模板，为基模板名为"body"的块提供了一个替代块

```

```html
//这个界面也用extends指令继承了这个基模板（base.html）
//返回第一个页面调用函数的结果 results.html 保存在templates文件夹中


{% extends 'base.html' %}
{% block body %}

<h2>{{the_title}}</h2> 
	<table>
		<p>You submited the following data:</p>
		<tr>
			<td>Phrase:</td>
			<td>{{the_phrase}}</td>
		</tr>
		<tr>
			<td>Letters:</td>
			<td>{{the_letters}}</td>
		</tr>
	</table>
	<p>when '{{the_phrase}}' is search for the '{{the_letters}}', the following results are returned:</p>
	<h2>{{the_results}}</h2> 

{% endblock %}


// 这些额外参数{{the_phrase}}、{{the_letters}}、{{the_results}}，在呈现之前需为他们提供值
```

## 从Flask呈现模板

​		Flask提供了一个名为render_template的函数，如果指定一个模板名和所需的参数，调用这个函数时会返回一个HTML串。为了使用render_template，要把这个函数名增加到从flask模块导入的函数列表中（在代码最上面），然后根据需要调用这个函数。

​		为了呈现entry.html模板中的HTML表单，需要

```python
#1.导入render_template函数
#2.创建一个新的url，这里就是 /entry
#3.创建一个函数entry_page返回正确呈现的HTML
#4.重新保存文件为vsearch4web.py，修改完后，你的目录如下图所示
from flask import Flask, render_template

from vsearch import search4letters

app = Flask(__name__)

@app.route('/')
def hello()->str:
    return 'Hello world from Flask'

@app.route('/search4')
def do_search()->str:
    return str(serach4letters('life, the universe, and wverything!', 'eiru,!'))


@app.route('/entry')
def entry_page()->'html':
	return render_template('entry.html', the_title = 'Weclome to search4letters on the web!')

app.run()

#按之前步骤运行vsearch4web.py，点击Do it!
#问题：点击时报错，没有呈现出results.html页面
	#405状态码指示客户端（你的浏览器）使用了这个服务器不允许的一个HTTP方法来发送请求。
    #暂时知道GET和POST就好。
    
    

```

![image-20201110143215911](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110143215911.png)

![image-20201110144302789](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110144302789.png)

补充知识点：HTTP状态码响应，主要分为五类

1. 100~199：**信息消息**
2. 200~299：成功消息：服务器已经接收、理解和处理客户端的请求，一切正常。
3. 300~399：**重定向**消息，服务器通知客户端请求可以在别处处理。
4. 400~499：**客户端错误**：服务器从客户端接收到一个它不理解也无法处理的请求。通常这是客户端的问题。
5. 500~599：**服务器错误**：服务器从客户端接收到一个请求，但服务器尝试处理这个请求时失败了。通常这是服务器的问题。

## 处理提交的数据

​		除了接收url作为第一个参数，@app.route修饰符还接收其他可选的参数。

其中之一是methods参数，这会列出url支持的HTTP方法。默认地，Flask对所有方法都支持GET方法。你可以覆盖这个默认行为。

```python
#继续修改代码
from flask import Flask, render_template

from vsearch import search4letters

app = Flask(__name__)

@app.route('/')
def hello()->str:
    return 'Hello world from Flask'

#注意啦，就是这里，而且注意是 methods ，但是在HTML文件中是 method 
@app.route('/search4', methods=['GET', 'POST'])
def do_search()->str:
    return str(serach4letters('life, the universe, and wverything!', 'eiru,!'))


@app.route('/entry')
def entry_page()->'html':
	return render_template('entry.html', the_title = 'Weclome to search4letters on the web!')

# 开启调试模式，如果代码改变后保存，Web应用会自动重启
#  * Detected change in 'D:\\KILLEN\\LEARN\\Example\\Python\\webapp\\vsearch4web.py', reloading
#  * Restarting with stat
	app.run(debug=True)
    
#执行效果，无论你输入什么，返回的结果都是一样的，为什么呢？
#因为我们的HTML表单将数据提交到Web服务器，但想要对这些数据做某些处理，我们需要次修改Web应用的代码来接收这个数据。
#Flask提供了一个内置对象，名为request，利用这个对象可以很容易地访问所提交的数据
#request对象包含一个名为form的字典属性，可以访问从浏览器提交的HTML表单数据。

#1.将request增加到导入列表
#2.在do_search中创建两个新变量，并把HTML表单的数据赋至新创建的变量，然后在search4letters调用中使用这些变量。
from flask import Flask, render_template, request
.
.
.
#用的是html元素的name
@app.route('/search4', methods=['GET', 'POST'])
def do_search()->str:
    phrase = request.form['phrase']
	letters = request.form['letters']
    return str(search4letters(phrase,letters))

```

![image-20201110153456148](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110153456148.png)

## 结果生成为HTML

​		继续修改do_search函数代码，为results.html的变量赋值，让其结果在results.html显示，如图。

```python
@app.route('/search4', methods=['GET', 'POST'])
def do_search()->str:
    phrase = request.form['phrase']
	letters = request.form['letters']
    
	title = 'Here are your results:' 
	results = str(search4letters(phrase, letters))
    
	return render_template('results.html',
							the_title = title,
							the_phrase = phrase,
							the_letters = letters,
							the_results = results, )#最后这个小逗号可有可无，不会报错，我一般不写
```

![image-20201110154650295](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110154650295.png)

## 重定向来避免不想要的错误

​		使用Flask的重定向技术，需要在from flask导入行上增加rediect(在代码最前面)，然后修改hello函数，将实际的请求替换为 /entry。

```python
from flask import Flask, render_template, request, rediect 

@app.route('/')
def hello()->'302':
    return rediect('/entry')

#'302'是调用"rediect"时Flask向浏览器发回的状态
#rediect('/entry')===> 替换请求
```

但是使用rediect，每次指向'/'的请求都会变成两个请求'/','/entry'，有些浪费。实际上Flask可以为一个给定的函数**关联多个url**，这样就减少对重定向的需要。如果一个函数有多个关联的url，url会尝试依次与各个url匹配，如果找到一个匹配，就会执行这个函数。

```python
#删掉rediect
#删掉hello函数，和 @app.route('/')
#@app.route('/')加入到

@app.route('/')
@app.route('/entry')
def entry_page()->'html':
	return render_template('entry.html', the_title = 'Weclome to search4letters on the web!')
 
```

## 准备把你的Web应用部署到云

​		这个Web应用已经可以在你的的本地运行，现在考虑部署这个应用，让更多的人都能使用。这有很多选择，有很多不同的基于Web的托管服务。有一个流行的服务名为***PythonAnywhere***，这个服务基于云，在AWS上托管。

​		但是，和所有的云托管的部署解决方案一样，***PythonAnywhere***喜欢控制Web应用的启动。这意味着***PythonAnywhere***认为应该由它负责调用app.run()，这也说明不需要在你的代码中调用app.run()。实际上，如果你试图执行这行代码，***PythonAnywhere***会拒绝运行你的Web应用。

新建一个dunder.py

```python
print('We start off in:', __name__)
if __name__ == '__main__':
	print('And end up in:', __name__)
#print('We start off in:', __name__) ===> 打印当前的活动命名空间，这结果存储在__name__变量中。
#if __name__ == '__main__': ===> 查看__name__的值是否设置为__main__，如果是，则显示另一个消息，确认__name__的值



#由Python执行
python dunder.py

#只显示了一行，因为__name__的值设置为"dunder"(这是所导入的模块的名字)
>>>import dunder
```

​		这里要了解一点：如果你的程序代码直接由Python执行，dunder.py中的if语句会返回True，因为活动命名空间是_ _ main_ __ 。不过，如果你的程序作为一个模块导入(就像Python Shell提示窗口示例那样)，if语句总是返回False，因为_ __ _name_ _  _ _的值不是_ _ _main _ _，而是所导入的模块的名字(在这里就是dunder)。

![image-20201110170355755](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201110170355755.png)

```python
#修改你的代码
if __name__ == '__main__':	
	# 开启调试模式
	app.run(debug=True)
```

```python
#我的最终版本代码

from flask import Flask, render_template, request 
from vsearch import search4letters

app = Flask(__name__)
 

@app.route('/search4', methods=['GET', 'POST'])
def do_search()->'html':
	phrase = request.form['phrase']
	letters = request.form['letters']

	title = 'Here are your results:' 
	results = str(search4letters(phrase, letters))
	return render_template('results.html',
							the_title = title,
							the_phrase = phrase,
							the_letters = letters,
							the_results = results )
	# return str(search4letters(phrase,letters))
    
@app.route('/')
@app.route('/entry')
def entry_page()->'html':
	return render_template('entry.html', the_title = 'Weclome to search4letters on the web!')


# 加入if原因：云托管时他会自动执行app.run()，而拒绝执行本地的app.run()，导致报错
# 不是部署时，执行app.run（）;
if __name__ == '__main__':	
	# 开启调试模式
	app.run(debug=True)
```

## 小结

* 你已经了解Python Package Index(**PyPI**)，这是集中管理第三方Python模块的一个存储库。链接互联网时，可以使用pip自动安装包。

* 使用pip安装**Flask**微Web框架，然后用它来构建你的Web应用。

* __ __name__ __值(由解释器维护)指出了当前活动的命名空间(后面会更详细地介绍有关内容)。

* 函数名前面的@符号标识这是一个修饰符。利用修饰符，可以改变一个已有函数的行为而无需修改这个函数的代码。在你的Web应用中，使用了Flask的@app.route修饰符为Python函数关联url。一个函数可以修饰多次（如do_search函数就关联了多个url）。

* 已经了解如何使用**Jinja2**文本模板引擎在Web应用中呈现HTML页面。

  

> 来源：Head First Python(第二版)书籍

