---
title: web自动化基础
date: 2025-06-30 18:15:30
tags:
categories:
cover:
description:
---



# 一、Web自动化前端基础及Selenium原理和环境安装

## 1. API自动化和Web自动化区别

- **API自动化**：  
  - 关注数据的流动  
  - 对数据进行设计、传输、验证  

- **Web自动化**：  
  - 围绕浏览器页面  
  - 功能包括：  
    - 启动、关闭网页窗口  
    - 获取、改变窗口大小  
    - 获取、改变网址  
    - 点击、输入、保存  

---

## 2. 浏览器和前端基础

### 1. 元素

网页内容由HTML元素决定，HTML是标记语言，通过标签和标签的属性标记内容。  

**示例HTML代码：**  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>网页标题</title>
</head>
<body>
    输入框：<input type="text">
    <br />
    文本框：<textarea rows="20" cols="40">默认值</textarea>
    <br />
    按钮A: <button>登录</button>
    <br />
    按钮B: <input type="button" value="注册">
    <br />
    <ol>
        <li>张三</li>
        <li>李四</li>
        <li>王五</li>
    </ol>
    <br />
    <select>
        <option>上海</option>
        <option>深圳</option>
        <option>广州</option>
    </select>
</body>
</html>
```

**特点：**  
1. 标签能被浏览器识别并展示特定效果。  
2. 标签属性与标记同等重要。  
3. 标签可嵌套、配合，实现不同效果。  
4. 展示效果可动态更改，不可迷信静态效果。  

### 2. JavaScript

决定网页的动态效果，语法与Python相似。  

**示例代码：**  
```javascript
// 条件判断
if (a == b) {
    console.log('a和b相等');
} else {
    console.log('a和b不相等');
}

// 函数封装
function func(params) {
    if (a == b) { 
        console.log('a和b相等');
    } else { 
        console.log('a和b不相等');
    }
}
```

### 3. DOM（文档对象模型）

D: 文档
O：面向对象
M：模型

将浏览器和网页内容视为对象，通过面向对象方式访问、控制。  

**示例：**  

window：浏览器窗对象
docment：网页内容对象

```javascript
window.location.href = 'https://www.baidu.com';  // 跳转网址
document.body.outerHTML = '<body>你好，你的网页被黑了</body>';  // 修改网页内容
```

---

## 3. Selenium

Selenium是一个开源的、跨平台的、支持多语言的Web自动化测试工具。  

### 1. 安装Selenium

```bash
pip install selenium -U  
```

第三方

```
pip install webdriver-helper==1.*  
```

### 2. 安装浏览器驱动

**方式1：官方自动下载（较慢）**  
```python
from selenium import webdriver  
driver = webdriver.Chrome()  # 使用Chrome浏览器驱动  
```

**方式2：第三方自动下载**  
```python
from webdriver_helper import get_webdriver  
driver = get_webdriver('chrome')  # 使用Chrome浏览器驱动  
```

**方式3：手动下载**  
1. 确定平台（如Windows）和浏览器版本（如120.0.6099.225）。  
2. 从[Chrome驱动官网](https://googlechromelabs.github.io/chrome-for-testing/)下载对应驱动。  

```python
from selenium import webdriver
driver = webdriver.Chrome() # 使用chrome的浏览器驱动
```

### 3. 简单示例

**手动测试步骤：**  
1. 启动浏览器  
2. 访问百度  
3. 输入关键字  
4. 点击搜索按钮  
5. 等待  
6. 截图  

**代码实现：**  
```python
from selenium import webdriver
import time

driver = webdriver.Chrome()  # 1. 启动浏览器
driver.get("https://www.baidu.com")  # 2. 访问百度
driver.find_element('xpath', '//*[@id="kw"]').send_keys("北凡老师")  # 3. 输入关键字
driver.find_element('xpath', '//*[@id="su"]').click()  # 4. 点击搜索按钮
time.sleep(5)  # 5. 等待
driver.get_screenshot_as_file("page.png")  # 6. 截图
driver.quit()  # 7. 关闭浏览器
```

### 4. 底层原理

1. 启动浏览器驱动。  
2. 与驱动建立连接，要求驱动启动浏览器。  
3. 发送HTTP请求（如命令`get`+参数`百度网址`）。  
4. 发送HTTP请求：命令（findElement）+参数（{"using": xpath, "value": //*[@id="su"]}）
5. 发送HTTP请求：命令（sendKeysToElement）+参数（...）
6. 发送HTTP请求：命令（...）+参数（...）

selenium底层原理：
1. 启动浏览器驱动
2. 向浏览器驱动发送HTTP请求
3. HTTP请求，使用webdriver协议标准  

可以扩展出：appium

向收集发送请求，实现APP自动化





































