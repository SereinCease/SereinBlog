---
title: requests框架笔记
date: 2025-06-30 12:45:23
tags: 接口自动化
categories: 软件测试
cover:
description:
---

# 四、接口自动化测试框架之requests详解（day16）

## 1. 市面上主流的接口自动化测试方案

1. **基于工具的接口测试**  
   - Postman: JavaScript  
   - JMeter: Java  

2. **基于代码的接口测试**  
   - Python + pytest + requests （+ YAML + logging + allure + git + jenkins）  

3. **基于平台的接口测试**  
   - 前端: Vue  
   - 后端:  
     - Python: Django  
     - Java: SpringBoot  

---

## 2. HTTP接口协议

### 1. 什么是接口

**API（Application Programming Interface）**：  
- 一个程序和另一个程序的数据交互方式（序列化和反序列化）。  

**API测试**：  
- 一个程序对另一个程序的测试。  
- 涉及数据的序列化与传输。  

**Restful接口**：  

- Postman将数据按JSON序列化，通过HTTP协议传输到Nginx的80端口。  

**RPC接口**：  
- RPC Client将数据按二进制序列化，通过TCP协议传输到RPC Server的8123端口。  

**Windows接口**：  
- Win32程序将数据二进制序列化，通过Windows事件总线传输到Windows进程。  

---

### 2. HTTP协议

1. 发送请求：客户端 -> 服务器
2. 回复响应： 服务器 -> 客户端

**请求和响应的组成**：  
- **行**：数据第一行。  
- **头**：正文之前的内容。  
- **正文（体，Body）**：主要数据内容。  

#### 1. 请求  
**行**： 请求方法 路径（协议、主机、路径）版本号

```
GET https://www.baidu.com/ HTTP/1.1
```

**常见请求方法**：  

- `GET`：获取资源。  
- `POST`：创建资源。  
- `DELETE`：删除资源。  
- `PUT`：修改资源。  
- `OPTIONS`：查询接口信息。  

**头**：  
- 键值对形式，数量、长度、名字不限，必须是ASCII。  
示例：  

```
beifan: www.baidu.com
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML,
like Gecko) Chrome/124.0.0.0 Safari/537.36
Accept:
text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image
/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
sec-ch-ua: "Chromium";v="124", "Google Chrome";v="124", "Not-A.Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: zh-CN,zh;q=0.9
Cookie: BAIDUID_BFESS=3E8FB44D82CFDB0B6F04524F2264D9D6:FG=1; BIDUPSID
```

头与正文间有两个空行

**正文**：  

- 可以是空、表单（键值对）、JSON字符串或二进制数据（图片、视频等）。  

#### 2. 响应  
**行**：  版本号 状态码 状态描述

```
HTTP/1.1 200 OK
```

**常见状态码**：  

- `1xx`：继续请求。  
- `2xx`：请求成功。  
- `3xx`：重定向（无正文）。  
- `4xx`：客户端错误（请求问题）。  
- `5xx`：服务器端错误（接口问题）。  

**头**：  

```
Connection: keep-alive
Content-Security-Policy: frame-ancestors 'self' https://chat.baidu.com
http://mirror-chat.baidu.com https://fj-chat.baidu.com https://hba-chat.baidu.com
https://hbe-chat.baidu.com https://njjs-chat.baidu.com https://nj-chat.baidu.com
https://hna-chat.baidu.com https://hnb-chat.baidu.com http://debug.baidu-int.com;
Content-Type: text/html; charset=utf-8
Date: Sun, 21 Apr 2024 12:26:53 GMT
Server: BWS/1.1
Set-Cookie: H_PS_PSSID=40366_40379_40301_40511_40080_60132; path=/; expires=Mon,
21-Apr-25 12:26:53 GMT; domain=.baidu.com
Traceid: 1713702413079251559415571183161739773267
X-Ua-Compatible: IE=Edge,chrome=1
X-Xss-Protection: 1;mode=block
Content-Length: 406773
```

**正文**：  

