# Python  4個內置數據結構之字典

## 字典

>字典：無序的鍵/值對集合

```python

#字典是无序且可变的;可以把字典想象成一个 两列多行 的数据结构;与列表类似,可根据需要扩展(和收缩).
#字典数据是{}包裹的

#创建一个字典，你如果直接(或在文本里写好)copy代码是可以的，但如果想在Python解释器这直接写的话，可能一回车就 ou 啦
>>> person = {
    'Name':'Killen',
    'Gender':'Female',
    'Occupation':'Engineer'
}

#查看刚创建的字典（看到了吗？存储的时候是 无序 的哦）
>>> person
{'Name': 'Killen', 'Occupation': 'Engineer', 'Gender': 'Female'}

#中括号记法查找值 字典使用 键 来访问其关联的 数据值
>>> person['Name']
'Killen'

#运行时处理字典	添加数据
>>> person['Age'] = 25
>>> person
{'Name': 'Killen', 'Age': 25, 'Occupation': 'Engineer', 'Gender': 'Female'}
```

## 实例

>一个小问题：找出输入单词中的元音，并统计其出现的次数？

```python
#这个可以保存一个vowels.py的文档来运行会比较好，因为我直接在Python解释器运行的时候没成功
>>> vowels = ['a', 'e', 'i', 'o', 'u']
    word = input("Provide a word to search for vowels:") #此处输入你的单词

    found = {}
	
    #way one 	to see if letter exists in found, exists: +1; not exists:0
    for letter in word:
        if letter in vowels:
            if letter not in found:
                found[letter] = 0
            found[letter] += 1
  
	#way two    ctrl+/ 可屏蔽代码
    # for letter in word:
    # 	if letter in vowels:
    # 		found.setdefault(letter, 0)
    # 		found[letter] += 1             
            
    print(found) 
    print(sorted(found)) #有序输出
    print(found)

#sorted(found)时，产生了一个found的副本，并不影响found本身
>>> Provide a word to search for vowels:'ministrator' #'ministrator'是我输入的单词
	{'i': 2, 'a': 1, 'o': 1}
    ['a', 'i', 'o']
    {'i': 2, 'a': 1, 'o': 1}
    
#是不是发现有序之后的数据跟没排序的时候不一样呢？排序的时候是按 列表 的形式输出的 键，而没有输出 值
#在vowels.py里追加代码
>>> for k in sorted(found):
	print(k , 'was found', found[k], 'time(s).')
    
    print()

	for k, v in sorted(found.items()):
	print(k , 'was found', v, 'time(s).')    
    
#追加代码的显示效果，按理说应该是 a was found 1 time(s),可能由于我的解释器的原因，显示跟你们可能会有细微差异，不过问题不大
('a', 'was found', 1, 'time(s).')
('i', 'was found', 2, 'time(s).')
('o', 'was found', 1, 'time(s).') 
()
('a', 'was found', 1, 'time(s).')
('i', 'was found', 2, 'time(s).')
('o', 'was found', 1, 'time(s).')  

#to see if letter exists in found, exists: +1; not exists:0
#found.setdefault(letter, 0)等价于 
#if letter not in found:
#    found[letter] = 0	
#这两个其实都是判断found里的 键 是否存在，比较常用，也有那种先初始化字典的，没有判断的，部分代码：
>>> found = {}
    found['a'] = 0
    found['e'] = 0
    found['i'] = 0
    found['o'] = 0
    found['u'] = 0
    for letter in word:
        if letter in vowels:
            found[letter] = found[letter] + 1 #简单写法 found[letter] += 1
#这种情况如果出现未被初始化的 键，就会报错 KeyError         
    
    
```

## 总结

* 默认地，所有字典都是无序的，因为它不会维持插入的顺序。如果需要在输出中对字典进行排序，要使用sorted内置函数。
* items方法允许按行迭代处理一个字典，也就是说，按键/值对迭代处理。一次迭代中，items方法会向for循环返回下一个键和它的关联值。
* 如果试图访问一个字典中不存在的键，会导致一个 KeyError。出现 KeyError 时，程序会由于运行时错误崩溃。
* 访问一个键之前，可以通过确保字典中每个键都有一个关联值来避免KeyError。尽管这里in和not in操作符可以提供帮助，不过更成熟的技术是使用setdefault方法。

> 来源：Head First Python(第二版)书籍