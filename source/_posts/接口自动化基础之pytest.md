---
title: 接口自动化基础之pytest
date: 2025-06-29 11:47:00
tags:
categories:
cover:
description:
---

# 一、接口自动化基础之pytest框架用法、规则、配置、标记(day13)

创建新的项目：api_framwrok_241

## 1. 什么是测试框架

测试框架：抽象出来一个工具集合，提供大量组件或功能：

- 用例发现：自动化的从各目录、各文件种收集测试用例  
- 用例管理：根据需求对用例进行筛选、忽略、跳过等操作  
- 环境管理：在用例执行前后，自动完成某些操作，构造合适的执行条件  
- 用例执行：执行用例的测试步骤  
- 断言：执行用例时，判定执行结果是否符合预期  

大部分的编程语言都有对应测试框架：

- Java: JUnit, TestNG  
- php: phpunit  
- Python: unittest, pytest  
- 更多...... 

unittest:  
- python内置、无需安装  
- 用浓郁Java风格  
- 无法升级、扩展  

pytest:  
- 手动安装、自由切换版本  
- 有浓郁Python风格  
- 有丰富第三方生态进行扩展  
- 完全兼容unittest  

---

## 2. pytest测试框架

### 1. 安装pytest

```bash
pip install pytest    # 安装pytest  
pip install pytest -U # 安装、升级pytest 到最新版  
pip install pytest==7.0   # 安装pytest 7.0版本  
```

- `pip install` 安装第三方库的命令  
- `pytest` 第三方库的名字  
- `-U` 升级，保持最新版  
- `==7.0` 指定版本号  

```bash
pip show pytest  # 查看pytest信息  
pytest  # 启动测试框架  
```

***

### 2. 编写测试用例

1. 创建：`test_`开头的py文件  
2. 创建：`test_`开头的函数  
3. 创建：`assert`断言  

示例文件 `test_abc.py`：
```python
def test_ok():
    assert 1 == 1  # 测试通过

def test_fail():
    assert 1 == 2  # 测试失败
```

***

### 3. 执行测试用例

1. 命令行  
    ```bash
    pytest
    ```

2. 代码  
    ```python
    import pytest
    pytest.main()  # 启动测试框架
    ```

***

### 4. 看懂执行结果

1. 执行环境  
    ```
    platform win32 -- Python 3.12.0, pytest-8.1.1, pluggy-1.4.0
    rootdir: E:\PyProject\api_framework_241
    ```

2. 用例收集情况  
    ```
    collected 2 items
    ```

3. 用例执行过程  
    ```
    test_abc.py .F
    [100%]
    ```

| 缩写 | 单词    | 含义                       |
| ---- | ------- | -------------------------- |
| .    | passed  | 通过                       |
| F    | failed  | 失败（用例执行时报错）     |
| E    | error   | 出错（fixture执行报错）    |
| s    | skipped | 跳过                       |
| X    | xpassed | 预期外的通过（不符合预期） |
| x    | xfailed | 预期内的失败（符合预期）   |

4. 用例失败原因  
    ```
    ================================== FAILURES
    ===================================
    __________________________________ test_fail
    __________________________________
    def test_fail():
    > assert 1 == 2 # 测试失败
    E assert 1 == 2
    test_abc.py:6: AssertionError
    ```
    
5. 测试框架总结信息  
    ```
    =========================== short test summary info
    ===========================
    FAILED test_abc.py::test_fail - assert 1 == 2
    ========================= 1 failed, 1 passed in 0.03s
    =========================
    ```

***

### 5. 用例发现规则

pytest识别、加载测试用例过程称之为用例发现，规则：

1. 遍历所有的目录（venv除外）  
2. 遍历所有 `test_`开头 或者 `_test`结尾的python文件  
3. 遍历所有 `test` 开头的类  
   - 类不能拥有`__init__`方法  
4. 收集 `test_`开头的函数或者方法，作为测试用例  

**重点**：pytest，只有函数和方法，才被视为测试用例，目录、文件、类，作为用例的容器  

## 3. pytest高级用法

### 1. 配置

pytest 有2种配置方式：

- 命令行参数  
- ini配置文件  

查看所有的配置项  
```bash
pytest -h  
```

可以分成三大部分
1. 参数
2. 配置项
3. 环境变量

常用的命令行参数：  

- `-v`：增加详细程度  
- `-q`：减少详细程度  
- `-s`：不进行内容捕获，才能正常的输入输出  
- `-x`：快速退出（冒烟测试）  

常用的ini配置项：  
1. 在根目录中的创建 `pytest.ini` 文件  
2. 创建 pytest 选择器  
3. 按行，添加配置项  

