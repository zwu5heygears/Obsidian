# List
## 顺序存储线性表
```cpp
#define MAXSIZE 20
typedef int ElemType;
typedef struct{
    ElemType data[MAXSIZE];  // 数组结构
    int length; // 长度
}SqList;
// 访问
int GetElem(SqList L, int i, ElemType *e){
    if(L.length = = 0 || i < 0 || i > L.length - 1)
        return 0;
    *e = L.data[i];
    return 1;
}
// 插入
int ListInsert(SqList *L, int i, ElemType e){
    int k;
    if(L->length == MAXSIZE || i < 0 || i > L->length - 1)
        return 0;
    if(i <= L->length -1){
        for(k = L->length - 2; k>=i;k--){
            L->data[k+1] = L->data[k];
        }
    }
    L->data[i] = e;
    L->length++;
    return 1;
}
// 删除
int ListDelete(SqList *L, int i, ElemType *e){
    if(L->length == 0 || i > L->length -1 || i < 0)
        return 0;
    *e = L->data[i];
    int k;
    if(i <= L->length - 1){
        for(k = i; k < L->length - 2; k++){
            L->data[k-1] = L->data[k];
        }
    }
    L->length--;
    return 1;
}
```
## 链式存储线性表
```cpp
/*
"链式存储链表  指针结构
规定头结点为-1节点且不存储数值（一般存储长度）
head->next开始为下标0且存储data
*/
#include <iostream>
using namespace std;
// 结构
typedef struct Node 
{ 
    int data;
    struct Node *next;
}*Linklist, Node;
// 查询
int GetElem(Linklist L, int i, int *e){
    int j = 0;
    Linklist p; // 节点
    p = L->next; // p指向头结点下一节点
    while(p && j < i){
        p = p->next;
        ++j;  
    }
    if(!p || j > i)
        return 0;
    *e = p->data;
    return 1;
}
// 插入
int ListInsert(Linklist *L, int i, int e){ // 传递的是指针的指针（要改变指针而不是指针的值）
    int j = 0;
    Linklist p;
    p = *L;
    while(p && j < i){
        p = p->next;
        ++j;
    }
    if(!p || j > i)
        return 0;
    Linklist s = new Node;
    s->data = e;
    s->next = p->next;
    p->next = s;
    return 1;
}

// 删除
int ListDelete(Linklist *L, int i, int *e){
    int j = 1;
    Linklist p = *L;
    Linklist q;
    while (p->next && j < i){
        p = p->next;
        ++j;
    }
    if(!p || j > i)
        return 0;
    q = p->next;
    p->next = q->next;
    *e = q->data;
    delete q;
    return 1;
}
  
// 创建链表头插法
void CreatListHead(Linklist *L, int n){
    Linklist p;
    int i;
    (*L) = new Node;  // 形参已经定义了不能重复定义！
    (*L)->next = nullptr;
    for(int i = 0; i < n; ++i){
        p = new Node;
        cin >> p->data;
        p->next = (*L)->next;
        (*L)->next = p;
    }
}

// 创建链表 尾插法
void CreateListTail(Linklist *L, int n){
    Linklist p, r;
    int i;
    (*L) = new Node;
    r = *L;
    while(i < n){
        p = new Node;
        cin >> p->data;
        r->next = p;
        r = p;
        ++i;
    }
    r->next = nullptr;
}

// 删除链表
int ClearList(Linklist *L){
    Linklist p, q;
    p = (*L)->next;
    while(p){
        q = p->next;
        delete p;
        p = q;
    }
    (*L)->next = nullptr; // 初始节点指向空
    return 1;
}
```
## 循环链表
```cpp
typedef struct DulNode
{
    int data;  
    struct DulNode *prior;
    struct DulNode *next;
}DuleNode, *DuLinklist;
```
## 静态链表
```cpp
typedef struct StaticLinkList
{
    int data;
    int cur;
}Component, StaticLinkList[maxnum];
// 初始化
int InitList(StaticLinkList space){
    for(int i = 0; i < maxnum; i++){
        space[i].cur = i + 1;
    }
    space[maxnum - 1].cur = 0;
    return 0;
}

// 获取空闲分量的首个下标
int Malloc_SSL(StaticLinkList space){
    int i = space[0].cur;
    if(space[0].cur){
        space[0].cur = space[i].cur;
    }
    return i;
}
// 回收节点
void Free_SSL(StaticLinkList L, int k){
    L[k].cur = L[0].cur;
    L[0].cur = k;
}

// 获取静态链表长度
int ListLength(StaticLinkList L){
    int j = 0;
    int i = L[maxnum - 1].cur;
    while(i){
        i = L[i].cur;
        j ++;
    }
    return j;
}

// 插入
int InsertList(StaticLinkList L, int i, int e){
    int j, k, l;
    k = maxnum - 1;
    if(i < 1 || i > ListLength(L) + 1){
        return 0;
    }
    j = Malloc_SSL(L);
    if(j){
        L[j].data = e;
        for(l = 1; l <= i - 1; l++){
            k = L[k].cur;
        }
        L[j].cur = L[k].cur;
        L[k].cur = j;
        return 1;
    }
    return 0;
}
// 删除
int ListDelete(StaticLinkList L, int i){
    int j, k;
    if(i < 1 || i > ListLength(L)){
        return 0;
    }
    k = maxnum - 1;
    for(j = 1; j <= i - 1; j++){
        k = L[k].cur;
    }
    j = L[k].cur;
    L[k].cur = L[j].cur;
    Free_SSL(L, j);
    return 1;
}
```
# Stack
## 顺序栈
 - 栈
 - 顺序共享栈
 - 链栈
