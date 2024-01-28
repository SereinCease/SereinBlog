---
title: python学习
date: 2023-05-19 15:00:17
tags: Python
---

##### \# 1.type()查看类型信息

```python
print(type("hello"))
str_a = "hello"
str_type = type(str_a)
print(str_type)


```

##### \# 2.强制类型转换 int(),float(),str()

```python
a = str(111)
print(type(a),a)
```

##### \# 3.字符串定义

```python
str1 = '字符串'
str2 = "字符串"
str3 = """字符串"""
str4 = "\"字符串"
```

##### \# 4.字符串拼接

```python
name = "学习"
print("我爱" + name)
```



##### \# 5.字符串格式化 %s,%d,%f占位

```python
num = 99
message = "我爱%s%s天" % (name, num)
print(message)
```



##### \# 6.精度

```python
float_num = 99.999
print("保留两位小数: %.2f" % float_num)
print(f"格式化：{float_num}")
```



##### \# 7.表达式格式化

```python
print("1*1的结果是:%d" % (1*1))
print("字符串类型: %s" % type(name))
```



##### \# 练习题

```python
name1 = "名字"
stock_price = 19.99
stock_code = "003032"
growth_price = 1.2
growth_day = 7
print(f"公司:{name1},股票代码：{stock_code},当前股价:{stock_price}")
print("每日增长系数:%.2f,经过了%d天的增长，股价达到了：%.2f" %(growth_price, growth_day, stock_price*growth_price**growth_day))
```



##### 

##### \# 8.input语句

```python
ID = input("请输入你的ID")
print("yourID为 %s" % ID)
```



##### \# Bool类型

```python
print(f"1小于2是对的吗{1 < 2} ")py
```



##### \# 9.if 语句

```python
import random

num = random.randint(1, 10)
guess_num = int(input("输入你要第一次猜测的数字:"))
if guess_num == num:
    print("猜对啦")
else:
    if guess_num < num:
        print("猜小啦")
    else :
        print("猜大啦")
    guess_num = int(input("输入你要第二次猜测的数字:"))
    if guess_num == num:
        print("猜对啦")
    else:
        if guess_num < num:
            print("猜小啦")
        else:
            print("猜大啦")
        guess_num = int(input("输入你要第三次猜测的数字:"))
        if guess_num == num:
            print("猜对啦")
        else:
            if guess_num < num:
                print("猜小啦")
            else:
                print("猜大啦")
            print(num)
```

```
import random

num = random.randint(1, 10)
count = 0
flag = True
while flag:
    i = int(input("输入你要猜测的数字:"))
    if i < num:
        print("猜小了")
    elif i > num:
        print("猜大了")
    else:
        print("猜对了")
        flag = False
    count += 1
print(count)
```



##### #10 练习

```
print("hello", end='')
print("hello\tworld")
print("hiiiii\tworld")
```

九九乘法表

```python
i = 1
j = 1
while(i < 9):
    while(j <= i):
        print(f"{j}*{i}={i*j}\t", end='')
        j +=1
    j = 1
    i +=1
    print()

```

```python
import random

count = 10000
for i in range(1,21):
    num = random.randint(1,10)
    if num < 5:
        print(f"员工{i},绩效分{num},低于5，不发工资")
        continue

    if count != 0:
        count = count - 1000
        print(f"向员工{i}发放工资1000元，账户余额还剩{count}元")
    else:
        print("工资发放完毕")
        break

```

