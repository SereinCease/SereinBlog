---
title: web自动化-电商实战
date: 2025-07-05 19:29:57
tags: 
categories: web自动化
cover:
description:
---

# 四、Web自动化实战-电商项目个人中心用例设计和实现（day28）

## 1. 需求分析与用例设计

### 项目地址
http://116.62.63.211/shop/ 

核心业务：购物

个人中心业务：

- 业务管理模块

  - 订单管理：数据展示、筛选、交互

  - 售后管理：数据展示、筛选、交互

  - 商品收藏：数据展示、筛选、交互

- 财产中心模块

  - 我的积分：数据展示、筛选、交互

- 资料管理模块
  - 个人资料：文件上传、表单
  - 收货地址：数据展示、管理（增删查改）、表单
  - 安全设置：修改、表单
  - 我的消息：数据展示、筛选、交互
  - 我的足迹：数据展示、筛选、交互
  - 留言问答：数据展示、筛选、交互

必测的三个功能：

- 收货地址：数据展示、管理（增删查改）、表单

- 订单管理：数据展示、筛选、交互

- 个人资料：文件上传、表单

### **前置条件**  

1. 启动浏览器并登录  
2. 准备头像图片文件  

### 用例步骤

进入个人中：http://116.62.63.211/shop/personal/index.html
获取旧头像src属性：/html/body/div[4]/div[3]/div/dl/dd[1]/img
点击修改按钮：/html/body/div[4]/div[3]/div/dl/dd[1]/span/a
为input选择图：//*[@id="user-avatar-popup"]/div/div[2]/form/div[2]/div/input
点击上传：//*[@id="user-avatar-popup"]/div/div[2]/form/button
获取系统提示：//p[@class="prompt-msg"]
断言：系统提示 == "上传成功"
重新进入个人中心：http://101.34.221.219:8010/?s=personal/index.html
获取新头像src属性：/html/body/div[4]/div[3]/div/dl/dd[1]/img
断言： 旧头像src属 ！= 新头像src属性

### 后置操作

1.关闭浏览器

---

## 2. 代码实现

```python
import time
import pytest
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
from webdriver_helper import get_webdriver

avatar_img = r'D:\PythonProgram\webTest\data\birthday.jpg'
username = 'lyreth'
password = '123456'
#前置条件-准备头像图片
@pytest.fixture()
def pre_img():
    return avatar_img
#前置条件-启动浏览器并自动登录
@pytest.fixture
def user_driver():
    driver = get_webdriver('chrome')
    driver.maximize_window()
    driver.get('http://116.62.63.211/shop/user/logininfo.html')
    driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[1]/input').send_keys(
        username)
    driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[2]/div/input').send_keys(
        password)
    driver.find_element(By.XPATH, '/html/body/div[4]/div/div[2]/div[2]/div/div/div[1]/form/div[3]/button').click()
    msg = WebDriverWait(driver, 10).until(lambda d: d.find_element(By.XPATH, '//p[@class="prompt-msg"]').text)
    print('系统提示', msg)  # 打印元素内容
    assert msg == '登录成功'
    yield driver
    driver.quit()

def test_new_avatar(user_driver,pre_img):
    user_driver.get('http://116.62.63.211/shop/personal/index.html')
    user_driver.implicitly_wait(10)
    old_img_src=user_driver.find_element(By.XPATH, '/html/body/div[4]/div[3]/div/dl/dd[1]/img').get_attribute('src')
    user_driver.find_element(By.XPATH, '/html/body/div[4]/div[3]/div/dl/dd[1]/span/a').click()
    user_driver.find_element(By.XPATH, '//*[@id="user-avatar-popup"]/div/div[2]/form/div[2]/div/input').send_keys(pre_img)
    time.sleep(3)
    user_driver.find_element(By.XPATH, '//*[@id="user-avatar-popup"]/div/div[2]/form/button').click()

    msg = WebDriverWait(user_driver, 10).until(lambda d: d.find_element(By.XPATH, '//p[@class="prompt-msg"]').text)

    assert msg == '上传成功'
    user_driver.get('http://116.62.63.211/shop/personal/index.html')
    time.sleep(3)
    new_img_src=user_driver.find_element(By.XPATH, '/html/body/div[4]/div[3]/div/dl/dd[1]/img').get_attribute('src')
    assert old_img_src != new_img_src
```

