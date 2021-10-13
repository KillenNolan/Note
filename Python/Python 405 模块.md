# Python 402 模块

## 模块

> 模块就是包含函数的文件；下面的代码是一个vsearch.py的文件，也可以称作 模块

```python
#使用关键字 def 定义一个函数（方法）
def search4vowels(word:str) -> set:
	"""Display any vowels found in an asked-for word."""
	vowels = set('aeiou')  	
	return vowels.intersection(set(word))

def search4letters(phrase:str, letters:str='aeiou') -> set:
	""" Return a set of the 'letters' found in 'phrase'."""
	return set(letters).intersection(set(phrase))
```

## import 模块

>如何找到模块

```python
当你想要导入模块时，会先从三个主要位置搜索模块
	1.你的当前工作目录
    2.你的解释器的site-packages位置
    3.标准库的位置
```

## 安装模块到site-packages

```python
site-packages的位置：包含你已经安装的第三方Python模块（也包括你自己写的模块）
```

### 使用setuptools将模块安装到site-packages

```python
#在Python 3.4版本中，标准库包含一个名为setuptools的模块，可用来在 site-packages 中增加任何模块。
#这个工作setuptools字需要3步完成
	1.创建一个发布描述
    	需要创建两个描述文件：setup.py和README.txt
    2.生成一个发布文件
    	创建一个发布文件：py -3 setup.py sdist
    3.安装发布文件
    	py -3 - m pip install vsearch-1.0.tar.gz

```

#### 	setp1:创建一个setup.py和一个空的RERADME.txt 

```python
from setuptools import setup
setup(
	name = 'vsearch',
	version = '1.0',
	description = 'The Head Firsst Python Search Tools',
	author = 'Killen',
	author_email = 'hfpy2e@gmail.com',
	url = 'headfirstlabs.com',
	py_modules = ['vsearch']
)
```

#### 	step2:生成发布文件

```python
#指定或新建一个目录（我的是mymodules），以后用来存放你写的 模块、第三方Python文档。
#将第一步的两个文件移至此目录下，且保证此电脑上只有此唯一版本（此电脑其他地方不要有）。
#图一 进入你的指定目录，在此使用命令生成发布文件。
#图二 是你命令执行后，目录下产生的目录或文件，注意dist
#mymodules的三个py文件已经合并到 源发布文件（vsearch-1.0.tar.gz）
#注意图一倒数第三行"and everything under it"说明成功，否则就是失败了。
#py -3(在windows上运行Python3) setup.py(执行其中的代码) dist(并传递dist作为参数)

```

![image-20201106083331948](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201106083331948.png)

![生成的文件](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201106084159963.png)

#### step3:安装发布文件

```python
#图一 进入dist目录，查看dist下产生的一个名为 vsearch-1.0.tar.gz 的文件
#使用pip命令安装这个文件到site-packages
#提示"Successfully installed vsearch-1.0"表示成功啦
```



![查看目录](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201106084713095.png)



![安装](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201106085637591.png)

## 共享你的代码

```python
#共享分为正式和非正式
#非正式：你发给别人就好了（email，u盘，个人网站啦）
#正式：PyPI（拼作"pie-pee-eye"，这是Python Package Index的缩写）
##了解这个网站提供了那些模块，访问https://pypi.org/ 的help
```

![image-20201107102835814](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201107102835814.png)

## 总结

* 模块就是将一个或多个函数保存在文件中。

* 通过确保模块总在解释器的当前工作目录（这是可能的，但比较困难）或者在解释器的site-packages位置上（到目前为止，这是更好的选择），可以共享一个模块。

* 按照setuptools的3个步骤可以确保将模块安装到site-packages，这将允许你导入模块并使用模块中的函数，而不论当前工作目录是什么。

  

> 来源：Head First Python(第二版)书籍