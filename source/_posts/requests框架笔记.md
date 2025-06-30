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

```

```



