- 可以是空、文本（HTML、TXT）、JSON字符串或二进制数据。

<!DOCTYPE html><!--STATUS OK--><html><head><meta http-equiv="Content-Type"
content="text/html;charset=utf-8"><meta http-equiv="X-UA-Compatible"
content="IE=edge,chrome=1"><meta content="always" name="referrer"><meta
name="theme-color" content="#ffffff"><meta name="description" content="全球领先的中
文搜索引擎、致力于让网民更便捷地获取信息，找到所求。百度超过千亿的中文网页数据库，可以瞬间找到相关
的搜索结果。"><link rel="shortcut icon" href="https://www.baidu.com/favicon.ico"
type="image/x-icon" /><link rel="search"
type="application/opensearchdescription+xml" href="/content-search.xml" title="百
度搜索" /><link rel="icon" sizes="any" mask
href="https://www.baidu.com/favicon.ico"><link rel="dns-prefetch"
href="//dss0.bdstatic.com"/><link rel="dns-prefetch" href="//dss1.bdstatic.com"/>
<link rel="dns-prefetch" href="//ss1.bdstatic.com"/><link rel="dns-prefetch"
href="//sp0.baidu.com"/><link rel="dns-prefetch" href="//sp1.baidu.com"/><link
rel="dns-prefetch" href="//sp2.baidu.com"/><link rel="dns-prefetch"
href="//pss.bdstatic.com"/>  

---

## 3. requests用法

### 1. 安装  
```bash
pip install requests
```

```
pip show requests
```

### 2. 发送HTTP请求  

**多种使用方式**：  
```python
requests.get('http://www.baidu.com/')
requests.request('get', 'http://www.baidu.com/')

sess = requests.session()
sess.get('http://www.baidu.com/')
sess.request('get', 'http://www.baidu.com/')
```

**统一内部原理**：  
```python
sess = requests.session()  # 实例化类
sess.request('get', 'http://www.baidu.com/')  # 调用实例方法
```

**参数说明**：  
- **行**：  
  - `method`：请求方法（如`GET`）。  
  - `url`：接口地址。  
  - `params=None`：查询字符串。  
- **头**：  
  - `headers=None`：请求头。  
  - `cookies=None`：Cookies。  
- **正文**：  
  - `data=None`：表单参数。  
  - `files=None`：文件（二进制）。  
  - `json=None`：JSON参数。  
- **其他**：    
  - `timeout=None`, # 超时时间
  - `allow_redirects=True`, # 跟随重定向
  - `proxies=None`, # 代理设置
  - `hooks=None`, # 内部钩子
  - `stream=None`, # 流式传输
  - `verify=None`, # 验证HTTPS证书
  - `cert=None`, # 自定义HTTPS证书

**示例**：  

```python
import requests

resp = requests.request(
    # 行
    method='get',
    url="https://www.baidu.com/upload",
    params={"dir": "user_home"},
    #### 头
    headers={"name": "beifan"},
    #### 正文
    data={"name": "北凡"},  # 表单数据
    json={"age": [1, 1, 2]},
    files={"file": open("conftest.py", "rb")}  # 二进制模式打开文件
)
```
**注意**：`data`和`json`不能共存。  

### 3. 解析响应  
```python
import requests

resp = requests.request('get', 'https://api.weixin.qq.com/cgi-bin/token')

# 行
print(resp.status_code, resp.reason)#状态码和状态描述

# 头
print(resp.headers)

# 正文
print(resp.text)      # 文本内容（人类可读）
print(resp.content)   # 二进制内容（适合下载）
print(resp.json())    # 将JSON文本反序列化为字典
```

---

## 4. 接口自动化实战

### 1. 接口文档  
- 地址：`http://47.107.116.139/showdoc/web/?#/1`  
- 需要密码（找班主任索取）。  

### 2. 基本流程  
1. **看懂文档**：  
   - 接口数量、项目风格、接口具体信息（请求四要素：方法、地址、参数、鉴权或依赖）（响应：状态码、正文、错误码、错误提示）。  
   - 业务需求：何时、为何请求接口。  