上传按钮不能交互报错：

使用强制等待（隐式等待只能判断元素是否存在），发现变为可点击

如何写显式等待代码：

思路：能不能变成可点击，能不能自动变成可点击，如何判断它是可以点击的

1.难题：不能点击

2.难题：不能得到系统提示

```python
def func(d):
    el = user_driver.find_element(By.XPATH, '//*[@id="user-avatar-popup"]/div/div[2]/form/button')
    try:
        el.click()
    except Exception:
        return False

    msg = WebDriverWait(user_driver, 10).until(lambda d: d.find_element(By.XPATH, '//p[@class="prompt-msg"]').text)
    return msg

msg = WebDriverWait(user_driver, 10).until(func) # 函数不加括号
```

等待策略中做选择：

- 可维护性，大于技术深度

遇到错误：

1. 出错类型
2. 出错原因
3. 出错解决办法

# 五、Web自动化实战-电商项目复杂表单元素自动化操作（day29）

## 1. 引入专业测试框架目录结构
  1. 安装依赖
2. 启动框架
3. 查看日志
4. 查看报告

## 2. 收货地址用例设计

**收货地址**: 数据展示、管理（增删查改）、表单  

**默认地址规则**:  
- 没有默认地址时，最新的地址就是默认  
- 有默认地址时，标记默认的地址就是默认  
- 只会有1个默认地址  

### 1. 前置操作
1. 启动浏览器  
2. 登录账号  
3. 进入【我的地址】：http://116.62.63.211/shop/useraddress/index.html
4. 创建默认地址  

---

### 2. 用例步骤

#### 创建用例
**有默认地址场景**:  
1. 创建新地址（参数：非默认）  
   1. 点击【新增地址】：/html/body/div[4]/div[3]/div/div/button  
   2. 输入【别名】：/html/body/div[1]/form/div[1]/input  
   3. 输入【姓名】：/html/body/div[1]/form/div[2]/input  
   4. 输入【电话】：/html/body/div[1]/form/div[3]/input  
   5. 选择【省份】：  
      1. 点击元素加载选项：/html/body/div[1]/form/div[4]/div[1]/a/span  
      2. 点击选项完成选择：/html/body/div[1]/form/div[4]/div[1]/div/ul/li[15]  
   6. 选择【城市】：  
      1. 点击元素加载选项：/html/body/div[1]/form/div[4]/div[2]/a/span  
      2. 点击选项完成选择：/html/body/div[1]/form/div[4]/div[2]/div/ul/li[2]  
   7. 选择【区县】：  
      1. 点击元素加载选项：/html/body/div[1]/form/div[4]/div[3]/a/span  
      2. 点击选项完成选择：/html/body/div[1]/form/div[4]/div[3]/div/ul/li[4]  
   8. 输入【详细地址】：//*[@id="form-address"]  
   9. 切换【默认】：/html/body/div[1]/form/div[6]/div  
   10. 点击【保存】：/html/body/div[1]/form/div[7]/button  
2. 断言：  
   1. 系统提示：/html/body/div[4]/div/p == "操作成功"  
   2. 默认地址的别名 != 本次输入的别名  

**没有默认地址场景**:  
1. 创建新地址（参数：非默认）同上  
2. 断言：  
   1. 系统提示：/html/body/div[4]/div/p == "操作成功"  
   2. 默认地址的别名 == 本次输入的别名  

#### 删除用例
**有默认地址场景**:  
1. 删除地址  
   1. 点击【删除】：//*[@id="data-list-5605"]/div[2]/a[3]  
   2. 点击【确定】：//*[@id="am-modal-ubh1v"]/div/div[3]/span[2]  