示例 `pytest.ini`：  
```ini
[pytest]
addopts = -s -x
```

add options

配置是用来改变pytest

**约定大于配置**：对于成熟的工具来说，默认配置往往是比较好的配置，可以适用于大部分场景。如非必要，请勿更改。  

***

### 2. 标记mark

mark主要用途是让用例和用例之间变得不同，实现用例的筛选。  

#### 1. 用户自定义标记

1. 注册（在 `pytest.ini` 中）：  
    ```ini
    [pytest]
    markers =
        api
        ui
        ut
        e2e
    ```

2. 标记用例：  
    ```python
    import pytest
    
    @pytest.mark.ut
    def test_ok():
        assert 1 == 1  # 测试通过
    
    @pytest.mark.e2e
    def test_fail():
        assert 1 == 2  # 测试失败
    
    @pytest.mark.api
    def test_baili():
        pass
    
    @pytest.mark.ui
    def test_beifan():
        pass
    ```

3. 筛选用例：  
    ```bash
    pytest -m api  # 只执行拥有api标记的用例
    pytest -m "ut or api"  # 只执行拥有ut或api标记的用例
    pytest -m "ui and api"  # 只执行同时拥有ui和api标记的用例
    ```

#### 2. 框架内置标记

1. 不需要注册，直接使用  
2. 不仅用于筛选，还有特殊效果  
3. 不同的标记，拥有不同的效果  

示例：  
```python
import pytest

@pytest.mark.skip
def test_skip():
    assert 1 == 2  # 失败

@pytest.mark.skipif(1 == 1)  # 跳过
def test_skipif():
    assert 1 == 2  # 失败

@pytest.mark.skipif(1 == 2)  # 不会跳过
def test_skipif():
    assert 1 == 2  # 失败

@pytest.mark.xfail  # 预期失败
def test_passed():
    assert 1 == 2  # 结果失败

@pytest.mark.xfail  # 预期失败
def test_fail():
    assert 1 == 1  # 结果成功
```

#### 3. 参数化测试

参数化测试：通过数据修改参数，从而改变测试用例。  
数据驱动测试 = 参数化测试 + 数据文件  

**参数化之前**：  
```python
def add(a, b):
    return a + b

def test_add_1_10():
    a = 1
    b = 1
    assert 2 == add(a, b)

def test_add_2_20():
    a = 2
    b = 2
    assert 4 == add(a, b)

def test_add_2_30():
    a = 2
    b = 3
    assert 5 == add(a, b)

def test_add_3_3():
    a = 3
    b = 3
    assert 6 == add(a, b)
```

**参数化之后**：  
```python
@pytest.mark.parametrize(
    "a, b, c",  # 1. 列出参数
    [           # 2. 准备参数的值
        [1, 1, 2],
        [2, 2, 4],
        [2, 3, 5],
        [3, 3, 6],  # 3. 根据需求修改数据
    ]
)
def test_add_1_1(a, b, c):
    assert c == add(a, b)
```

# 二、接口自动化基础之pytest框架fixture、常用插件、Allure报告、企业级定制(day14)

## 1. pytest夹具fixture

**夹具**: 在用例执行之前，执行之后，自动的运行自定义代码

**场景**:  
- 执行之前：创建测试账号，执行之后：删除测试账号  
- 执行之前：启动浏览器，执行之后：关闭浏览器  
- 执行之前：创建测试场景，执行之后，销毁测试场景  

### 1. 创建fixture

1. 创建函数  
2. 添加装饰器  
3. `yield`  

```python
@pytest.fixture  
def f():  
    print('启动浏览器') # 前置操作，用例执行之前，自动执行  

    yield '浏览器类型: chrome' # 返回值，使用例使用  

    print('关闭浏览器') # 后置操作，用例执行之后，自动执行  
```

简便的方式创建fixture  

```python
@pytest.fixture  
def f(): # 使用fixture  
    print('启动浏览器')  

    return None  
```

### 2. 使用fixtures

```python
def test_abc(f): # 使用fixture，得到它的返回值
    print('我是用例内容，正在执行')

    print('收到fixture的返回值:', f)
```

#### 执行结果

```
test_abc.py::test_abc 启动浏览器  
我是用例内容，正在执行  
收到fixture的返回值：浏览器类型：chrome  
PASSED关闭浏览器  
```

#### 简便的方式使用fixture

```python
@pytest.mark.usefixtures('f') # 使用fixture  
def test_abc():  
    print('我是用例内容，正在执行')
```

### 3. fixture的作用范围

fixture启动比较慢，能否共享（复用）fixture？