2. **设计用例**：  
   - 前置条件、用例参数、预期结果。  
3. **编写与执行用例**：  
   - 使用Python + pytest + requests。  
4. **输出报告**：  
   - 日志与测试报告。  

### 3. 微信公众号项目  

```python
import random
import time

import requests
import json

g_var = {}
# 1.获取鉴权码access token接口
def test_get_token():
    res = requests.request(
        method='get',
        url="https://api.weixin.qq.com/cgi-bin/token",
        params={
            "grant_type": "client_credential",
            "appid": "",
            "secret": ""
        }
    )

    assert res.status_code == 200
    access_token = res.json()["access_token"]
    assert access_token != ""
    g_var["access_token"]= access_token # 保存变量，为了其他接口使用


# 2.获取公众号已创建的标签接口
def test_get_tags():
    res = requests.request(
        method='get',
        url="https://api.weixin.qq.com/cgi-bin/tags/get",
        params={
            "access_token": g_var["access_token"]
        }
    )

    assert res.status_code == 200
    tags = res.json()["tags"]
    assert tags
    assert tags[0]['id'] == 2

# 3.创建标签接口
def test_create_tags():
    timestamp = str(time.time())
    res = requests.request(
        method='post',
        url="https://api.weixin.qq.com/cgi-bin/tags/create",
        params={
            "access_token": g_var["access_token"]
        },
        json={"tag":{"name":"熊" + timestamp}}
    )

    assert res.status_code == 200
    s = res.text.replace("\\\\","\\")
    res_json = json.loads(s)#替换后手动进行反序列化，将字符串转换为json
    name = res_json["tag"]['name']
    id = res_json["tag"]['id']
    g_var['tag_id'] = id
    assert name == "熊" + timestamp
    assert isinstance(id,int)
    
    

# 4.编辑标签接口
def test_edit_tags():
    timestamp = str(time.time())
    res = requests.request(
        method='post',
        url="https://api.weixin.qq.com/cgi-bin/tags/update",
        params={
            "access_token": g_var["access_token"]
        },
        json={"tag": {"id":g_var['tag_id'],"name":"happy"+timestamp}}
    )

    assert res.status_code == 200
    assert res.json()['errcode'] == 0
    assert res.json()['errmsg'] == 'ok'

# 5.删除标签接口
def test_del_tags():
    res = requests.request(
        method="post",
        url="https://api.weixin.qq.com/cgi-bin/tags/delete",
        params={
            'access_token':g_var['access_token']
        },
        json= {"tag":{"id":g_var["tag_id"]}}
    )
    assert res.status_code ==200
    assert res.json()['errcode'] == 0
    assert res.json()['errmsg'] == 'ok'

# 6.文件上传接口
def test_file_upload():
    res = requests.request(
        method="post",
        url="https://api.weixin.qq.com/cgi-bin/media/uploadimg",
        params={
            'access_token': g_var['access_token']
        },
        files={
            "media":open("data/shu.png","rb")
        }
    )
    assert res.status_code == 200
    url = res.json()['url']
    assert 'http' in url
    assert 'mmbiz.qpic.cn' in url
```

创建接口时断言发生问题

```
AssertionError: assert '\\u718a1751596032.4772298' == '熊1751596032.4772298'
```

```python
print(res.content)
print(res.text)
print(res.json())
#打印出的结果
{"tag":{"id":194,"name":"\\\\u718a1751596032.4772298"}}
{"tag":{"id":194,"name":"\\u718a1751596032.4772298"}}
{'tag': {'id': 194, 'name': '\\u718a1751596032.4772298'}}
```

使用``s.replace("\\\\","\\")` 将\\\u718a1751596032.4772298变为\\u718a1751596032.4772298，就变成了Unicode编码格式

# 五、接口自动化测试框架之requests封装（day17）

## 1. 回顾项目特点

1. 大部分的用例由以下几步骤  
   - 发送请求  
   - 提取数据  
   - 断言数据  
2. 大部分的用例需要相同参数值（身份凭据）  

**封装目的**：通过隐藏细节减少重复步骤，降低使用难度，增加新特性。

---

## 2. 封装请求类

### 1. HTTP报文日志
```python
import requests  
import logging  