2. 断言：  
   1. 系统提示 = "删除成功"  
   2. 新默认地址别名 = 旧默认地址别名  

**没有默认地址场景**:  
1. 创建新地址（参数：非默认）同上  
2. 断言：  
   1. 系统提示 = "删除成功"  
   2. 新默认地址别名 != 旧默认地址别名  

#### 编辑用例
**有默认地址场景**:  
1. 编辑地址并设为默认  
   1. 点击【编辑】：//*[@id="data-list-5610"]/div[2]/a[1]  
   2. 点击【默认地址】：/html/body/div[1]/form/div[6]/div  
2. 断言：  
   1. 系统提示 = "操作成功"  
   2. 新默认地址别名 != 旧默认地址别名  

**没有默认地址场景**:  
1. 自动指定最新地址（默认地址只有一个）  

---

### 3. 后置操作
1. 删除默认地址  
2. 删除测试新增地址  
3. 关闭浏览器  

---

### 4. 整理前后置关系
- **前置**：启动浏览器、登录  
  - **场景A：有默认地址**  
    - 前置：进入我的地址，创建1默认地址+1个非默认地址  
    - 用例1：创建  
    - 用例2：编辑  
    - 用例3：删除  
    - 后置：删除全部地址  
  - **场景B：没有默认地址**  
    - 前置：进入我的地址，创建2个非默认地址  
    - 用例1：创建  
    - 用例2：编辑  
    - 用例3：删除  
    - 后置：删除全部地址  
- **后置**：关闭浏览器  

---

## 3. 收货地址用例实现

### 1. 实现fixture（前置、后置）
```python
def new_address(driver, alias, name, tel, province, city, county, address, default=False):
    # 点击新增地址按钮
    driver.get('http://116.62.63.211/shop/useraddress/index.html')
    driver.find_element(By.XPATH, '/html/body/div[4]/div[3]/div/div/button').click()
    
    # 切换到iframe
    iframe = driver.find_element(By.XPATH, '//iframe[@src="http://116.62.63.211/shop/useraddress/saveinfo.html"]')
    driver.switch_to.frame(iframe)
    
    # 填写表单
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[1]/input').send_keys(alias)
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[2]/input').send_keys(name)
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[3]/input').send_keys(tel)
    
    # 选择省份
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[4]/div[1]/a/span').click()
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[4]/div[1]/div/u1/i[15]').click()
    
    # 选择城市
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[4]/div[2]/a/span').click()
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[4]/div[2]/div/u1/i[2]').click()
    
    # 选择区县
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[4]/div[3]/a/span').click()
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[4]/div[3]/div/u1/i[4]').click()
    
    # 输入详细地址
    driver.find_element(By.XPATH, '//*[@id="form-address"]').send_keys(address)
    
    # 设置默认地址
    if default == True:
        driver.find_element(By.XPATH, '/html/body/div[1]/form/div[6]/div').click()
    
    # 保存
    driver.find_element(By.XPATH, '/html/body/div[1]/form/div[7]/button').click()
    driver.switch_to.default_content()

def del_address(driver, del_el):
    # JS点击解决位置变化问题
    driver.execute_script('arguments[0].click()', del_el)
    # 确认删除
    driver.find_element(By.XPATH, '//span[text()="确定"]').click()

@pytest.fixture(scope="module")
def has_default_address(user_driver):
    # 创建测试数据
    user_driver.get('http://116.62.63.211/shop/useraddress/index.html')
    new_address(user_driver, 'AAA', '默认地址', '1231231231', '湖北省', '武汉市', '汉阳区', '我的地址', default=True)
    new_address(user_driver, 'BBB', '非默认地址', '1231231231', '湖北省', '武汉市', '汉阳区', '我的地址')
    
    yield user_driver
    
    # 清理数据
    user_driver.get('http://116.62.63.211/shop/useraddress/index.html')
    del_el_list = user_driver.find_elements(By.XPATH, '//a[text()="删除"]')
    for del_el in del_el_list:
        del_address(user_driver, del_el)
```

