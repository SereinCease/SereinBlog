---
title: 数据结构知识点
date: 2024-03-05 19:38:12
tags: 数据结构
categories: 408
cover: 
description: 数据结构中非代码知识点总结
---

# 第一章绪论

## 1.基本概念

时间复杂度

空间复杂度

# 第三章栈，队列和数组

1.数组和特殊矩阵

一维数组的存储结构：a[i]=LOC+i*sizeof(ElemType) （下标从0开始）

二维数组按行优先存储结构：

b[i] [j]=LOC+(i*N+j)**sizeof(ElemType)  (M行N列)

2.特殊矩阵的矩阵压缩

采用矩阵压缩的目的：减少不必要的存储空间

(1)对称矩阵

(2)三角矩阵

(3)三对角矩阵

3.稀疏矩阵

稀疏矩阵压缩后失去了随机存储特性

**适用于稀疏矩阵的两种存储结构：三元组表和十字链表**

稀疏矩阵的特点是矩阵中非零元较少

主要考点为计算数组下表，存储地址

# 第五章树与二叉树

## 5.1树的基本概念

1.树的性质

1.结点数=总度数+1

2.度为m的树，m叉树的区别

3.度为m的树第i层至多有m^i-1^个结点（i>=1）

4.高度为h的m叉树至多有m^h-1^/m-1个结点



## 5.3二叉树的遍历和线索二叉树

1.由遍历序列构成二叉树

先+中

后+中

层+中

# 第六章图

## 6.3图的遍历

广度优先搜索BFS

使用辅助队列，先进先出

类似于树的层次遍历

各边的权值相等时，可解决单源最短路径问题

# 第七章查找

![](https://cdn.jsdelivr.net/gh/SereinCease/images/blog/2024-03-14/20240314131102-b846fe.png)
