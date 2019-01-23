#  [Codeforce-1059-E. Split the Tree-树上倍增](http://codeforces.com/contest/1059/problem/E)



## 【Description】

```c++
You are given a rooted tree on n vertices, its root is the vertex number 1. The i-th 
vertex contains a number wi. Split it into the minimum possible number of vertical 
paths in such a way that each path contains no more than L vertices and the sum of 
integers wi on each path does not exceed S. 
Each vertex should belong to exactly one path.

A vertical path is a sequence of vertices v1,v2,…,vk
where vi (i≥2) is the parent of vi−1.
```

## 【Input】

```c++
The first line contains three integers n, L, S (1≤n≤10^5, 1≤L≤10^5, 1≤S≤10^18) — the 
number of vertices, the maximum number of vertices in one path and the maximum 
sum in one path.

The second line contains n integers w1,w2,…,wn (1≤wi≤109) — the numbers in the vertices 
of the tree.

The third line contains n−1 integers p2,…,pn (1≤pi<i), where pi is the parent of the i-
th vertex in the tree.
```

## 【Output】

```c++
Output one number  — the minimum number of vertical paths. If it is impossible to split 
the tree, output −1.
```

------



## 【Examples】 

#### Sample Input

```c++
3 1 3
1 2 3
1 1
```

#### Sample Output

```c++
3
```
#### Sample Input

```c++
3 3 6
1 2 3
1 1
```

#### Sample Output

```c++
2
```
#### Sample Input

```c++
1 1 10000
10001
```

#### Sample Output

```c++
-1
```

------



## 【Problem Description】

```c++
给你一棵树，每个节点有个权值wi，将这棵树分为k条垂直路径，在保证路径节点权值和不超过S并且路径长度不超过L
的情况下，使k最小。
```

## 【Solution】

```c++
贪心，倍增
1、对每个节点预处理出向上伸展的 满足题目条件的最长路径的长度f[i]
2、dfs一遍，从子孙节点开始向上，对每个节点，求其所有子孙节点f[i]的最大值Max，若Max为0，则ans加1.

对于第一步的预处理，需要用到倍增算法：
例如一棵树：1-2，2-3，3-4，4-5.并记fa[i][j]表示第i个节点的第2^j的祖先为fa[i][j];
则对于每个节点的直接父节点即为fa[i][0]=i-1;
那么就可以求得每个节点的祖先节点就为其直接父节点的直接父节点即 fa[i][1]=fa[ fa[i][0] ][0];
类似的fa[i][j]=fa[ fa[i][j-1] ][j-1];所以fa数组就可以在O(nlog(n))的时间复杂度内求得。
fa数组记录的就是每个节点的1，2，4，8，16辈的祖先是哪个节点。

然后根据二进制的特性，任意一辈的祖先节点都可以通过组合来求得。例21=10101=1+0+4+0+16
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-01-15 21:26:01 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-01-15 22:01:06
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int w[maxn],fa[maxn][40],va[maxn][40];//每个节点的权值，第i个节点的第2^j个祖先节点，第i个节点到其第2^j的祖先节点的权值和
vector<int>g[maxn];//邻接表存储
int ans=0,f[maxn];//每个节点满足条件的最长路径长度
void dfs1(int u,int p){//预处理每个节点的直接父节点
    for(auto v:g[u]){
        if(v!=p){
            fa[v][0]=u;
            va[v][0]=w[u];
            dfs1(v,u);
        }
    }
}
int dfs2(int u,int p){
    int Max=0;
    for(auto v:g[u]){
        Max=max(Max,dfs2(v,u)); //求其所有子孙节点的最大f[i]
    }
    if(!Max){
        ans++;
        return f[u]-1;
    }
    return Max-1;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,L,S;scanf("%lld%lld%lld",&n,&L,&S);
    for(int i=1;i<=n;i++){
        scanf("%lld",w+i);
        if(w[i]>S) return puts("-1"),0;
    }
    for(int i=2;i<=n;i++){
        int x;scanf("%lld",&x);
        g[x].push_back(i);
    }
    dfs1(1,-1);
    for(int i=1;(1<<i)<n;i++){//O(nlog(n))求出fa数组
        for(int j=1;j<=n;j++){
            fa[j][i]=fa[fa[j][i-1]][i-1];
            va[j][i]=va[fa[j][i-1]][i-1]+va[j][i-1];
        }
    }
    for(int i=1;i<=n;i++){//求得满足题目条件的最长路径的长度值f[i]
        int sum=w[i],now=i;f[i]=1;
        for(int j=19;~j&&sum<=S&&f[i]<=L;j--){
            if(fa[now][j]&&sum+va[now][j]<=S&&f[i]+(1<<j)<=L){
                sum+=va[now][j];f[i]+=(1<<j);now=fa[now][j];
            }
        }
    }
    dfs2(1,-1);
    printf("%lld\n",ans);
    cin.get(),cin.get();
    return 0;
}
```
