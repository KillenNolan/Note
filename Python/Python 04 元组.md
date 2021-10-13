# Python  4個內置數據結構之元组

## 元组

>集合：有序的不可变对象集合

```python
#元组与列表类似，只不过一旦创建就不能改变（元组是一个不可变的列表）。
#type内置函数来确认所创建的各个对象的类型
    >>> vowels = ['a', 'e', 'i', 'o', 'u']
    >>> type(vowels)
    <class 'list'>

    >>> vowels2 =  ('a', 'e', 'i', 'o', 'u') 
    >>> type(vowels2)
    <class 'tuple'>

    >>> vowels
    ['a', 'e', 'i', 'o', 'u']
    >>> vowels2
    ('a', 'e', 'i', 'o', 'u')

#元组是不可变的，列表是可变的
    >>> vowels[2] = 'I'
    >>> vowels
    ['a', 'e', 'I', 'o', 'u']

    #修改元组的时候是会报错的
    >>> vowels2[2] = 'I'
    Traceback (most recent call last):
      File "<console>", line 1, in <module>
    TypeError: 'tuple' object does not support item assignment
    >>> vowels2
    ('a', 'e', 'i', 'o', 'u')

#只有一个对象的元组
    #每个元组的括号之间至少要包含一个逗号
    #没有逗号的时候我们得到了一个字符串
    >>> t = ('python') 
    >>> type(t)
    <class 'str'>

    >>> t = ('python',)
    >>> type(t)
    <class 'tuple'>

```

## 总结

* 元组与列表类似，只不过一旦创建就不能改变（元组是一个不可变的列表）。

* 元组用小括号，列表用大括号。

* 可用内置函数type来查看对象的类型。

* 当你的数据从不需改变，就可放在元组中。

* 每个元组的括号之间至少要包含一个逗号，('python',)。

  

> 来源：Head First Python(第二版)书籍