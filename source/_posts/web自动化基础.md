---
title: web自动化基础
date: 2025-06-30 18:15:30
tags:
categories:
cover:
description:
---



# 一、Web自动化前端基础及Selenium原理和环境安装（day25）

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



# 二、Web自动化Selenium浏览器控制及元素控制（day26）

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

# 三、Web自动化元素定位及定位等待解决定位失败（day27）

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
    driver.get("http://116.62.63.211/shop/user/loginInfo.html")
    # 输入账号
    driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[1]/input').send_keys("lyreth")
    # 输入密码
    driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[2]/div/input').send_keys("123456")
    # 点击登录
    driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[3]/button').click()
    input("暂停执行")  # 等待用户输入
```

---

## 2. 元素定位策略

过去：8种
现在：5种
将来：4种 （推测）

### 查看代码

```python
# selenium.webdriver.common.by.By

class By:
    """Set of supported locator strategies."""
    ID = "id"                   # 通过id属性
    XPATH = "xpath"             # 通过XPath表达式
    LINK_TEXT = "link text"     # 通过超链接文本（精确匹配）
    PARTIAL_LINK_TEXT = "partial link text"  # 通过超链接文本（模糊匹配）
    NAME = "name"               # 通过name属性
    TAG_NAME = "tag name"       # 通过标签名
    CLASS_NAME = "class name"   # 通过class属性
    CSS_SELECTOR = "css selector"  # 通过CSS选择器
```

```python
# selenium.webdriver.remote.webdriver.WebDriver.find_element
    if by == By.ID:
        by = By.CSS_SELECTOR
        value = f'[id="{value}"]'
    elif by == By.CLASS_NAME:
        by = By.CSS_SELECTOR
        value = f".{value}"
    elif by == By.NAME:
        by = By.CSS_SELECTOR
        value = f'[name="{value}"]'
