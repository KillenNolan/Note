# Python 401 函数

## 函数

> 可重用的代码块。

```python
#函数的引入了两个新关键字：def，return。
#函数可以接收参数数据。
#函数包含代码，通常还有文档。
#函数模板
def a_descriptive_name(optional_arguments):
    """A documentation string."""
    '''A documentation string.'''
    #Your function's code goes here 
    #Your function's code goes here
    return optional_value
#"def"指定函数名，并列出参数
#"docsting"描述这个函数的用途，有两种方式：''' ''',""" """
```

## 实例

```python
#之前的例子
vowels = ['a', 'e', 'i', 'o', 'u']
word = input("Provide a word to search for vowels:")
found = [] 
for letter in word:
	if letter in vowels: 
		found = vowels.intersection(set(word))
        for vowel in found:
            print(vowel)
            
#现在可以用个方法来写，还可以多次复用，保存为vsearch.py
    #写入效果见图片
    def search4vowels():
        """Display any vowels found in an asked-for word."""        
        vowels = set('aeiou')  	
        word = input("Provide a word to search for vowels:")
        found = vowels.intersection(set(word))
        for vowel in found:
            print(vowel) 

    #代码写入成功以下是我测试调用的结果
    #第一个结果说明 方法（函数）创建成功
    >>> search4vowels
    <function search4vowels at 0x000001E91441F4C0>

    #调用
    >>> search4vowels()   

```

### 图片

```python
#tab和空格尽量不要混用，那个破折号一样的是tab效果，那个点就是空格。
#可在你的代码编写器中将tab键设置为4个空格，这样把代码copy到Python Shell中
#代码末尾有省略号一样的一般表示代码在这 创建成果 可调用
```

![查找元音例子](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201107084527607.png)

## 函数有返回

```python
#Python有个内置函数bool
#会返回True,False（注意首字母大写哦~）
#如果计算为0、值为None、空串或一个空的内置数据结构，则为False，那所有的其他对象计算则为True

#for example 返回一个值
#False:见图，基本为 False 的就前面几种，其他的都是True，自己多敲敲

#for example 返回一个值
#以search4vowels为例，我们来修改它，当然，你要重写一个py我也是不介意的
	def search4vowels(word):
        """Display any vowels found in an asked-for word."""
        vowels = set('aeiou')  	
        found = vowels.intersection(set(word))
        return bool(found)
    
#结果：见图啊

#for example 返回多个值
#多个值：把多个值打包在一个数据结构中，返回这个数据结构
#以search4vowels为例，我们再来修改它
	def search4vowels(word):
        """Display any vowels found in an asked-for word."""
        vowels = set('aeiou')  	
        found = vowels.intersection(set(word))
        return found
    
#结果：见图啊

#注意：set()就是解释器的表示啦
##再给你们放张图，瞧瞧看看啦
##空列表、空字典、空集合、空元组

```

### 图片

![image-20201107101017958](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201107101017958.png)

![image-20201107101957923](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201107101957923.png)

## 小结

* 函数是命名的代码块。
* def关键字用来命名函数，函数代码在def关键字下（相对于def关键字）缩进。
* Python的三重引号字符串可以用来为函数增加多行注释。如果采用这种方式，他们成为docstring。
* return语句允许函数返回任意多个值（也可以不返回任何值）。

## 函数有参数

```python

#现在我们继续往vsearch.py中写入代码，简化之前的search4vowels方法

	#函数注解：1.可选的	2.可提供信息，不会有其他任何行为（如类型检查）
    #word:str==>希望word参数是一个字符串
    #->set===>指出函数向其调用者返回一个集合  

    #这个是简化之前的search4vowels，参数直接调用时写入
	def search4vowels(word:str)->set:
        """Display any vowels found in an asked-for word."""
        vowels = set('aeiou')  	
        return vowels.intersection(set(word))
    
    #代码写入成功以下是我测试调用的结果
    #第一个结果说明 方法（函数）创建成功
    >>> search4vowels
    <function search4vowels at 0x000001E91450C9D0>

    #调用参数不对，报错TypeError
    >>> search4vowels()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: search4vowels() missing 1 required positional argument: 	   'word'

    #正确调用 需要给 word 传一个值
    >>> search4vowels('python')
    {'o'}    

#前面写的这个方法，只能用来查找元音，那怎样可以不止查找别的什么吗？
#phrase===>我们之前的word
#letters==>希望查找的字符
#默认参数写最后
#letters:str='aeiou'===>这里指明如果没有指定这参数的话，就默认查找元音，这种默认设定很有用，如果之前有人调用了search4vowels，现在他替换search4letters时就不用改写参数了，只需替换方法名
>>>def search4letters(phrase:str, letters:str='aeiou') -> set:
        """ Return a set of the 'letters' found in 'phrase'."""
        return set(letters).intersection(set(phrase))
    
#调用方式也可带参数调用，这样顺序是可以打乱的，但默认参数一定要放到最后。

#查看函数注解
#help(search4letters)
```

### 图片

![image-20201106161714700](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201106161714700.png)

![image-20201107091423860](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201107091423860.png)

![查看函数注解](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201107093510466.png)

## 小结

* 函数可以接收任意多个命名参数，也可以没有参数。
* 函数注解可以用来描述函数参数的类型以及函数的返回类型。
* 除了支持代码重用，函数还可以隐藏复杂性。如果你有一行复杂的代码，想要大量使用，可以把它抽象成一个简单的函数调用
* 任何函数参数都可以在函数的def行赋一个默认值。如果为一个函数设置了默认值，调用函数时，为这个参数指定值就是可选的。
* 除了按位置赋参数，还可以使用关键字。使用关键字时，任何顺序都是可以的（因为使用关键字可以除去任何二义性，位置不再重要）。

> 来源：Head First Python(第二版)书籍