- 每个用例执行fixture：5s * 3(fixture执行次数) = 15s  
- 每个用例复用fixture：5s * 1(fixture执行次数) = 5s  

pytest中，用一个作用范围内fixture会自动的共享（复用）

pytest中，支持5级作用域：

1. function：默认，不共享  
2. class：同一个类中用例，自动共享  
3. module：同一个模块（文件）中用例，自动共享  
4. package：同一个包（目录）中用例，自动共享  
5. session：所有用例，自动共享  

```python
import pytest

@pytest.fixture(scope='class')
def f():
    print('启动浏览器') # 前置操作，用例执行之前，自动执行
    yield '浏览器类型：chrome' # 返回值，供用例使用
    print('关闭浏览器') # 后置操作，用例执行之后，自动执行

class Test_A:
    def test_1(self, f):
    pass
    def test_2(self, f):
    pass
    class Test_B:
    def test_3(self, f):
    pass
```

### 4. conftest.py

从名字上看：测试配置

从内容上看：python代码

从效果上看：被pytest自动导入，实现跨文件的fixture

每个目录都可以创建一个conftest.py

子目录的conftest.py可以屏蔽父目录的conftest.py

哪个文件里用例更近，它的优先级就越高

### 5. fixture的其他写法

对于模块级夹具有 3 几种写法：

1. setup / teardown  
2. setUpModule / tearDownModule  
3. setup_module / teardown_module  

第一种是测试框架 nose 的写法，**pytest 从 7.2.0 开始不再兼容 nose 框架**，这种写法无了

第二种是测试框架 unittest 的写法，这是 python 的标准库，应该会一直兼容下去
第三种是测试框架 pytest 的写法，是仿 xunit 风格，使用非面向对象的方式来创建夹具在实际的运行过程中，所有的写法都会统一处理成 fixture，**建议一步到位直接写 fixture**

***

## 2. pytest常用插件

https://pypi.org/

### 1. pytest-html

用途：生成HTML测试报告  
文档：https://pytest-html.readthedocs.io/en/latest/installing.html  
安装：  

```
pip install pytest-html
```

配置：命令行参数  

```
--html=report.html --self-contained-html
```

### 2. pytest-xdist

用途：并发执行用例  
文档：https://pytest-xdist.readthedocs.io/en/stable/  

安装：  

```
pip install pytest-xdist
```

配置：命令行参数  

```
-n {0,1,2,3,...,n, auto}
```

注意：  
1. 多进程额外增加资源  
2. 多进程乱序  
3. 多进程竞争资源（-s失效）  
4. auto 自动判断进程数（CPU内核数）  

### 3. pytest-order

用途：定义用例的执行顺序  
文档：https://pytest-order.readthedocs.io/en/latest/  

安装：  

```
pip install pytest-order
```

配置：标记  
```python
@pytest.mark.order(5) # 后执行  
def test_abc():  
    pass  
@pytest.mark.order(1) # 先执行  
def test_bbc():  
    pass  
```

顺序规则：  
● 先执行有order的用例，再执行没有order的用例  
● 先执行order较小的用例，再执行order较大的用例  

- order全局生效，可以跨文件、跨目录

### 4. pytest-rerunfailures

用途：用例失败时自动重试

文档：https://github.com/pytest-dev/pytest-rerunfailures

安装：

```
pip install pytest-rerunfailures
```

配置：命令行参数

```
--reruns 5 --reruns-delay 1
```

### 5. pytest-result-log

用途：把用例的执行结果保存到日志文件

文档：https://mp.weixin.qq.com/s/f90fcj54pKvebnBahlllog

安装：

```
pip install pytest-result-log
```

配置：pytest.ini

```
log_file = ./pytest.log
log_file_level = info
log_file_format = %(levelname)-8s %(asctime)s [%(name)s:%(lineno)s] : % (message)s
log_file_date_format = %Y-%m-%d %H:%M:%S

；记录用例执行结果
result_log_enable = 1
；记录用例分割线
result_log_separator = 1
；分割线等级
result_log_level_separator = warning
；异常信息等级
result_log_level_verbose = info
```

pycharm插件：Ideolog（日志颜色）

### 6. allure-pytest

用途：生成allure数据文件

文档：https://docs.gameta.io/allure-report/# pytest

安装：

```
pip install allure-pytest
```

配置：命令行参数

```
--alluredir=temps --clean-alluredir
```

本插件只生成数据，不生成报告：
1. 创建目录 temps
2. 清空目录内容
3. 在目录中创建数据文件

***

## 3. 定制企业级的测试报告

