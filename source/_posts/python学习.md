---
title: python基础
date: 2023-05-19 15:00:17
tags: Python
---

安装-day1

# Python基础语法及数据类型常用的方法（day2）

## 1. 注释的基本使用

**注释的含义：**  
- 注释的作用是让代码更加具备可读性，对代码进行解释说明的文字  
- 如果想让代码不参与程序的内存，那么可以将代码进行注释  
- 被注释的代码和解释性说明的文字信息都不会参与程序的运行  

**注释的使用：**  
- **单行注释**  
  - 使用符号：`# 注释的相关描述文字信息`  
  - **快捷键**: `Ctrl + /`  
- **多行注释**  
  - 使用符号：`""" 被多行注释的相关内容 """`  
  - 没有快捷键  
  - 使用多行注释的引号，可以是单引号也可以是双引号，前后都是3个单/双引号即可  

**代码演示：**  
```python
# 单行注释：输出666  
print(666)  

# 单行注释：输出888  
print(888)  

# 不想让代码参与程序的执行，那么可以将代码注释掉  
# print(999)  # 快捷键: Ctrl + /  

# 使用多行注释  
"""  
print(1)  
print(1)  
"""  

# 使用3个单引号  
'''  
print(2)  
print(2)  
print(2)  
'''  

# 一般多行注释比较少用，因为单行注释可以起到多行注释的效果  
#  
# print(3)  
# print(3)  
# print(3)  
# print(3)  
```

**注意点：**  
- 多行注释的符号不管是单引号还是双引号，全部要使用英文状态下的符号  
- Python中所有需要使用的特殊符号都是英文状态下，不能使用中文符号  

---

## 2. 变量的定义和使用

### 变量的概念：  
- 变量就是用来存储数据的  
- 变量的作用就是用来储存不同类型的数据  
- 变量的值可以一直变化或者被覆盖  

### 变量的基本定义和使用代码演示：  
```python
# 变量需要取名字：变量名  
# 变量是通过等于号进行赋值，把等号右边的实际值赋值给等号左边变量名  
num = 666  
num2 = 888  

# 对变量进行输出，输出对应的变量名中储存的实际值  
print(num)  # 666  
print(num2) # 888  

# 变量是可以重复赋值，以及覆盖  
print("-" * 100)  
num = 777  
print(num)  # 777  

# 注意点：在使用变量之前的最后一次赋值为变量的实际值  
num = 999  
print(num2) # 888  
```

### 变量的数据类型：  
```python
# 如何查看变量的数据类型：使用type内建函数进行查看  
# 内建函数：实现某种特定功能就叫做函数，Python自带的函数叫做内建函数  
# print(), type()都是属于内建函数具有某种功能  

# 整数类型  
num = 100  
# 浮点数类型  
num2 = 3.14  
print(type(num))   # <class 'int'> 代表整数类型  
print(type(num2))  # <class 'float'> 代表浮点数类型（小数类型）  

# 布尔类型  
# 是两个关键字分别是：True默认代表数字：1,和False默认代表数字：0  
# 关键字：Python中已经取好的变量名，就叫做关键字，也具备某种特殊的含义  
print(True)  
print(False)  
print(type(True))   # <class 'bool'>  
print(type(False))  # <class 'bool'>  
print(True + True)   # 2  
print(True + False)  # 1  
print(False + True)  # 1  
print(False + False) # 0  

# 列表：[]  
# 列表中可以存放任何的数据类型  
list1 = [11, 12.34, False, True]  
print(len(list1))  # 4  
print(list1)  
print(type(list1))  # <class 'list'>  

# 元组：()  
t1 = (1, 2, 3, 4)  
print(len(t1))  # 4  
print(t1)  
print(type(t1))  # <class 'tuple'>  

# 元组()默认可以不写，如果数据与数据之间隔开，那么就是元组  
t2 = 6, 7, 8  
print(t2)  
print(type(t2))  # <class 'tuple'>  

# 字典: {键:值}, 可以同时有多个键值对  
dict1 = {"name": "胡歌"}  
print(dict1)  
print(type(dict1))  # <class 'dict'>  

dict2 = {"name": "胡歌", "name2": "彭子晏", "name3": "阮经天"}  
print(len(dict2))  # 3:3组键值对  
print(dict2)  
print(type(dict2))  # <class 'dict'>  

# 集合: {数据1,数据2}  
# 特性: 自动去重，集合中没有重复的数据  
set1 = {1, 2, 3}  
print(set1)  
print(type(set1))  # <class 'set'>  

set2 = {1, 1, 2, 3, 12, 1, 2, 1, 2, 1, 2, 3, 4, 2, 1}  
print(set2)  # {1, 2, 3, 4, 12}  
print(len(set1))  # 3  
print(len(set2))  # 5  

# 字符串: '字符',"字符","""字符"""  
# 字符串含义: 用引号引起来的任何一串字符就叫做字符串  
name = '胡歌'  
name2 = "彭子晏"  
name3 = """阮经天"""  # 在定义变量的时候三个单/双引号表示字符串，不是多行注释  
num4 = "120"  
num5 = '120'  
num6 = '''120'''  

print(len(name))    # 2  
print(len(name2))   # 3  
print(len(name3))   # 3  
print(len(num4))    # 3  
print(type(name))   # <class 'str'>  
print(type(name2))  # <class 'str'>  
print(type(name3))  # <class 'str'>  
print(type(num4))   # <class 'str'>  
```