logger = logging.getLogger('request_utils')  # 日志记录器  

class RequestUtils:  
    sess = requests.Session()  # 实例化Session  

    def send_request(self, **kwargs):   # 统一参数类型，仅限关键字参数
        logger.info('正在发送请求...')  
        for k, v in kwargs.items():  
            logger.info(f'	参数内容: {k}={v}')  

        resp = self.sess.request(**kwargs)  # 发送请求  # 参数长度、内容是不确定
        logger.info('收到接口响应')  
        logger.info(f'状态码={resp.status_code}')  
        logger.info(f'响应头={resp.headers}')  
        logger.info(f'	响应正文={resp.text}')  
        return resp
```

### 2. 自动添加公共参数
```python
class RequestUtils:
    sess = requests.Session()  # 实例化
    public_params = {}  # 公共参数字典  

    def send_request(self, **kwargs):  # 统一参数类型，仅限关键字参数
        logger.info('正在发送请求...')
        for k, v in kwargs.items():  
            if k == 'params':
                v.update(self.public_params) # 合并参数内容，把public_params合并到params中
            logger.info(f'	参数内容: {k}={v}')  
```

### 3. 简化文件上传
```python
class RequestUtils:
    sess = requests.Session() # 实例化
    pubilc_params = {}
    def send_request(self, **kwargs):
        # 统一参数类型，仅限关键字参数
        logger.info('正在发送请求...')
        for k, v in kwargs.items():
            if k == 'params':
                v.update(self.pubilc_params) # 使用属性中的字典，修改本次参数
            elif k == 'files':
                for name, file in v.itemn():
                    v[name] = open(file, "rb") # 二进制方式打开文件
```

## 3. 使用YAML数据驱动测试

参数和返回值，结构相似的情况下，才适合数据驱动测试

- 创建标签
- 编辑标签

### 完整代码

打开数据文件封装函数

```python
def to_yaml(path):
    with open(path,encoding="utf-8") as f:
        s = f.read()
        data_yaml = yaml.safe_load(s)
    return data_yaml

```

使用`ddt_creat_tag.yaml`和`ddt_edit_tag_fail.yaml`分别对创建标签和编辑标签进行数据驱动测试

`request_utils.py`代码

```python
import requests
import logging

logger = logging.getLogger("request_utils") #日志记录器

class RequestUtils:
    sess = requests.session()
    pubilc_params = {}  # 定义公共参数
    opened_files=[] #创建一个打开文件的列表，用于接口请求完成后关闭文件
    def send_request(self,**kwargs):
        logger.info('正在发送请求...')
        params=kwargs.get('params',{}) #使用get方法，当不存在params时会创建params且默认值为{}
        params.update(self.pubilc_params) # 使用属性中的字典，修改本次参数
        kwargs['params'] = params #当参数中没有params时需要添加params键来更新kwargs
        files = kwargs.get('files',{})
        for name, file_path in files.items():
            if isinstance(file_path, str):
                file_obj = open(file_path, "rb")
                files[name] = file_obj
                self.opened_files.append(file_obj) #写入列表
        kwargs['files'] = files
        for k, v in kwargs.items():
            logger.info(f'  参数内容: {k}={v}')
        res = self.sess.request(**kwargs)  # 发送请求  # 参数长度、内容是不确定
        logger.info('收到接口响应')
        logger.info(f'  状态码={res.status_code}')
        logger.info(f'  响应头={res.headers}')
        logger.info(f'  响应正文={res.text}')
        for f in self.opened_files:
            f.close()
        return res
```

测试用例代码

```python
import logging
import time
import pytest

from commons.request_utils import RequestUtils
from commons.aaa import to_yaml
import json

