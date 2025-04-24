---
title: html+css学习
date: 2024-12-26 14:18:37
tags: 
categories: 前端
cover:
description:
---

# HTML基础

 跳转到锚点

第一步：设置锚点

```html
<!-- 第一种方式：a标签配合name属性 -->
<a name="test1"></a>
<!-- 第二种方式：其他标签配合id属性 -->
<h2 id="test2">我是一个位置</h2>
```

第二步：跳转锚点

```html
<!-- 跳转到test1锚点-->
<a href="#test1">去test1锚点</a>
<!-- 跳到本页面顶部 -->
<a href="#">回到顶部</a>
<!-- 跳转到其他页面锚点 -->
<a href="demo.html#test1">去demo.html页面的test1锚点</a>
<!-- 刷新本页面 -->
<a href="">刷新本页面</a>
<!-- 执行一段js,如果还不知道执行什么，可以留空，javascript:; -->
<a href="javascript:alert(1);">点我弹窗</a>
```

有序列表

```
<h2>要把大象放冰箱总共分几步</h2>
<ol>
<li>把冰箱门打开</li>
<li>把大象放进去</li>
<li>把冰箱门关上</li>
</ol>
```

无序列表

```
<ul>
    <li>深圳</li>
    <li>上海</li>
    <li>北京</li>
</ul>
```

列表嵌套

```
<h2>我想去的几个城市</h2>
<ul>
	<li>成都</li>
	<li>
		<span>上海</span>
		<ul>
			<li>外滩</li>
			<li>杜莎夫人蜡像馆</li>
			<li>
				<a href="https://www.opg.cn/">东方明珠</a>
			</li>
			<li>迪士尼乐园</li>
		</ul>
	</li>
	<li>西安</li>
	<li>武汉</li>
</ul>
```

自定义列表

一个 dl 就是一个自定义列表，一个 dt 就是一个术语名称，一个 dd 就是术语描述（可以有多 个）

```
<h2>如何高效的学习？</h2>
<dl>
	<dt>做好笔记</dt>
	<dd>笔记是我们以后复习的一个抓手</dd>
	<dd>笔记可以是电子版，也可以是纸质版</dd>
	<dt>多加练习</dt>
	<dd>只有敲出来的代码，才是自己的</dd>
	<dt>别怕出错</dt>
	<dd>错很正常，改正后并记住，就是经验</dd>
</dl>
```

