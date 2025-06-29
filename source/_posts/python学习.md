---
title: python学习
date: 2023-05-19 15:00:17
tags: Python
---



## 模块和包的操作

```python
import random #整个模块
from random import randint #具体函数
import random as rd #别名

"""自定义模块"""
# model.py 内容：
num = 666
def func(): 
    print("函数示例")

# 使用自定义模块
import model
print(model.num) #使用变量
model.func() #调用函数
```

## 异常处理

```python
Traceback (most recent call last): #异常信息的溯源
	File "D:\pythonProject_241\38异常的基本信息.py", line 1, in <module> #具体位置
		print(a) #具体代码内容
NameError: name 'a' is not defined #出现原因
```

```python
# 异常处理
try :
    #可能出错的代码
    print(1)
    print(1/0)
except:
    print("出现异常执行的代码")
else:
    print("没有异常执行的代码")
finally:
    print("无论是否有异常都会执行的代码")
```

### 异常传递

![](https://cdn.jsdelivr.net/gh/SereinCease/images/blog/2025-06-25/image-20250625183953005-84191c.png)

## 文件操作

```python
# 打开文件
"""r:不存在会报错""" 
file1 = open("file.txt", "r", encoding="utf-8")
# 读取所有
# print(file1.read())
# 一行一行读取所有内容，返回list
# print(file1.readlines())
# 只读取一行内容
print(file1.readline())
# 关闭文件防止内存溢出
file1.close()

"""w 存在：覆盖，不存在：新建""" 
with open("file1.txt", "w", encoding="utf-8") as file2:
    # file2.write("bndfgn\hfdsh\scnxcvxb")
    file2.write("1\gsdg")
    
"""a：不会覆盖,追加"""
with open("file1.txt", "a", encoding="utf-8") as file3:
    file3.write("hhhhh\oooo")
    
"""其他文件类型使用二进制方式进行读写 """
with open(r"E:\shu.png","rb") as file4:
    print(file4.read())
```

连续读时返回列表为空，是因为文件指针在第一次读取时指向了文件末尾

```python

```