allure 是一个专业测试报告框架，是一个Java程序
allure-pytest 是一个pytest插件，是一个python程序

allure-pytest > 数据文件 > allure > 测试报告

### 1. 搭建allure环境

1. JDK

​	下载地址：https://www.oracle.com/java/technologies/downloads/#jdk17-windows

​	安装：双击运行、安装、重启
​	版本：建议 JDK 17+

​	验证：

```
java --version
```

2. allure源程序

​	下载地址：https://github.com/allure-framework/allure2/releases

​	解压：E:\abc\allure-2.24.1\allure-2.24.1\bin

​	修改环境变量：PATH

​	验证：allure
​	E:\abc\allure-2.24.1\allure-2.24.1\bin\allure

### 2. 生成企业级测试报告

generate  根据数据生成HTML报告
open  打开生成的HTML报告
serve  生成并打开HTML报告

serve = generate + open

```
E:\abc\allure-2.24.1\allure-2.24.1\bin\allure generate -o report temps # 根据数据生成HTML报告
E:\abc\allure-2.24.1\allure-2.24.1\bin\allure open report # 打开生成的HTML报告
```

```
allure generate -o report temps # 根据数据生成HTML报告
allure open report # 打开生成的HTML报告
```

### 3. 定制报告内容

#### 1. 功能分组

通过装饰器，对用例进行分组

```python
@allure.epic
@allure.feature
@allure.story
@allure.title
```

敏捷开发的水语：

- epic 史诗
- feature 主题
- story 故事
- title 标题

allure仅建立行为层次结构（另一种是基于套件）

```python
@allure.epic('码前自动化测试项目')
@allure.feature('模块A')
@allure.story('文件上传功能')
@allure.title('上传失败')
def test_abc():
    logger.info('11111')

@allure.epic('码前自动化测试项目')
@allure.feature('模块A')
@allure.story('文件上传功能')
@allure.title('上传成功')
def test_baa():
    logger.info('22222')

@allure.epic('码前自动化测试项目')
@allure.feature('模块B')
@allure.story('充值体现功能')
@allure.title('充值成功')
def test_bbc():
    logger.info('444444')
```

#### 2. 自定义logo

1. 确定插件的名称 `custom-logo-plugin`

2. 修改配置文件: "E:\abc\allure-2.24.l\allure-2.24.l\config\allure.yml"

3. 加入新的插件名称，启用插件

4. 在插件中修改logo:
    "E:\abc\allure-2.24.l\allure-2.24.l\plugins\custom-logo-plugin\static\styles.css"

```css
.side-nav__brand{
    background: url('logo.png') no-repeat left center !important;
    margin-left: 22px;
    height: 90px;
    background-size: contain !important;
}

.side-nav__brand-text{
    display: none;
}
```

### 总结：配置文件

run.py

```python
import pytest
import os
pytest.main()  # 启动测试框架, 自动生成allure数据
os.system('allure generate -c -o report  temps')  # 手动生成allure报告
```

pytest.ini

```ini
[pytest]

addopts = --alluredir=temps  --clean-alluredir  tests/homework/test_2_1.py

markers =
  api
  ui
  ut
  e2e

log_file = ./logs/pytest.log
log_file_level = info
log_file_format = %(levelname)-8s %(asctime)s [%(name)s:%(lineno)s]  : %(message)s
log_file_date_format  = %Y-%m-%d %H:%M:%S


; 记录用例执行结果
result_log_enable = 1
; 记录用例分割线
result_log_separator = 1
;分割线等级
result_log_level_separator  = warning
;异常信息等级
result_log_level_verbose = info

disable_test_id_escaping_and_forfeit_all_rights_to_community_support = 1
```

### 将常用包打包成 requirements.txt

如果你希望保持每个项目独立，但又不想手动一个个安装依赖，可以：

在原项目中导出依赖：

```bash
pip freeze > requirements.txt
```

新建项目后，在终端运行：

```bash
pip install -r requirements.txt
```

这样就能快速恢复所有依赖。

# 三、接口自动化基础之Pytest框架之YAML详解以及Parametrize数据驱动(day15)

## 1. YAML语法详解

YAML是一个完全兼容JSON的数据格式。

**重点：**  
1. **YAML完全兼容JSON**  
2. YAML和JSON一样，是数据，不是语句  
3. 序列化：将编程语言中的数据转为文件  
4. 反序列化：将文件中的内容转为编程语言中的数据  
5. **文本文件：** 可以使用记事本之类的工具进行创建、编辑  

**YAML优点：**  
1. 结构更加清晰  
2. 语法更加简洁，支持注释  
3. 和Python风格相似  

