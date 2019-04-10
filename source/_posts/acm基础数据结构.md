---
title: acm基础数据结构
date: 2019-4-8 12:00:01
tags: [acm,数据结构，基本算法]
---

### 唉
> 前期靠板子，后者两行泪
面试好像拿板子，确实是一件不太对的事，唉，反正面试也就哪几个简单的数据结构，最近就把常见的数据结构，和常见的图论算法，背一下，唉，酷比，我为啥要学数学。。。文章写的都是一些简单的东西，大家随便看看，退役选手，码力下降，智商下降。

### 数据结构

#### 树状数组

> 用法在于单点更新，区间查询，这里的单点更新是单点加减，换的话好像不是不行
这个算法是利用了一个二进制的操作，只要记得要初始化就行。
```cpp
int tree[maxn]
int lowbit(int x)
{
    return x&(-x);
}
void add(int st,int flag) // st这个位置加flag的值
{
    for(int i = st;i<=n;i+=lowbit(i))
    {
        tree[i] += flag;
    }
}
int getsum(int st)  // 前st的数的和，包括st
{
    int res = 0;
    for(int i = st;i>0;i-=lowbit(i))
    {
        res += tree[i];
    }
    return res;
}
```

#### 单调栈
这个我觉得碰到的机会不大，但是他们有人碰到了，我还是稍微看下。单调栈的用法怎么说呢，其实没那么玄学，它就是一个栈，只是里面的数据是单调的，一般用来维护的是左边第一比他小或者大的。下面的是一个例子，用来找从左到右第一个比他小的地方。

```cpp
// 这里就手动写栈了
int tot = -1;
int shu[maxn]; // 数据
int st[maxn];  // 这个是模拟的栈
int next[maxn];
ans[++tot] = 1; 
for(int i = 2;i<=n;i++)
{
    if(shu[i] < shu[st[tot]] )
    {
        while(tot>=0 && shu[i]>shu[st[tot]])
        {
            tot--;
        }    
    }
    if(tot == -1)
    {
        next[i] = -1;
        st[++tot] = i;
    }
    else
    {
        next[i] = st[tot];
        st[++tot] = i;
    }
}
```

#### 单调队列
这个是用来干啥呢，用来维护的是n个数，对每m个区间，最大的数，或最小的数，复杂度降到线性的一个问题。对于找m个区间里面的最大值的话，就是做一个递减队列，就是更新的时候不仅要注意最大最小的比较还要注意一下是否超过了这个区间的大小。感觉有点像那个斜率dp的一个处理方式。
```cpp
void getmax()
{
    int i,head=1,tail=0;
    for(i=1;i<m;i++)
    {
        while(head<=tail && v[tail].x<=a[i]) tail--;
        v[++tail].x=a[i],v[tail].y=i;
    }
    for(;i<=n;i++)
    {
        while(head<=tail && v[tail].x<=a[i]) tail--;
        v[++tail].x=a[i],v[tail].y=i;
        while(v[head].y<i-m+1) head++;
        mx[i-m+1]=v[head].x;
    }
}
```
### 算法

这个模块的东西稍微多点，唉，随便默写一下把。
#### 常规dp
```cpp


```