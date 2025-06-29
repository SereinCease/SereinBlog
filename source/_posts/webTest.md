---
title: webTest
date: 2025-05-16 11:10:59
tags:
categories:
cover:
description:
---











## 元素等待

找不到页面元素时在一定时间内等待

### 隐式等待

抛出元素不存在的异常 NoSuchElementException 

```python
#driver.implicitly_wait(timeout)，timeout：为等待最大时长，单位：秒
driver.implicitly_wait(10)
```

### 显式等待

抛出超时异常 TimeoutException 

```python
# WebDriverWait(driver, timeout, poll_frequency=0.5)
#driver为浏览器驱动对象，timeout为超时的时长，poll_frequency为检测间隔时间
#调用方法 until(method)：直到...时
element = WebDriverWait(driver,10,1).until(lambda x:x.find_element(By.CSS_SELECTOR,"#userA"))
element.send_keys("admin")
```

select

```python
element = driver.find_element(By.CSS_SELECTOR,"#selectA")
select = Select(element)
select.select_by_index(2)
sleep(3)
select.select_by_value('sh')
sleep(3)
select.select_by_visible_text("A北京")
sleep(10)
```

弹出框

```python
# 获取弹出框对象
# alert.text，返回alert/confirm/prompt中的文字信息
# alert.accept() ，接受对话框选项
# alert.dismiss()， 取消对话框选项
alert = driver.switch_to.alert
alert.dismiss()
```

