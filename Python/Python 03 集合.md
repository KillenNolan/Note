# Python  4個內置數據結構之集合

## 集合

>集合：无序的唯一对象集合;

```python
#集合时无序的，且其中的任何对象不会重复;
#集合允许完成并集，交集和差集操作;
#集合与列表和字典相似，可根据需要扩展(和收缩);
#集合和字典都是{}包裹的.
>>> vowels = {'a', 'e', 'e', 'i', 'o', 'u', 'u'}

#不重复，且依旧 无序 哦
>>> vowels
{'u', 'e', 'o', 'a', 'i'}

#使用set函数 将任意序列(如一个字符串) 来生成一个集合 
#使用set函数快速创建 元音 集合（）
>>> vowels2 = set ('aeeiouu')
>>> vowels2
{'e', 'i', 'o', 'u', 'a'}

#集合方法的小例子

#并集 union ==>将两个集合合并，再把合并的结果赋给一个新变量 u(这也是一个集合)
    #set(word) 在转换过程中，会删除所有重复的对象
    >>> word = 'hello'#字符串
    >>> u =vowels.union(set(word))
    >>> u
    {'o', 'l', 'h', 'i', 'e', 'a', 'u'}

    #转换成一个列表
    >>> u_list = sorted(list(u))
    >>> u_list
    ['a', 'e', 'h', 'i', 'l', 'o', 'u']
    #我发现不用list,好像也会转换成一个列表，这我就不知道为啥了？ 可能排序的会默认list
    >>> uu=sorted(u)
    >>> uu
    ['a', 'e', 'h', 'i', 'l', 'o', 'u']


#找出不是共有元素 difference
    >>> d = vowels.difference(set(word))
    >>> d
    {'a', 'u', 'i'}
    #d集合由在vowels中，但不在set(word)中的对象组成
    >>> print(sorted(vowels))
    ['a', 'e', 'i', 'o', 'u']
    >>> print(sorted(set(word)))
    ['e', 'h', 'l', 'o']
 
#交集：找出共同元素 intersection
    >>> i = vowels.intersection(set(word))
    >>> i
    {'o', 'e'}
```

## 实例

>一个小问题：找出输入单词中的元音，现在如何用集合来完成？

```python
#这个是我们学习列表的时候写的vowels3.py
>>> vowels = ['a', 'e', 'i', 'o', 'u']
    word = input("Provide a word to search for vowels:") #此处输入你的单词

    found = []	

    for letter in word:
        if letter in vowels:
            if letter not in found:
                found.append(letter)  
    print(found)

#现在用用我们新学习的方法该怎么完成呢？保存为vowels7.py,然后F5运行代码
>>> vowels = ['a', 'e', 'i', 'o', 'u']
    word = input("Provide a word to search for vowels:") #此处输入你的单词
    
    print(vowels.intersection(set(word))) 
    
#由于我之前已经让word='hello'了，而且我是在shell窗口直接运行的，所以结果是{'o', 'e'}
>>> print(vowels.intersection(set(word)))
{'o', 'e'}    

```



## 总结

* Python的集合中不允许有重复
* 与字典类似，集合永大括号包围，不过集合没有键/值对。集合中每个唯一对象之间用一个逗号隔开。
* 同样与字典类似，集合不维持插入顺序（不过可以用sorted函数排序）。
* 可以向set函数传递任何序列，由这个序列中的对象创建一个元素集合（去除所有重复）。
* 集合提供了大量内置功能，包括完成并集、差集和交集的方法。

> 来源：Head First Python(第二版)书籍