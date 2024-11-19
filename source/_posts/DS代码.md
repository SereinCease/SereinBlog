---
title: 代码题
date: 2024-01-23 18:19:08
tags: 数据结构
categories: 408
cover: https://cdn.jsdelivr.net/gh/SereinCease/images/blog/2024-02-01/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240201122417-ae9943.jpg
---

## 顺序表的定义

```c++
#define MaxSize 50
#define InitSize 100;
typedef int ElemType;
//静态分配
typedef struct {
    ElemType data[MaxSize];
    int length;
}SqList;
//动态分配
typedef struct {
    ElemType *data;
    int MaxSize;//最大容量
    int length;
}SeqList;
```

## 顺序表的初始化

```c++
//静态分配初始化
void InitList(SqList &L){
    L.length=0;
}

//动态分配初始化
void InitList(SeqList &L){
    L.data=(ElemType *)malloc(MaxSize*sizeof(ElemType));
    L.length=0;
    L.MaxSize=InitSize;
}
```



## 顺序表的插入，插入i位置的元素

```c++
bool ListInsert(SqList &L,int i,ElemType element){
    //判断插入位置是否合法
    if(i<1||i>L.length+1)
        return false;
    //判断顺序表是否满了
    if(L.length>=MaxSize)
        return false;
    //顺序表插入操作
    for (int j = L.length; j >=i; j--) {
        L.data[j]=L.data[j-1];
    }
    L.data[i-1]=element;
    L.length++;
    return true;
}
```

## 顺序表的删除，删除i位置的元素

```c++
//顺序表的删除，输入删除元素的位置，返回删除元素的值
bool ListDelete(SqList &L,int i,ElemType e){
    if(i<1||i>L.length)
        return false;
    e=L.data[i-1];
    for (int j = i; j < L.length; j++) {
        L.data[j-1]=L.data[j];
    }
    L.length--;
    return true;
}
```

## 按值查找

```c++
//顺序表的查找
int LocateElem(SqList L,ElemType e){
    for (int i = 0; i < L.length; i++) {
        if(L.data[i]==e)
            return i+1;
        return 0;
    }
}
```





## 打印顺序表

```c++
//打印顺序表
void PrintList(SqList L){
    for (int i = 0; i < L.length; i++) {
        printf("%4d",L.data[i]);
    }
    printf("\n");
}
```

## 有序表插入仍然保持有序(升序）

```c++
int List_insert(SqList &L,ElemType e){
    int i;
    for ( i = 0; i < L.length; i++) {
        if(e<L.data[i])
            break;
    }
    for (int j =L.length; j>=i+1 ; j--) {
        L.data[j]=L.data[j-1];
    }
    L.data[i]=e;
    L.length++;
    return i+1;
}
```





单链表结构体定义

```c
#include <stdio.h>
#include <stdlib.h>
//链表练习
typedef int ElemType;
typedef struct LNode{
    ElemType data;
    struct LNode *next;//定义指针域
}LNode,*LinkList;


```

单链表遍历输出

```c
void ListPrint(LNode* L){
    L=L->next;//从第一个结点开始遍历
    while(L!=NULL){
        printf("%3d",L->data);
        L=L->next;
    }
    printf("\n");
}
```

单链表按位查找

```c
LinkList serach_i(LinkList L, int i){
    if(i<1)
        return NULL;//判断插入位置是否合法
    LNode *p=L->next;//从第一个结点开始遍历
    int k=1;
    while (p!=NULL&&k<i){
        p=p->next;
        k++;
    }
    return p;
}
```

单链表按值查找

```c
LNode *search_e(LinkList L, int e){
    LNode *p=L->next;
    while(p!=NULL&&(p->data!=e){
        p=p->next;
    }
    return p;
}
```

头插法

```c
//头插法
LinkList List_HeadInsert(LinkList &L){
    L=(LNode*)malloc(sizeof (LNode));
    L->next=NULL;//头结点初始化;
    LNode *s;//定义一个指针指向新节点
    int x;//记录新节点的值
    scanf("%d",&x);//输入新节点的值
    while (x!=9999){
        s=(LNode*)malloc(sizeof (LNode));
        s->data=x;
        s->next=L->next;
        L->next=s;
        scanf("%d",&x);
    }
    return L;
}

```

尾插法

```c
//尾插法
LinkList  List_TailInsert(LinkList &L){
    L=(LNode*)malloc(sizeof (LNode));
    L->next=NULL;//头结点初始化;
    LNode *s,*r=L;
    int x;
    scanf("%d",&x);
    while(x!=9999){
        s=(LNode*)malloc(sizeof (LNode));
        s->data=x;
        r->next=s;
        r=s;
        scanf("%d",&x);
    }
    r->next=NULL;
    return L;
}
```



main函数

```c
int main() {
    LNode* L;
    List_HeadInsert(L);
    ListPrint(L);
    List_TailInsert(L);
    ListPrint(L);
    return 0;
}
```