```cpp
const int maxnum = 100;
typedef struct {
    int data[maxnum];
    int top; // 栈顶指针
}SeqStack;

// 入栈
int Push(SeqStack *S, int e){
    if(S->top == maxnum - 1){
        return 0;
    }
    S->top++;
    S->data[S->top] = e;
    return 1;
}
// 出栈
int Pop(SeqStack *S, int *e){
    if(S->top == -1){
        return 0;
    }
    *e = S->data[S->top];
    S->top--;
    return 1;
}

/* 2 顺序共享栈 */
// 结构
typedef struct {
    int data[maxnum];
    int top1;
    int top2;
}SqDoubleStack;
// 入栈
int PushDouble(SqDoubleStack *S, int e, int stackNum){
    if(S->top1 + 1 == S->top2)
        return 0;
    if(stackNum == 1){
        S->data[++S->top1] = e; // 先赋值给top1然后自加1
    }
    else if (stackNum == 2){
        S->data[--S->top2] = e; // 先赋值top2然后自减
    }
    return 1;
}

// 出栈
int PopDouble(SqDoubleStack *S, int *e, int stackNum){
    if(stackNum == 1){
        if(S->top1 == -1)
            return 0;
        *e = S->data[--S->top1];
    }
    else if (stackNum == 2)
    {
        if(S->top2 == maxnum)
            return 0;
        *e = S->data[++S->top2];
    }
    return 1;
}

/* 3 链栈 */
// 结构
typedef struct StackNode
{
    int data;  // 栈值
    struct StackNode *next; // 下一节点
}StackNode, *LinkStackPtr; // 栈的节点指针
typedef struct
{
    LinkStackPtr top;  // 栈顶节点和数量
    int count;
}LinkStack;

// 进栈
int PushLinkStack(LinkStack *S, int e){
    LinkStackPtr s = new StackNode;
    s->data = e;
    s->next = S->top;
    S->top = s;
    S->count++;
    return 1;
}

// 出栈
int PopLinkStack(LinkStack *S, int *e){
    LinkStackPtr p;
    if(S->count == 0)
        return 0;
    *e = S->top->data;
    p = S->top;
    S->top = S->top->next;
    delete p;
    S->count--;
    return 1;
}
```
# Queue
## 顺序队列
```cpp
/* 1 顺序队列 */
// 结构
const int maxnum = 100;
typedef struct
{
    int data[maxnum];
    int front;
    int rear;
}SqQueue;

// 初始化队列
int InitQueue(SqQueue *Q){
    Q->front = 0; // 头
    Q->rear = 0; // 尾
    return 1;
}
// 获取队列长度
int QueueLength(SqQueue Q){
    return (Q.rear - Q.front + maxnum) % maxnum;
}

// 入队
int EnQueue(SqQueue *Q, int e){
    if((Q->rear + 1) % maxnum == Q->front) // 队列已满
        return 0;
    Q->data[Q->rear] = e;
    Q->rear = (Q->rear + 1) % maxnum;
    return 1;
}

// 出队
int DeQueue(SqQueue *Q, int *e){
    if(Q->front == Q->rear) // 队列为空
        return 0;
    *e = Q->data[Q->front];
    Q->front = (Q->front + 1) % maxnum; // 头指针移后
    return 1；
}
```
## 链式队列
```cpp
/* 2 链式队列 */
// 结构
typedef struct QNode
{
    int data;
    struct QNode *next;
}QNode;
typedef struct{

    QNode  *front, *rear;

}LinkQueue;

// 入队
int EnLinkQueue(LinkQueue *Q, int e){
    QNode *s = new QNode;
    s->data = e;
    s->next = nullptr;
    Q->rear->next = s;
    Q->rear = s;
    return 1;
}
// 出队
int DeLinkQueue(LinkQueue *Q, int *e){
    QNode *p;
    if(Q->front == Q->rear)
        return 0;
    p = Q->front->next;
    *e = p->data;
    Q->front->next = p->next;
    if(Q->rear == p)
        Q->rear = Q->front;
    delete p;
    return 1;
}
```

# String

# Tree

# Map

