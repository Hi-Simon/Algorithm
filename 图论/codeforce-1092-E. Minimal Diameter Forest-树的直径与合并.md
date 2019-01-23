#  [codeforce-1092-E. Minimal Diameter Forest-树的直径与合并](http://codeforces.com/contest/1092/problem/E)



## 【Description】

```cpp
You are given a forest — an undirected graph with n vertices such that each its 
connected component is a tree.

The diameter (aka "longest shortest path") of a connected undirected graph is the 
maximum number of edges in the shortest path between any pair of its vertices.

You task is to add some edges (possibly zero) to the graph so that it becomes a tree 
and the diameter of the tree is minimal possible.

If there are multiple correct answers, print any of them.
```

## 【Input】

```cpp
The first line contains two integers n and m (1≤n≤1000, 0≤m≤n−1) — the number of 
vertices of the graph and the number of edges, respectively.

Each of the next mlines contains two integers v and u (1≤v,u≤n, v≠u) — the descriptions
of the edges.

It is guaranteed that the given graph is a forest.
```

## 【Output】

```cpp
In the first line print the diameter of the resulting tree.

Each of the next (n−1)−m lines should contain two integers v and u (1≤v,u≤n, v≠u) — the
descriptions of the added edges.

The resulting graph should be a tree and its diameter should be minimal possible.

For m=n−1no edges are added, thus the output consists of a single integer — diameter of 
the given tree.

If there are multiple correct answers, print any of them.
```

------



## 【Examples】 

#### Sample Input

```cpp
4 2
1 2
2 3
```

#### Sample Output

```cpp
2
4 2
```
#### Sample Input

```cpp
2 0
```

#### Sample Output

```cpp
1
1 2
```
#### Sample Input

```cpp
3 2
1 3
2 3
```

#### Sample Output

```cpp
2
```
------



## 【Problem Description】

```cpp
首先定义一棵树的直径为 树上任意两点的最短距离中最长的那个值。
题意：给你一个森林，将森林中所有的树合并为一颗树，并使最终的这棵树的直径最短。
```

## 【Solution】

![graph](C:\Users\asus\Downloads\graph.png)

```cpp
在所有树中，找到直径最大的一个，然后将其他所有树的直径中点挂在直径最大的那棵树的直径中点即可。

现在问题有三个，1.对于每棵树求直径，2.对于每个直径找中点，3.合并所有的树
定义dia[i]结构体，包括直径值Max,以及此时的根Mvet;表示第i棵树的直接和此时的根。
定义vMax[i]数组，表示以i为根节点的子树的最大深度减一，例上图，vMax[2]=2;
定义gras[i]数组，表示i的儿子为gras[i],并且gras[i]在 以i为根节点的子树中 长度最长的路径上。
例：gras[7]=9;
对于vMax和gras数组，可以通过dfs直接求得。
1、求直径：
对于每棵树，若以1号节点在直径上，则包含1号节点的直径长度dia[i].Max=vMax[u]+vMax[v]+1;
v是u的一个直接子节点。
所以这棵树的最大直径，就可以遍历一遍这棵树每个节点j，求得包含j号节点的直径，最长的那个即dia[i].Max
而dia[i].Mvet=j;同时遍历整棵树的过程就是用dfs，所以可以与求vMax数组部分一起求。

2、对于每个直径找中点。
第1步，我们求得了每棵树的直径，以及在直径上的一个点。所以我们只要判断这个点的位置即可。
例直径为5，已知的直径上的点v=1，且vMax[1]=1,那么只需要用两次gras[]数组即可，即v=gras[v],v=gras[v]

3、合并
设总直径Maxdia=-INF
总共三个特判，
	1、当两棵树直径都为1的时候，总直径要加2， 比如1-2，3-4

	2、当两棵树的直径差小于等于1的时候，总直径要加1

	3、当两棵树直径都是奇数时，并且直径差为2时，总直径要加1
其他直接合并即可。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-01-17 15:30:26 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-01-17 22:09:43
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 1005
int a[maxn];
bool vis[maxn]; //标记是否访问过
vector<int>g[maxn];
struct node{
    int Max,Mvet;
}dia[maxn];//第i课树的直接，以及直径上的一个点
bool cmp(node a,node b){
    return a.Max>b.Max;
}
int gras[maxn];//节点i在最长路径上的儿子
int dianum=0;//总共dianum棵树
int vMax[maxn];//以i为根节点的子树的深度减一
int dfs(int u){
    vis[u]=1;
    for(auto v:g[u]){
        if(!vis[v]){
            dfs(v);
            if(vMax[u]+vMax[v]+1>dia[dianum].Max){//所有树的最大直径
                dia[dianum].Max=vMax[u]+vMax[v]+1;
                dia[dianum].Mvet=u;
            }
            if(vMax[v]+1>vMax[u]){//求vMax数组
                vMax[u]=vMax[v]+1;
                gras[u]=v;
            }
        }
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m;scanf("%lld%lld",&n,&m);
    for(int i=1;i<=m;i++){
        int u,v;scanf("%lld%lld",&u,&v);
        g[u].push_back(v);
        g[v].push_back(u);
    }
    for(int i=1;i<=n;i++){
        if(!vis[i]) dianum++,dia[dianum].Max=0,dia[dianum].Mvet=i,dfs(i);//遍历每棵树
    }
    sort(dia+1,dia+dianum+1,cmp);
    int Maxv=1,Maxdia=dia[1].Max;
    for(int i=1;i<=dianum;i++){
        int val=vMax[dia[i].Mvet];
        for(int j=val;j>(dia[i].Max+1)/2;j--){//找到每棵树直径的中点
            dia[i].Mvet=gras[dia[i].Mvet];
        }
        if(Maxv==i) continue;
        if(Maxdia==1&&dia[i].Max==1) Maxdia+=2;//三个特判
        else if (abs(dia[i].Max - Maxdia) <= 1) Maxdia +=1;
        else if(abs(dia[i].Max-Maxdia)==2&&(dia[i].Max&1)&&(Maxdia&1)) Maxdia++;
    }
    printf("%lld\n",Maxdia);
    for(int i=2;i<=dianum;i++){
        printf("%lld %lld\n",dia[Maxv].Mvet,dia[i].Mvet);
    }
    cin.get(),cin.get();
    return 0;
}
```
