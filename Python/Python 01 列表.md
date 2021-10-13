# Python 02  4個內置數據結構之列表 (list)

## 前提:安裝好Python的運行環境



## 模塊OS

```python
import os //導入模塊os,這個模塊提供了一種平台獨立的方式來與底層操作系統交互
```

```python
from os import hetcwd //導入模塊os的指定函數hetcwd,hetcwd:返回你的當前工作目錄
```



## 列表:有序的可變對象集合

> 在Python中的列表是動態的,可根據需要擴展(收縮),不需要預聲明列表的大小;
> 								是異構的,可以在一個列表中混合不同類型的數據,不需要預聲明所要存儲的隊形類型	

### 列表方法:會改變一個列表的狀態

append();insert(0,'5');extend([1,2]);remove('5');pop(); 

```python
word = "I Love Python"
plist = list(word) //將字符串轉換成一個列表
print(plist) //['I', ' ', 'L', 'o', 'v', 'e', ' ', 'P', 'y', 't', 'h', 'o', 'n']

plist.append('54') //在plist后追加54
print(plist) //['I', ' ', 'L', 'o', 'v', 'e', ' ', 'P', 'y', 't', 'h', 'o', 'n', '54']

plist.append('520') //在plist后追加520
print(plist) //['I', ' ', 'L', 'o', 'v', 'e', ' ', 'P', 'y', 't', 'h', 'o', 'n', '54', '520']

plist.pop() //將plist最後一個彈出,相當於刪除 54
print(plist) //['I', ' ', 'L', 'o', 'v', 'e', ' ', 'P', 'y', 't', 'h', 'o', 'n', '54']

plist.insert(0,[0,2]) //在指定位置的前面添加值[0,2]
print(plist) //[[0, 2], 'I', ' ', 'L', 'o', 'v', 'e', ' ', 'P', 'y', 't', 'h', 'o', 'n', '54']

plist.insert(0,55) //在指定位置的前面添加值55
print(plist) //[55, [0, 2], 'I', ' ', 'L', 'o', 'v', 'e', ' ', 'P', 'y', 't', 'h', 'o', 'n', '54']

plist.pop(0) //將plist指定位置的值彈出,相當於刪除 55
print(plist) //[[0, 2], 'I', ' ', 'L', 'o', 'v', 'e', ' ', 'P', 'y', 't', 'h', 'o', 'n', '54']

plist.extend([11])  //在plist后追加元素,可單個,可多個 11
plist.extend([12,11]) //在plist后追加元素,可單個,可多個 12, 11
print(plist) //[[0, 2], 'I', ' ', 'L', 'o', 'v', 'e', ' ', 'P', 'y', 't', 'h', 'o', 'n', '54', 11, 12, 11 ]

plist.remove(11) //刪除plist第一個找到的11
plist.remove('I')
print(plist) //[[0, 2], ' ', 'L', 'o', 'v', 'e', ' ', 'P', 'y', 't', 'h', 'o', 'n', '54', 12, 11 ]
```



### 中括號記法:(通常)不會改變列表的狀態

```python
word = "Hello" //字符串 下標+:0,1,2,3,4 下標-:-1,-2,-3,-4,-5
word[0] //第一個字母'H'
word[-1] //最後一個字母'o'
print(word) // 'Hello'
 

letters = list(word)//將 字符串 轉換成一個 列表
letters[0] //第一個字母'H'
letters[-1] //最後一個字母'o'
print(letters) // ['H', 'e', 'l', 'l', 'o']
```



### 切片:(通常)不會改變列表的狀態(多寫代碼,多觀察,多看效果,解釋好難)

```python
//**word[start,stop,step]** start:default 0; stop: default len(word); step:default 1
word = "Panic" //下標+:0,1,2,3,4 下標-:-1,-2,-3,-4,-5
len(word) //字符串 長度 5

//可轉word為list,用法也類似
word[0:5:1] //'Panic' 從下標 0 開始,不包含下標 5,間隔為1(自左向右)
word[0:5:2] //'Pnc'	從下標 0 開始,不包含下標 5,間隔為2
word[1:5:2] //'ai'	從下標 1 開始,不包含下標 5,間隔為2

word[::-1] //'cinaP'
word[4:1:-1] //'cin' 從下標 4 開始(也就是'c'),下標 1(不包含下標 0,1;也就是說去掉 'Pa'),間隔為-1;最後就是抓取下標為 4;4-1=3;4-1-1=2的值 'cin'
```



> 来源：Head First Python(第二版)书籍