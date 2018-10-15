---
title: dp 常见模板
tags: [ACM,dp]
---
### 最大上升子序列
```
LIS(LDS)
template<class Cmp>  
int LIS (Cmp cmp)(nlogn)  
{  
    static int m, end[N];  
    m = 0;  
    for (int i=0;i<n;i++)  
    {  
        int pos = lower_bound(end, end+m, a[i], cmp)-end;  
        end[pos] = a[i], m += pos==m;  
    }  
    return m;  
}  
    cout << LIS(less<int>()) << endl;         //严格上升  
    cout << LIS(less_equal<int>()) << endl;   //非严格上升  
    cout << LIS(greater<int>()) << endl;      //严格下降  
    cout << LIS(greater_equal<int>()) << endl;//非严格下降  
```
### 最大公共子序列
```
留个递推公式,差不多的意思(n^2)

   dp[i][j]=dp[i-1][j-1]+1;
   dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
   dp[0][0]=0;

```
### 前向星造图法
```cpp
struct EDGE{          //图的单位,边的结构体
  int to;             //儿子
  int next;           //同父的下一个结点
  int w;              //权
}edge[maxn*2];

void addedge(int u,int v,int w) //加边
{
  edge[tot].to = v;
  edge[tot].w = w;
  edge[tot].next = head[u];
  head[u] = tot++;
}

void dfs(u,v)//遍历的规则,单边
{
   for(int i =head[u];i!=-1;i = edge[i].next)
   {
     int v = edge[i].to;
     int w = edge[i].w;
     伪代码区
   }  
}
void dfs(int u,int father)//双边
{
  for(int i = head[u];i!=-1;i=edge[i].next)
    {
      int v = edge[i].next;
      int w = edge[i].w;
      if(v != father)
      {
        伪代码
      }
    }
}   

```
### 什么是开挂
```cpp
poj 3107 不开 500ms
         开了 188ms
         类型应该是何前面的(int) 相对应
inline int read()//加快输入输出
{
    char k=0;char ls;ls=getchar();for(;ls<'0'||ls>'9';k=ls,ls=getchar());
    int x=0;for(;ls>='0'&&ls<='9';ls=getchar())x=x*10+ls-'0';
    if(k=='-')x=0-x;return x;
}
```
### 状态压缩里面常用的一些东西
```cpp
//1 获得当前行的数
int getnum(int x)
{
  int ret = 0;
  while(x)
  {
    x &= x-1;
    ret++;
  }
  return ret;
}
//2 看当前行左右是不是满足题设
bool check(int x)
{
  if(x & x<<1) return 0;
  return 1;
}
// 看是不是可以满足条件,和题目给的图一样,是可以放的,并且和上一个是不是会冲突
bool suit(int x,int y)
{
  if(x&y) return 0;
  return 1;
}

```
### 数位dp
```cpp
// 1 把各个数位上的数字存进去
ll dfs(int len(数位),....(中间各个东西的转化),flag(是否在最高位,要限制))
{
  if(len<0) return ..;
  if(!flag && ~dp[len][...]) return dp[len][...];
  ll ans= 0;
  int Max = flag?shu[len]:9;
  for(int i = 0;i<=Max;i++)
   {
     ans += dfs(len-1,...,flag&&i==Max);
   }
   if(!flag) dp[len][...] = ans;
   return ans;
}
void slove(ll x)
{
  int co = 0;
  while(x)
  {
    shu[co] = x%10;
    x/=10;
  }
  return dfs(co-1,...,1);
}
这是主要的一个套路;

```
