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





# 二、Web自动化Selenium浏览器控制及元素控制

**项目地址：http://101.34.221.219:8010/**

---

## 1. 浏览器控制

### WebDriver对象基础操作

#### 启动与关闭浏览器
```python
from selenium import webdriver
import time

# 启动Chrome浏览器
driver = webdriver.Chrome()  

# 访问百度
driver.get('https://www.baidu.com')  

# 定位搜索框并输入关键字
driver.find_element('xpath', '//*[@id="kw"]').send_keys("北凡老师")  

# 点击搜索按钮
driver.find_element('xpath', '//*[@id="su"]').click()  
time.sleep(5)  # 等待5秒

# 截图保存
driver.get_screenshot_as_file("page.png")  

# 关闭浏览器
driver.quit()  
```

**启动方式：**

- 实例化：`driver = webdriver.Chrome()`  

**关闭**

- 销毁： `del driver`

- 方法： `driver.quit()`

- 自动关闭（推荐使用上下文管理器）：  

  ```python
  with webdriver.Chrome() as driver:
      print(driver) #输出 WebDriver 对象的内存地址和基本信息
  ```



#### 窗口控制

- 最大化窗口：`driver.maximize_window()`  
- 最小化窗口：`driver.minimize_window()`  
- 全屏窗口：`driver.fullscreen_window()`  
- 指定窗口大小：`driver.set_window_size(2000, 600)`  

#### 导航操作
- 跳转页面：`driver.get("https://www.baidu.com")`  
- 返回上一页：`driver.back()`  
- 前进下一页：`driver.forward()`  
- 刷新页面：`driver.refresh()`  

#### 获取页面信息

**获取元素**

- find_element：返回元素对象
- find_elements：返回列表，列表中可能有多个元素对象

```python
print(driver.title)          # 网页标题
print(driver.current_url)    # 网页地址
print(driver.page_source)    # 网页HTML源码

# 截图处理（三种方式获取、处理图片内容）
print(driver.get_screenshot_as_base64())  # Base64编码后的二进制
print(driver.get_screenshot_as_png())     # 还原后的二进制
driver.get_screenshot_as_file("page.png") # 把二进制保存到文件

# 获取Cookies
for cookie in driver.get_cookies():
    print(cookie)

# 执行JavaScript
ts = driver.execute_script('''console.log('1111'); 
return localStorage.getItem('CVStringTimestamp');''') #localStorage.getItem('CVStringTimestamp为js代码
```

---

## 2. 元素控制

### Element对象操作

#### 点击元素

所有的元素都可以点击，只是点击效果不同，没有直接反馈

```python
a = driver.find_element('xpath', '//*[@id="su"]')  # 定位元素
a.click()  # 点击元素
```

#### 输入文本

只有输入框、文本框可以输入

```python
a = driver.find_element('xpath', '//*[@id="kw"]')  
a.send_keys("beifan")  # 输入内容
a.clear()             # 清空输入框
a.send_keys("bai11111111")  # 重新输入
```

#### 文件上传
仅适用于 `<input type="file">` 元素：  
```python
b = driver.find_element('xpath', '//*[@id="form"]/div/div[2]/div[2]/input')  
b.send_keys(r"C:\Users\admin\Desktop\jmeter.png")  # 输入文件绝对路径
```

#### 获取元素信息
```python
a = driver.find_element('xpath', '//*[@id="kw"]')  # 输入框
b = driver.find_element('xpath', '//*[@id="su"]')  # 按钮

# 获取元素属性
print(a.text)                      # 文字内容
a.screenshot("a.png")              # 元素截图
print(a.rect)                      # 元素位置和大小（字典格式）
print(a.tag_name)                  # 元素标签名（如：input）
print(a.get_attribute("type"))     # 获取属性值（如：text）
```









# 三、Web自动化元素定位及定位等待解决定位失败

## 1. 元素定位方法

