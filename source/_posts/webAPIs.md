---
title: webAPIs
date: 2025-02-10 19:15:16
tags: JavaScript
categories: 前端
cover:
description:
---

# Web APIs

### day2

## 事件监听

完成事件监听分成3个步骤：

1. 获取 DOM 元素
2. 通过 `addEventListener` 方法为 DOM 节点添加事件监听
3. 等待事件触发，如用户点击了某个按钮时便会触发 `click` 事件类型
4. 事件触发后，相对应的回调函数会被执行

#### 点击按钮弹出对话框

```js
<script>
    // 需求：点击按钮，弹出对话框
    // 事件源：按钮
    // 事件类型：点击
    // 事件处理程序：弹出对话框
    const btn  = document.querySelector('button')
    btn.addEventListener('click', function(){
      alert('你好呀')
    })
  </script>
```

#### 关闭广告

```js
<body>
   <button>点我</button>
  <div class="box">我是广告
    <div class="box1">X</div>
  </div>
  <script>
    // 需求：点击x，关闭广告    
    const box  = document.querySelector('.box')
    const box1  = document.querySelector('.box1')   
    box1.addEventListener('click', function(){
    	box.style.display = 'none'
    })
  </script>   
</body>
```