#g_var = {} #全局变量函数
logger = logging.getLogger('ddt')
ddt_create_tag = to_yaml('data/ddt_creat_tag.yaml')
logger.info(f'data_ddt_create_tag={ddt_create_tag}')
ddt_edit_tag_fail = to_yaml('data/ddt_edit_tag_fail.yaml')
logger.info(f'ddt_edit_tag_fail={ddt_edit_tag_fail}')
# 创建fixture，在所有用例结束后执行，用于删除标签
@pytest.fixture(scope='session')
def del_tags():

    yield
    print('所有用例都执行完毕，开始删除测试数据')
    logger.info('所有用例都执行完毕，开始删除测试数据')
    res = RequestUtils().send_request(
        method='get',
        url="https://api.weixin.qq.com/cgi-bin/tags/get",

    )
    tags = res.json()['tags'] #返回数据先转换为json，获取到tags列表
    for tag_name in ddt_create_tag: #外层循环，遍历已经创建的标签名
        tagId = '000' #设定了一个默认值
        for tag in tags: #内层循环，遍历tags列表
            if tag_name == tag['name']: #名字与创建的标签名匹配就获取id值
                tagId = tag['id']
        if tagId == '000': #如果是默认值就跳过本次循环
            continue
        RequestUtils().send_request(
            method="post",
            url="https://api.weixin.qq.com/cgi-bin/tags/delete",

            json={"tag": {"id": tagId}}
        )


# 1.获取鉴权码access token接口
def test_get_token():
    res = RequestUtils().send_request(
        method='get',
        url="https://api.weixin.qq.com/cgi-bin/token",
        params={
            "grant_type": "client_credential",
            "appid": "wx180cd14b59813610",
            "secret": "0a0ac08da6958e499c4f8695db3f7697"
        }
    )

    assert res.status_code == 200
    access_token = res.json()["access_token"]
    RequestUtils.pubilc_params['access_token'] = access_token
    # g_var["access_token"]= access_token # 保存变量，为了其他接口使用
    assert access_token != ""


# 2.获取公众号已创建的标签接口
def test_get_tags():
    res = RequestUtils().send_request(
        method='get',
        url="https://api.weixin.qq.com/cgi-bin/tags/get",

    )

    assert res.status_code == 200
    tags = res.json()["tags"]
    assert tags
    assert tags[0]['id'] == 2


# 3.创建标签接口
# 参数化
@pytest.mark.parametrize(
    "name",
    ddt_create_tag
)
def test_create_tags(name,del_tags):
    timestamp = str(time.time())
    res = RequestUtils().send_request(
        method='post',
        url="https://api.weixin.qq.com/cgi-bin/tags/create",

        json={"tag": {"name": name}}
    )

    assert res.status_code == 200
    s = res.text.replace("\\\\", "\\")
    res_json = json.loads(s)  # 替换后手动进行反序列化，将字符串转换为json
    tag_name = res_json["tag"]['name']
    tag_id = res_json["tag"]['id']
    g_var['tag_id'] = tag_id
    assert tag_name == name
    assert isinstance(tag_id, int)


# 4.编辑标签接口
def test_edit_tags():
    timestamp = str(time.time())
    res = RequestUtils().send_request(
        method='post',
        url="https://api.weixin.qq.com/cgi-bin/tags/update",

        json={"tag": {"id": g_var['tag_id'], "name": "学习" + timestamp}}
    )

    assert res.status_code == 200
    assert res.json()['errcode'] == 0
    assert res.json()['errmsg'] == 'ok'

@pytest.mark.parametrize(
    "name,code",
    ddt_edit_tag_fail
)
def test_edit_tags_fail(name,code):

    res = RequestUtils().send_request(
        method='post',
        url="https://api.weixin.qq.com/cgi-bin/tags/update",

        json={"tag": {"id": g_var['tag_id'], "name": name }}
    )

    assert res.status_code == 200
    assert res.json()['errcode'] == code




# 5.删除标签接口
def test_del_tags():
    res = RequestUtils().send_request(
        method="post",
        url="https://api.weixin.qq.com/cgi-bin/tags/delete",

        json={"tag": {"id": g_var["tag_id"]}}
    )
    assert res.status_code == 200
    assert res.json()['errcode'] == 0
    assert res.json()['errmsg'] == 'ok'