### 基础概念
- **定位策略 + 定位表达式**：通过特定方式（如XPath、CSS）定位元素。  
- **返回对象**：  
  - `find_element`：返回单个元素对象。  
  - `find_elements`：返回元素列表。  

### 示例：登录页面操作
```python
from selenium import webdriver
from selenium.webdriver.common.by import By

with webdriver.Chrome() as driver:
    driver.maximize_window()
    driver.get("http://101.34.221.219:8010/?s=user/logininfo.html")
    
    # 输入账号
    driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[1]/input').send_keys("beifan_1205")
    
    # 输入密码
    driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[2]/div/input').send_keys("beifan_1205")
    
    # 点击登录
    driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[3]/button').click()
    
    input("暂停执行")  # 等待用户输入
```

---

## 2. 元素定位策略

### 支持的定位策略（`By`类）
```python
from selenium.webdriver.common.by import By

class By:
    ID = "id"                   # 通过id属性
    XPATH = "xpath"             # 通过XPath表达式
    LINK_TEXT = "link text"     # 通过超链接文本（精确匹配）
    PARTIAL_LINK_TEXT = "partial link text"  # 通过超链接文本（模糊匹配）
    NAME = "name"               # 通过name属性
    TAG_NAME = "tag name"       # 通过标签名
    CLASS_NAME = "class name"   # 通过class属性
    CSS_SELECTOR = "css selector"  # 通过CSS选择器
```

### 策略选择建议
- **CSS选择器**：  
  - 适用于简单、高性能场景，兼容性好。  
  - 无法直接实现文本匹配（如`LINK_TEXT`）。  
- **XPath**：  
  - 功能强大，支持复杂逻辑（如父节点定位、文本匹配）。  
  - 语法灵活但执行速度略慢。  

**示例：文本匹配**  
```python
driver.find_element(By.LINK_TEXT, '注册').click()       # 精确匹配
driver.find_element(By.PARTIAL_LINK_TEXT, '注').click()  # 模糊匹配
```

---

## 3. XPath语法详解

### 基础语法
- **路径符号**：  
  - `/`：根路径或子节点。  
  - `//`：任意层级。  
  - `..`：父节点。  
- **条件限定**：  
  - `[@attr="value"]`：按属性筛选。  
  - `[n]`：按位置选择。  

**示例**  
```xpath
/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[1]/input  # 绝对路径
//input[@name="accounts"]                                             # 相对路径
//*[contains(text(), "登")]                                           # 文本包含
```

### 常用函数
- `text()`：匹配元素文本。  
- `contains()`：判断文本是否包含指定内容。  
- `starts-with()`：判断文本是否以指定内容开头。  

---

## 4. 元素定位失败原因及解决方案

### 常见问题
1. **元素未加载**：  
   - 解决方案：添加显式等待（如`time.sleep`或`WebDriverWait`）。  
2. **定位表达式错误**：  
   - 解决方案：使用开发者工具验证XPath/CSS。  
3. **元素被遮挡**：  
   - 解决方案：通过JavaScript绕过遮挡（如`execute_script("arguments[0].click()", element)`）。  
4. **元素状态无效**：  
   - 解决方案：修改元素属性（如移除`disabled`状态）。  

**示例：处理遮挡和状态问题**  
```python
# 通过JS点击被遮挡元素
el = driver.find_element(By.LINK_TEXT, '注册')
driver.execute_script("arguments[0].click()", el)

# 移除禁用状态并操作
el_a = driver.find_element(By.XPATH, '//input[@disabled]')
driver.execute_script('arguments[0].removeAttribute("disabled")', el_a)
el_a.send_keys("123")
```

---

## 5. 最佳实践
- **优先使用相对XPath**：避免因页面结构调整导致定位失败。  
- **结合显式等待**：确保元素加载完成后再操作。  
- **简化定位表达式**：如`//input[@name="accounts"]`优于长绝对路径。  

**总结**：合理选择定位策略，结合等待和JS处理复杂场景，可显著提升自动化脚本稳定性。  
```
```



























