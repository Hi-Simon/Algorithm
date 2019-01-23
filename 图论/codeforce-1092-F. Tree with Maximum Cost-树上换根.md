#  [codeforce-1092-F. Tree with Maximum Cost-树上换根](http://codeforces.com/contest/1092/problem/F)



## 【Description】

```cpp
You are given a tree consisting exactly of n vertices. Tree is a connected undirected
graph with n−1 edges. Each vertex v of this tree has a value avassigned to it.

Let dist(x,y)be the distance between the vertices x and y. The distance between the 
vertices is the number of edges on the simple path between them.

Let's define the cost of the tree as the following value: firstly, let's fix some 
vertex of the tree. Let it be v. Then the cost of the tree is ∑i=1ndist(i,v)⋅ai.

Your task is to calculate the maximum possible cost of the tree if you can choose v 
arbitrarily.
```

## 【Input】

```cpp
The first line contains one integer n, the number of vertices in the tree (1≤n≤2⋅105).

The second line of the input contains nintegers a1,a2,…,an (1≤ai≤2⋅105), where ai is 
the value of the vertex i

Each of the next n−1lines describes an edge of the tree. Edge i is denoted by two 
integers ui and vi, the labels of vertices it connects (1≤ui,vi≤n, ui≠vi).

It is guaranteed that the given edges form a tree.
```

## 【Output】

```cpp
Print one integer — the maximum possible cost of the tree if you can choose any vertex
as v.
```

------



## 【Examples】 

#### Sample Input

```cpp
8
9 4 1 7 10 1 6 5
1 2
2 3
1 4
1 5
5 6
5 7
5 8
```

#### Sample Output

```cpp
121
```
#### Sample Input

```cpp
1
1337
```

#### Sample Output

```cpp
0
```
------



## 【Problem Description】

```cpp
给你一棵树，树上每个节点的权值为a[i],定义dist(x,y)为x到y的边数。
再定义一个节点v，v为树上任意的一个节点,定义这棵树的花费为val=∑dist(i,v),i从1到n。
求这棵树的最大花费。
```

## 【Solution】

![img](http://codeforces.com/predownloaded/02/70/027013a6626a457eff9fea288b5c614682f5f0e9.png)

```cpp
树形dp
定义sum[i]为以i为根节点的子树的权值和(包括i),dp[i]为以i为根节点的整棵树的花费。
假设已知sum[]数组和dp[1]的值，那么我们可以求得根节点1的所有直接子节点的值dp[j]。

对于上图假设j=5,则dp[5]=dp[1]+(sum[1]-sum[5])-sum[5]
其中sum[1]-sum[5]=a[1]+a[2]+a[3]+a[4],sum[5]=a[5]+a[6]+a[7]+a[8];
因为对于1，2，3，4这四个节点来说，当根节点从1换为5时，他们到根节点的dist都增加了1，所以对于整棵树来说
花费增加了sum[1]-sum[5];
对于5，6，7，8这四个节点来说类似，当根节点从1换为5时，他们到根节点的dist都减少了1，所以对于整棵树来说
花费减少了sum[5]

至于sum数组和dp[1]可以通过dfs直接求得。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-01-16 21:58:37 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-01-16 22:18:33
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 200005
int a[maxn],dp[maxn],sum[maxn];//节点权值，dp[i]表示以i为根节点的花费，sum[i]表示以i为根节点的子树的权值和
vector<int>g[maxn];//邻接表
int dfs1(int u,int p){//处理出sum数组和dp[1]
    sum[u]=a[u];
    for(auto v:g[u]){
        if(v!=p){
            dfs1(v,u);
            sum[u]+=sum[v];
            dp[u]+=dp[v]+sum[v];
        }
    }
}
int ans=dp[1];
int dfs2(int u,int p){//从根节点开始，对于其每个子节点，进行换根
    for(auto v:g[u]){
        if(v!=p){
            dp[v]=dp[u]+(sum[1]-sum[v])-sum[v];
            dfs2(v,u);
        }
    }
    // cout<<sum[1]<<' '<<u<<' '<<dp[u]<<endl;
    ans=max(ans,dp[u]);
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;scanf("%lld",&n);
    for(int i=1;i<=n;i++){
        scanf("%lld",a+i);
    }
    for(int i=1;i<n;i++){
        int u,v;scanf("%lld%lld",&u,&v);
        g[u].push_back(v);
        g[v].push_back(u);
    }
    dfs1(1,-1); dfs2(1,-1);
    printf("%lld\n",ans);
    cin.get(),cin.get();
    return 0;
}
```