# 6.文件上传接口
def test_file_upload():
    res = RequestUtils().send_request(
        method="post",
        url="https://api.weixin.qq.com/cgi-bin/media/uploadimg",

        files={
            "media": "data/shu.png"
        }
    )
    assert res.status_code == 200
    url = res.json()['url']
    assert 'http' in url
    assert 'mmbiz.qpic.cn' in url

```



# 六、接口自动化测试框架之电商接口项目实战（day18）

## 1. 电商接口项目实战

### 1. 接口约定
**基础URL**： 
`http://116.62.63.211/shop/api.php`  

**查询字符串参数**：  
| 参数名                  | 说明                          | 必填 |
|-------------------------|-------------------------------|------|
| `s`                     | 接口名称                      | 是   |
| `application`           | 请求应用（web/app）           | 是   |
| `application_client_type`| 客户端类型（ios/android/weixin/alipay） | 是   |
| `token`                 | 身份凭据                      | 否   |
| `ajax`                  | Web端异步请求标识             | 否   |

**参数**：JSON  

**响应**：JSON（包含字段：`code`, `msg`, `data`）  

---

### 2. 获取Token（其他用例的依赖）
```python
from commons.request_utils import RequestUtils
g_var = {
    "url": "http://116.62.63.211/shop/api.php"
}
def test_get_token():
    res = RequestUtils().send_request(
        method="post",
        url=g_var['url'],
        params={
            "s": "user/login",
            "application": "app",
            "application_client_type": "ios"
        },
        json={
            "accounts": "lyreth",
            "pwd": "123456",
            "type": "username"
        }
    )
    token = res.json()["data"]["token"]
    code = res.json()['code']
    RequestUtils.pubilc_params = {
        "application": "app",
        "application_client_type": "ios",
        "token": token
    }
    assert code == 0
    assert token != ""
```

---

### 3. 商品收藏功能测试用例
#### (1) 收藏商品
```python
def test_goods_favor():
    res = RequestUtils().send_request(
        method="post",
        url=g_var['url'],
        params={
            "s": "goods/favor",
        },
        json={
            "id": "12"，
            'is_mandatory_favor': 1
        }
    )
    code = res.json()['code']
    msg = res.json()['msg']
    assert code == 0
    assert msg == '收藏成功'
```

#### (2) 验证收藏列表
```python
def test_usergoodsfavor_index_after_favor():
    resp = RequestUtils().send_request(
        method="post",
        url=g_var['url'],
        params={"s": "usergoodsfavor/index"}
    )
    
    code = resp.json()['code']
    text = resp.text
    assert code == 0
    assert '"goods_id":"2"' in text  # 验证商品ID=2存在
```

#### (3) 取消收藏
```python
def test_usergoodsfavor_cancel():
    resp = RequestUtils().send_request(
        method="post",
        url=g_var['url'],
        params={"s": "usergoodsfavor/cancel"},
        json={"id": "2"}
    )
    
    code = resp.json()['code']
    msg = resp.json()['msg']
    assert code == 0
    assert msg == '取消成功'
```

#### (4) 验证取消后收藏列表
```python
def test_usergoodsfavor_index_after_cancel():
    resp = RequestUtils().send_request(
        method="post",
        url="http://101.34.221.219:8010/api.php",
        params={"s": "usergoodsfavor/index"}
    )
    
    code = resp.json()['code']
    text = resp.text
    assert code == 0
    assert '"goods_id":"2"' not in text  # 验证商品ID=2已移除
```

目前为主：
学习了：python、pytest、reqeusts
封装了：requests日志记录、公共参数、文件上传
实战了：微信接口项目、电商接口项目
应用了：fixture删除测试数据、变量接口关联、yaml数据驱动测试
输出了：log日志文件、allure测试报告
实现：python=pytest+requests+yaml+logging+allure接口自动化测试