### 1. 序列化：Python转YAML

**将Python数据转为JSON文件：**  

https://tw.unicodery.com/5b57.html

```python
data = {
    "数字": [1, -1, 1.2],
    "字符串": ["1", '-1', """1.2"""],
    "布尔值": [True, False],
    "空值": None,
    '列表': [[1, 2, 3], [-1, -2, -3]],
    "字典": [{"a": 1}, {"b": 2}],
}

import json

s = json.dumps(data, ensure_ascii=False)  # Python转JSON字符串,且不将非 ASCII 字符转义为 Unicode 转义序列
#dumps是从字符串中加载数据，dump是从文件中加载数据
with open("data.json", "w", encoding="utf-8") as f:
    f.write(s)  # 创建JSON文件
```

**将Python数据转为YAML文件：**  
```bash
pip install pyyaml
```

```python
data = {
    "数字": [1, -1, 1.2],
    "字符串": ["1", '-1', """1.2"""],
    "布尔值": [True, False],
    "空值": None,
    '列表': [[1, 2, 3], [-1, -2, -3]],
    "字典": [{"a": 1}, {"b": 2}],
}

import yaml

s = yaml.safe_dump(data, allow_unicode=True, sort_keys=False)  # Python转YAML字符串

with open("data.yaml", "w", encoding="utf-8") as f:
    f.write(s)  # 创建YAML文件
```

### 2. 反序列化：YAML转Python

**将JSON转为Python：**  
```python
import json

with open("data.json", encoding="utf-8") as f:
    s = f.read()  # 得到字符串
    data_json = json.loads(s)  # 得到数据
#loads是从字符串中加载数据，load是从文件中加载数据
print(data_json)  # Python数据
```

**将YAML转为Python：**  
```python
import yaml

with open("data.yaml", encoding="utf-8") as f:
    s = f.read()  # 得到字符串
    data_yaml = yaml.safe_load(s)  # 得到数据（安全加载）

print(data_yaml)  # Python数据
```

### 3. YAML特色

1. 完全兼容JSON  
2. 支持注释（使用`#`）  
3. 成员通过符号表示：  
   - `-`：表示列表（数组）成员  
   - `:`：表示字典（对象）成员  
4. 通过缩进（2个空格）表示层级（Python使用4个空格）  
5. 自动处理类型  
6. 支持强制指定类型  

---

## 2. pytest + YAML实现数据驱动测试

数据驱动测试（Data Driver Test）= 参数化测试（pytest内置标记）+ 数据文件（YAML、JSON）

### 1. 参数化测试用例

```python
import pytest

def add(a, b):
    return a + b

@pytest.mark.parametrize(
    "a, b, c",
    [
        (1, 1, 2),
        (2, 2, 4),
        (3, 3, 6),
    ]
)
def test_add(a, b, c):
    assert add(a, b) == c
```

### 2. 数据内容委托到独立文件

```python
import pytest
import yaml

def add(a, b):
    return a + b

with open("ddt_data.yaml", encoding="utf-8") as f:
    s = f.read()  # 字符串
    data_yaml = yaml.safe_load(s)  # 数据

@pytest.mark.parametrize(
    "a, b, c",
    data_yaml
)
def test_add(a, b, c):
    assert add(a, b) == c
#同样的代码，同样的变量名，不会互相影响    
with open("data.yaml", encoding="utf-8") as f:
    s = f.read() # 字符串
	data_yaml = yaml.safe_load(s) # 数据
@pytest.mark.parametrize(
	"s",
	data_yaml
)
def test_add_str(s):
	pass
```

**YAML文件内容（ddt_data.yaml）：**  

```yaml
- [1, 1, 2]
- [2, 2, 4]
- [3, 3, 6]
- [a, b, c]
- [a, 1, 2]
```

---

### 3. 自动化测试框架结构

pytest是通用的测试框架，适用于：  
- 白盒测试  
- 单元测试  
- 集成测试  
- 黑盒测试（API、Web、App）  

**文件路径获取：**  

![](https://cdn.jsdelivr.net/gh/SereinCease/images/blog/2025-06-30/20250630121339-ddcff7.png)

![](https://cdn.jsdelivr.net/gh/SereinCease/images/blog/2025-06-30/20250630121506-795a4e.png)

**通用的黑盒测试框架：**  

```
commons/      # 常用代码目录
data/         # 数据目录
logs/         # 日志目录
report/       # 测试报告目录
temps/        # allure临时数据目录
tests/        # 测试用例目录
conftest.py   # pytest动态配置、共享fixture
pytest.ini    # 配置文件
run.py        # 框架启动文件
```

