**总结：**  
* Python中常见的数据类型包括:  
  ○ 整数类型: int  
  ○ 浮点数类型: float  
  ○ 布尔类型: bool  
  ○ 列表: list  
  ○ 元组: tuple  
  ○ 字典: dict  
  ○ 字符串: str  
  ○ 集合: set  

**分类：**  
- **数字类型的数据**：整数类型，浮点数类型，布尔类型  
- **非数字类型的数据**：字符串，列表，元组，字典，集合  
- **容器类型数据**：字符串，列表，元组，字典，集合（容器类型里面的数据每个值叫做元素）  

**内建函数：**  
- `print()`：输出功能  
- `type()`：查看变量的数据类型功能  
- `len()`：查看容器类型的数据个数  

---

## 3. PyCharm中波浪线含义

| 波浪线颜色 | 含义                   | 解决方法                    |
| ---------- | ---------------------- | --------------------------- |
| **黄色**   | 代码不符合PEP8书写规范 | 使用快捷键 `Ctrl + Alt + L` |
| **绿色**   | 英文单词拼写错误       | 修正拼写错误                |
| **红色**   | 语法错误               | 立即修正语法错误            |

---

## 4. 标识符的定义

**标识符的含义：** Python中所有的命名都叫做标识符  
- 变量名  
- 函数名  
- 类名  
- 对象名  
- 模块名  
- 包名  
- ...  

### 4.1 标识符的命名规则
- **由数字、字母、下划线组成，不能以数字开头，不能是关键字**  
  - 关键字：Python中已经取好的名字就叫做关键字  
  - 每个关键字都会有独特的含义  
  - 在PyCharm中显示：橙色高亮  

```python
import keyword  

# 查看关键字列表  
print(keyword.kwlist)  
# ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']  

print(len(keyword.kwlist))  # 35  
```

- **区分大小写**  
- **不建议使用不符合规范的名字**  

### 4.2 标识符命名建议
- **见名知意**：尽量使用英文单词（如姓名：name，年龄：age）  
- **小驼峰命名法**：第一个单词首字母小写，后续单词首字母大写（如：myName）  
- **大驼峰命名法**：每个单词首字母都大写（类名常用，如：FirstName）  
- **下划线命名法**：单词间用下划线连接（如：user_name）  

**注意点：**  
- 所有标识符命名唯一可以使用的符号就是 `_` 下划线  

---

## 5. 格式化输出

**格式化输出的含义：**  
- 按照指定的方式，拼接字符串新的内容进行输出  
- 当输出一个字符串的同时，结合变量一起输出  

### 5.1 占位符格式化输出
```python
name = "蔡徐坤"  
age = 18  
height = 1.78  

# 占位符类型：  
# %s - 字符串  %d - 整数  %f - 浮点数  
print('我的名字是：%s' % name)  
print('我今年：%d岁' % age)  

# 浮点数默认保留六位小数  
# %.2f 保留两位小数  
print('我的身高是：%.2f米' % height)  

# 多个变量同时输出  
print('我的名字是：%s，我今年：%d岁，我的身高是：%.2f米' % (name, age, height))  
```

### 5.2 format函数格式化输出
```python
name = "蔡徐坤"  
age = 18  
height = 1.78  

# 默认顺序  
print("我的名字是：{}，我今年：{}岁，我的身高是：{}米".format(name, age, height))  

# 指定变量名  
print("我的名字是：{name}，我今年：{age}岁，我的身高是：{height}米".format(  
    name=name, age=age, height=height  
))  
```

---

## 6. 标准输入

通过 `input()` 函数接收用户输入：  
```python
# input在没有收到数据之前，程序会挂起  
password = input("请输入你的密码：")  
print("您输入的密码是：", password)  
print(type(password))  # <class 'str'>  

# 注意：input获取的所有内容都会转为字符串  
num1 = input("请输入第一个数字：")  
num2 = input("请输入第二个数字：")  
print(f"相加结果为：{num1 + num2}")  # 字符串拼接（如：输入12和13得到"1213"）  

# 转换类型示例  
num1 = int(input("请输入第一个数字："))  
num2 = int(input("请输入第二个数字："))  
print(f"相加结果为：{num1 + num2}")  # 数值相加  
```

**总结：**  
- 使用 `input()` 时程序会挂起，不按回车不会往下执行  
- 输入的所有数据都会转为字符串类型  
- 需要数值计算时需手动转换类型（如 `int()`）  





