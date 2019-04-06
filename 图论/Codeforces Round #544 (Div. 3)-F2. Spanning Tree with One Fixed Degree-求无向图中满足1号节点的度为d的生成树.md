# [Codeforces Round #544 (Div. 3)-F2. Spanning Tree with One Fixed Degree-求无向图中满足1号节点的度为d的生成树](https://codeforces.com/contest/1133/problem/F2)

## 【Description】

```cpp
You are given an undirected unweighted connected graph consisting of n vertices 
and m edges. It is guaranteed that there are no self-loops or multiple edges in 
the given graph.

Your task is to find any spanning tree of this graph such that the degree of the 
first vertex (vertex with label 1 on it) is equal to D (or say that there are no 
such spanning trees). Recall that the degree of a vertex is the number of edges 
incident to it.
```

## 【Input】

```cpp
The first line contains three integers n, m and D (2≤n≤2⋅105, n−1≤m≤min(2⋅105,n
(n−1)2),1≤D<n) — the number of vertices, the number of edges and required degree 
of the first vertex, respectively.

The following m lines denote edges: edge i is represented by a pair of integers 
vi, ui (1≤vi,ui≤n, ui≠vi), which are the indices of vertices connected by the 
edge. There are no loops or multiple edges in the given graph, i. e. for each 
pair (vi,ui) there are no other pairs (vi,ui) or (ui,vi) in the list of edges, 
and for each pair (vi,ui) the condition vi≠ui is satisfied.
```

## 【Output】

```cpp
If there is no spanning tree satisfying the condition from the problem statement,
print "NO" in the first line.

Otherwise print "YES" in the first line and then print n−1 lines describing the 
edges of a spanning tree such that the degree of the first vertex (vertex with 
label 1 on it) is equal to D. Make sure that the edges of the printed spanning 
tree form some subset of the input edges (order doesn't matter and edge (v,u) is 
considered the same as the edge (u,v)).

If there are multiple possible answers, print any of them.
```

------



## 【Examples】 

#### Sample Input

```cpp
4 5 1
1 2
1 3
1 4
2 3
3 4
```

#### Sample Output

```cpp
YES
2 1
2 3
3 4
```
#### Sample Input

```cpp
4 5 3
1 2
1 3
1 4
2 3
3 4
```

#### Sample Output

```cpp
YES
1 2
1 3
4 1
```
#### Sample Input

```cpp
4 4 3
1 2
1 4
2 3
3 4
```

#### Sample Output

```cpp
NO
```

------



## 【Problem Description】

```cpp
给你一个无向无权图，求满足1号节点的度为d的生成树（无向图节点的度定义为此点所连接的边数
```

## 【Solution】

```cpp
因为要求生成树的1号节点的度等于d，所以首先要判断一下原图的1号节点的度是否大于等于d

要得到1号节点的度为d的生成树，需要分两步做：
    1、若原图1号节点的度为D，则需要从1号节点中删除D-d条边，得到新图，并且保证新图连通。
    2、对新图1号节点bfs，直接得到生成树。

主要在于第一步：
    要保证新图连通，考虑求无向图的连通分量：
        从原图中，将1号节点及与其相连的边都删掉，然后求连通分量。对于每个连通分量，
        找一个点与1号节点相连形成新图

    做完之后，就保证了新图的连通性，若连通分量的个数为t个，则新图的1号节点的度就为t,所
    以需要判断t是否大于d。若大于d则不可行。

    此时新图的1号节点的度为t，还需要连接d-t条边。从还未与1号节点相连的边中任意连接d-t条
    即可。
第二步：
    直接bfs即可。
注意：与1号节点相连的边，都是在原图中已存在的边。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-04-01 15:43:05 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-04-01 17:57:29
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 200005
vector<int>g[maxn],g1[maxn];//g为原图，g1为新构造的图
int vis[maxn];
void bfs(int u,int t){ //对每个点求连通分量
    queue<int>q;q.push(u);
    vis[u]=t;
    while(!q.empty()){
        int now=q.front();q.pop();
        for(auto v:g[now]){
            if(vis[v]) continue;
            g1[now].push_back(v);g1[v].push_back(now); //将所有连通分量放在新图中
            vis[v]=t;if(v!=1) q.push(v);else vis[now]=INF; //保证每个连通分量与1相连的边只有一条
        }
    }
}
void bfs1(){ //对新图直接求生成树，即答案
    memset(vis,0,sizeof(vis));
    queue<int>q;q.push(1);
    vis[1]=1;
    while(!q.empty()){
        int u=q.front();q.pop();
        for(auto v:g1[u]){
            if(vis[v]) continue;
            cout<<u<<' '<<v<<endl;
            vis[v]=1;q.push(v);
        }
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m,d;cin>>n>>m>>d;
    for(int i=1;i<=m;i++){
        int u,v;cin>>u>>v;
        g[u].push_back(v);g[v].push_back(u); 
    }int t=1; //连通分量的个数
    for(int i=2;i<=n;i++){
        if(vis[i]) continue; //如果此节点已在其他连通分量里，则跳过
        vis[1]=0;bfs(i,t); //每次都要将vis[1]=0,这样可以保证bfs时每个连通分量都与1号节点连一条边
        t++; 
    }t--;
    if(g[1].size()<d||t>d){ //如果1号节点的度数小于要求的度数，或者，连通分量的个数大于要求的度数则不可行
        cout<<"NO"<<endl;  
        cin.get(),cin.get();
        return 0;
    }
    d-=t; //每个连通分量与1号节点连接1条边，总共连接了t条边，还需要连接d-t条边

    for(auto v:g[1]){
        if(vis[v]==INF) continue; //已经和1号节点连接的边
        if(!d) break;d--;
        g1[1].push_back(v);g1[v].push_back(1); //任意再连接d-t条边
    }
    cout<<"YES"<<endl; bfs1(); //对新图直接求生成树
    cin.get(),cin.get();
    return 0;
}
```