```

**以下策略：不可以被CSS取而代之**

- XPATH
- LINK_TEXT
- PARTIAL_LINK_TEXT

**CSS和XPATH：**

1. 开发工具
2. 选择元素
3. 右键复制

**LINK_TEXT和PARTIAL_LINK_TEXT**

- 记录A标签中的显示文本
- 调用find_element

```python
driver.get('http://101.34.221.219:8010/?s=user/logininfo.html')
# driver.find_element(By.LINK_TEXT, '注册').click() # 对A标签文本内容 精确匹配
driver.find_element(By.PARTIAL_LINK_TEXT, '注').click() # 对A标签文本内容 模糊匹配
```

**CSS or Xpath？**

- 相同点：  
  - 网页中用一个元素，既可以被CSS定位，也可以被XPATH定位
  - 
    在Chrome底层，CSS选择器和XPATH都是通过JS实现
- 不同点：
  - CSS选择器在所有的浏览器中被支持，执行速度有保障  
  - CSS无法完成复杂的元素定位（比如：实现LINK_TEXT、定位到父元素），往往需要JS配合

**示例：文本匹配**  
```python
driver.find_element(By.LINK_TEXT, '注册').click()       # 精确匹配
driver.find_element(By.PARTIAL_LINK_TEXT, '注').click()  # 模糊匹配
```

---

## 3. XPath语法详解

XPATH 是XML的查询语言，支持逻辑判断、函数调用

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

  ```xpath
  //a[text()="登录"] 等同于 LINK_TEXT
  ```

  `contains()`：判断文本是否包含指定内容。  

  ```xpath
  //*[contains(text(),"登")] 等同于 PARTIAL_LINK_TEXT
  ```

- `starts-with()`：判断文本是否以指定内容开头。  

  ```xpath
  //*[starts-with(text(),"登")]
  ```

不是所有的XPATH函数都被浏览器支持

---

## 4. 元素定位失败原因及解决方案

### 常见原因

1. **元素不存在**：尚未加载或已经消失

   ex：弹出的提示框

   解决方案：添加等待（如`time.sleep`，`input("暂停执行")`或`WebDriverWait`）。  

   ```python
   #码尚商城的登录成功提示框
   with get_webdriver('chrome') as driver:
       # 访问登录页面
       driver.get('http://116.62.63.211/shop/user/loginInfo.html')
       # 定位账号输入框
       driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[1]/input').send_keys(
           "lyreth")
       # 定位密码输入框
       driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[2]/div/input').send_keys(
           "123456")
       # 点击登录按钮                   /html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[3]/button
       driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[3]/button').click()
       # / html / body / div[10] / div / p
       time.sleep(1)
       el = driver.find_element(By.XPATH, '/html/body/div[10]/div/p')
       print(el.text)
       input()
   ```

2. **元素存在，定位不到**：

   ex：id发生变化

   解决方案：手动写Xpath路径

   ```python
   with get_webdriver() as driver:
       # driver.maximize_window()
       driver.get('https://element-plus.org/zh-CN/component/form.html#%E5%85%B8%E5%9E%8B%E8%A1%A8%E5%8D%95')
       # input("暂停执行")
       # //*[@id="el-id-6164-140"] //*[@id="el-id-6722-140"]
       el = driver.find_element(By.XPATH, '//*[@id="page-content"]/div/div/div[1]/div/div[2]/div[1]/form/div[1]/div/div/div/input') # no such element:
       el.send_keys("123")
       input("暂停执行") # 等待输入
   ```

3. **定位成功，不能交互**：被遮挡

   解决方案：通过JavaScript绕过遮挡（如`execute_script("arguments[0].click()", element)`）。  

   ```python
   with get_webdriver() as driver:
   
       driver.get('http://116.62.63.211/shop/goods/index/id/5.html')
       el = driver.find_element(By.XPATH,'/html/body/div[4]/div[2]/div[2]/div/div[3]/div[2]/button[1]') # no such element:
       el.click() # 可以成功，但会产生遮挡
       el = driver.find_element(By.LINK_TEXT, '注册')
       # el.click() # 失败
       driver.execute_script("arguments[0].click()", el) # js模拟点击 arguments[0]表示传入参数的第一个，el表示传入的参数，此时el被注册覆盖，因此传入的是注册按钮
       input("暂停执行") # 等待输入
   ```

4. **可以交互，没有效果**：元素内容、状态不对

   如移除`disabled`状态

   ```python
   with get_webdriver() as driver:
       driver.maximize_window()
       driver.get('https://element-plus.org/zh-CN/component/checkbox.html#%E5%9F%BA%E7%A1%80%E7%94%A8%E6%B3%95')
       el_a = driver.find_element(By.XPATH,'//*[@id="page-content"]/div/div/div[1]/div/div[1]/div[1]/div[4]/label[1]/span[1]/input') # no such element:
       el_b = driver.find_element(By.XPATH,'//*[@id="page-content"]/div/div/div[1]/div/div[1]/div[1]/div[4]/label[1]/span[2]')
       el_b.click() # 不报错，但没效果
       input("暂停执行") # 等待输入
       driver.execute_script('''arguments[0].removeAttribute("disabled")''', el_a)
       # 修改元素状态
       el_b.click() # 不报错，有效果
       input("暂停执行") # 等待输入
   ```


## 5. 定位等待策略

元素尚未出现，或者已经消失

### 1. 强制等待
```python
input("暂停执行") # 需要人工干预，才能恢复
time.sleep(5) # 暂停固定的时间，随后自动恢复执行
```

- **优点**：简单直接。
- **缺点**：效率低，无法动态响应元素加载

### 2. 隐式等待

```python
driver.implicitly_wait(20)  # 全局等待最多20秒
```

- **优点**：提前出现，提前结束等待，全局生效。
- **缺点**：仅判断元素是否存在，灵活性差。

### 3. 显式等待（推荐）

```python
def func(d):
    print('显示等待，正在重试...')
    el = driver.find_element(By.XPATH, '//p[@class="prompt-msg"]')
    if el.text:
    	return el.text # 返回结果，提前结束等待
    else:
    	return False # 返回假值，继续重试
from selenium.webdriver.support.wait import WebDriverWait
msg = WebDriverWait(driver, 10).until(func) # 函数不加括号
# WebDriverWait，拿着 driver 不断的调用 func，把结果保存到 msg
```

简化版

```python
from selenium.webdriver.support.wait import WebDriverWait
msg = WebDriverWait(driver, 10).until(
lambda d: driver.find_element(By.XPATH, '//p[@class="prompt-msg"]').text)
# 函数不加括号
```

- **优点**：提前出现，提前结束等待，灵活控制等待条件（如元素是否出现、内容是否正确，位置是否合适，效果是否生
  效等）。
- **缺点**：需熟悉Python和Selenium。

原理：
- 创建一个函数，被selenium调用，如果没有满足结束条件就不断重试，如果满足了就提前结束

函数额外要求：

1. 必须有一个参数：driver
2. 返回值：真 或者 假
  - 如果为真，提前结束等待
  - 如果为假，继续重试