### 2. 实现测试用例
#### 场景A：有默认地址
- 前置：创建1默认地址+1个非默认地址  
  - 用例1：创建  
  - 用例2：编辑  
  - 用例3：删除  

- 后置：删除全部地址  

#### 1. iframe处理
原因：把另一个网页的内容，嵌入当前页面显示，但是元素不属于当前页面，无法定位

**解决方案**：  让selenium切换到另一个网页

```python
# 进入iframe
#//*[@id="am-modal-m2wav"]/div/div[2]/iframe
#//*[@id="am-modal-ljscr"]/div/div[2]/iframe
ifame = driver.find_element(By.XPATH,'//iframe[@src="http://116.62.63.211/shop/useraddress/saveinfo.html"]')
driver.switch_to.frame(ifame)
```

具体方法：
1. 定位到iframe元素、调用selenium切换到这个元素（进去）
2. 定位需要iframe中的元素 （定位）
3. iframe中的元素定位完毕之后，切回到主页面（退出）
4. 定位主页面的元素 （恢复正常操作）

#### 2. 点击被拦截解决方案

原因：元素被遮挡或元素位置移动

```python
# 使用JS强制点击
driver.execute_script('arguments[0].click()', element)
```

#### 3. 下拉选择处理
- 原生`<select>`：使用Selenium封装函数  
- `div`实现的下拉：模拟用户操作（点击加载选项 → 点击选择选项）  



# 六、Web自动化实战：POM设计模式深度封装流程（day30）

## 1. POM是什么
**Page Object Model**  
- 字面翻译：页面 对象 模型  
- 本质内涵：把页面进行面向对象式封装  

## 2. 为什么使用POM
学习Python语言的不同阶段：  
1. **脚本**：简单，不可重用，适合代码量少，临时使用场合  
2. **函数**：基本封装，可以重用，适合代码适中，多次使用场合  
3. **面向对象**：深度封装，隐藏细节，适合代码规模大、关系复杂的场合  

**POM的价值**：  
- 将页面内容（元素、网页、文本）和交互（点击、输入）通过面向对象方式深度封装  
- 便于代码复用，简化用法，隐藏细节  
- 中大型UI测试项目必备基础技巧  

## 3. 怎么样使用POM

### 1. 创建BasePage
抽象类，不代表任何具体页面，包含大部分页面可能用到的通用成员：  
1. 保存`driver`对象：控制真正的浏览器  
2. 保存`wait`对象：实现显式等待  
3. 获取系统提示  
4. 通过JS进行强制点击  
5. 其他通用功能  

**注意**：避免重复封装Selenium已有方法（如`find_element`）：  
```python
# 反面典型 - 无需重复封装
def find_element(self, by, value):  
    return self.driver.find_element(by, value)
```

### 2. 创建Page
具体页面类，代表每个具体页面，包含：  
1. 该页面的独有元素内容和交互方式  
2. 为不同页面创建不同的类  
3. 在类中为不同操作创建方法  
4. 在类中为页面元素创建可复用元素  

### 3. 使用Page
在测试用例中使用Page对象，而非直接使用Selenium：  
```python
# 用例中使用示例
login_page = LoginPage(driver)
login_page.input_username("test_user")
login_page.input_password("pass123")
login_page.click_submit()
```

### 4. POM特点总结
1. **面向对象抽象**：  
   - 相似页面可复用元素定位  
   - 大部分页面可复用交互逻辑  
2. **灵活方法定义**：  
   - 自定义方法名称、参数、返回值  
   - 方法间自由调用  
   - 子类可重写同名方法  
3. **深度封装细节**：  
   - 用例只需关心操作步骤，不关心实现细节  
   - 页面变化（元素/交互变更）时用例无需修改  

---

## 练习
1. 将登录页面进行POM封装，供fixture使用  
2. 将头像上传页面进行POM封装，供测试用例使用  

```
```




