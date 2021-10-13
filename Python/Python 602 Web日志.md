# Python 602 Web日志

## 编写一个Web日志函数

​		对文件处理，你已经有了一些了解，下面来具体运用你所掌握的知识吧！编写一个新函数（依旧写在vsearch4web.py中），名为log_request，他有两个参数：req和res。调用这个函数时，req的参数为当前的Flask请求对象，res参数赋为调用search4letters的结果。把**req**和**res**的值作为一行追加到一个名为vsearch.log的文件。

```python
###写入的不一样格式的log，在你需要显示的时候，可先删掉原先的log，保持格式一致
def log_request(req:'flask_request',res:str)->None:	
	with open('vsearch.log','a') as log:
		print(req.form, req.remote_addr, req.user_agent, res, file = log, sep = '|')
		# print(req.form, file = log, end ='|')
		# print(req.remote_addr, file = log, end = '|')
		# print(req.user_agent, file = log, end = '|')
		# print(res, file = log )		
		# # print(req, res, file = log)  
        
##req里面包含很多信息，但我们现在只需了解form，remote_addr，user_agent      
###form：从Web应用的HTML表单提交的数据
###remote_addr：运行Web浏览器的IP地址
###user_agent：提交数据的浏览器的标识
    
@app.route('/search4', methods=['GET', 'POST'])
def do_search()->'html':
	phrase = request.form['phrase']
	letters = request.form['letters']

	title = 'Here are your results:' 
	results = str(search4letters(phrase, letters))

    ##在这里调用我们写的log_request
	log_request(request,results)

	return render_template('results.html',
							the_title = title,
							the_phrase = phrase,
							the_letters = letters,
							the_results = results )
	# return str(search4letters(phrase,letters))      
    
```

## 读取Web日志

​		我们已经将log写入到vsearch里，现在我们需要在网页上读取它。进阶一点的需要一个HTML模板，名为viewlog.html ，**#是Python的注释写法**。

```python
from flask import Flask, render_template, request, escape

#简易版，原始数据，直接显示，不太好看
@app.route('/viewlog')
def view_the_log()->'html':
	""" look through the log """
	with open('vsearch.log') as log: 
	 	contents = log.read()
	return escape (contents)	
    
#进阶版，用HTML(<Table>)，可用Jinja2模板，嵌入显示逻辑，需HTML文档
@app.route('/viewlog')
def view_the_log()->'html':
	""" look through the log """
	with open('vsearch.log') as log:
		contents = []
		for line in log:
			contents.append([])
			for item in line.split('|'):
				contents[-1].append(escape(item))
	titles = ('Form Data', 'Remote_addr', 'User_agent', 'Results')
	return render_template('viewlog.html',
							the_title = 'View Log',
							the_row_titles = titles,
							the_data = contents, )
	# 	contents = log.read()
	# return escape (contents)	    
```
### escape转义数据

```python
#先从flask模块导入escape(从Jinja2继承来的)函数
>>> from flask import escape

#对一个普通的字符串使用escape，输出没有变化
>>> escape('This is a request')
Markup('This is a request')

#对一些包含特殊字符的字符串使用escape，输出会进行转义。
>>> escape('This is a <request>')
Markup('This is a &lt;request&gt;')

```

### split切割数据

```python
#创建一个关于名字的列表
>>> names = ['Terry', 'John', 'Michael', 'Killen']

#列表的中括号访问法
>>> names[-1]
'Killen'

#使用join技巧
>>> pythons = '|'.join(names)
>>> pythons
'Terry|John|Michael|Killen'

#用给定的分隔符将字符串分解为一个列表
>>> individuals = pythons.split('|')
>>> individuals
['Terry', 'John', 'Michael', 'Killen']
```

### viewlog.html模板

​		简单的HTML<table>的模板，不懂的可简单百度下table的结构。

```html
#viewlog.html 代码
#继承base模板
{% extends 'base.html' %} 
{% block body %}

#取名一般我们是对应的
#the_title 与Python中的the_title = 'View Log'对应
<h2>{{ the_title }}</h2> 
	<table> 
		<tr>
            #这里是Jinja2的for的循环构造，与Python的写法非常相似，代码逻辑一样
            #the_row_titles 与Python中的the_row_titles = titles对应
		 	{% for row_title in the_row_titles %}
				<th> {{row_title}}</th>
			{% endfor %}
		</tr>
        #这里是Jinja2的for的循环构造写法
        #the_data = contents
		{% for log_row in the_data %}
			<tr>
				{% for item in log_row %}
					<td> {{item}} </td> 
				{% endfor %}
			</tr>
		{% endfor %}
	</table>  

{% endblock %}
```

### 图片效果展示

​		第一张图是原始效果展示，是不是不太容易看，第二张效果就好很多了。

![原始效果](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201116101346788.png)

![结果](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201116101119690.png)

**小结**

- **2020-11-16**
- **学了新的escape来转义你的数据**
- **用到了之前学习的数据结构嵌套**



> 来源：Head First Python(第二版)书籍