#  [BZOJ-2115-Xor-异或线性基-求图上路径异或和最大值](https://www.lydsy.com/JudgeOnline/problem.php?id=2115)



## 【Description】

```cpp
考虑一个边权为非负整数的无向连通图，节点编号从1到n,试求出一条从1号节点到n号节点的路径，使得路径上经过的边
的权值的异或和最大。
路径可以重复经过某些点或边，当一条边在一条路径中出现多次时，其边权在计算异或和时也要被计算相应多的次数。
```

## 【Input】

```cpp
第一行包含两个整数N和 M， 表示该无向图中点的数目与边的数目。 接下来M 行描述 M 条边，每行三个整数Si，Ti
，Di，表示 Si 与Ti之间存在 一条权值为 Di的无向边。 图中可能有重边或自环。
```

## 【Output】

```cpp
仅包含一个整数，表示最大的XOR和（十进制结果）,注意输出后加换行回车。
```

------



## 【Examples】 

#### Sample Input

```cpp
5 7 
1 2 2 
1 3 2 
2 4 1 
2 5 1 
4 5 3 
5 3 4 
4 3 2 
```

#### Sample Output

```cpp
6
```

------



## 【Problem Description】

```cpp
无
```

## 【Solution】

```cpp
结论：1到n的路径异或和最大值等于 任意一条1到n的路径的异或和与图中的一些环的组合。
原因：
	假设1到n只有一条路径，那么答案就是所有边的异或和。
	如果1到n有多条路径，又因为是无向图，所以这些路径一定能组合成很多环。而如果要求1到n的最长异或路径，就要
多走一些环，具体走哪些环就是线性基来解决了。（每个环只能走一次，由于异或的性质，走两次就相当于没走过。
	那为什么可以先任意选择一条1到n的路径呢？ 若任意选择的这条路径不是最优路径，那么这条路径会和最优路径组
成一个环，当走过一遍这个环后，就想当于选择了最优路径。
	那么最后解决方案就出来了，就是将图中所有的环和任意一条1到n的路径的异或值求出来，利用线性基求最大值。
	求所有环的异或值可以用dfs直接求得。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-02-28 19:21:23 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-02-28 22:16:40
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 50005
#define maxm 100005
struct node{
    int u,v,w,next;
    node(){}
    node(int _u,int _v,int _w,int _next):u(_u),v(_v),w(_w),next(_next){}
}g[maxm<<1];
int head[maxm],cnt=0,a[maxn],d[maxn];
bool vis[maxn];
void init(){
    memset(head,-1,sizeof(head));
    memset(vis,0,sizeof(vis));cnt=0;
    memset(a,0,sizeof(a));
}
void addedge(int u,int v,int w){ //建立邻接表
    g[cnt]=node(u,v,w,head[u]);
    head[u]=cnt++;
}
void insert(int val){ //线性基的插入
    for(int i=60;i>=0;i--){
        if(val&(1LL<<i)){
            if(!a[i]){a[i]=val;break;}
            else val^=a[i];
        }
    }
}
void dfs(int u){ //求所有环的异或值
    vis[u]=1;
    for(int i=head[u];~i;i=g[i].next){
        if(!vis[g[i].v]) d[g[i].v]=d[u]^g[i].w,dfs(g[i].v);
        else insert(d[u]^g[i].w^d[g[i].v]); //如果当前点访问过，则说明形成环，将其异或值插入线性基
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    init();
    int n,m;scanf("%lld%lld",&n,&m);
    for(int i=1;i<=m;i++){
        int u,v,w;scanf("%lld%lld%lld",&u,&v,&w);
        addedge(u,v,w);addedge(v,u,w); 
    }
    dfs(1);
    int ans=d[n];//任意一条1到n的路径的异或值
    for(int i=60;i>=0;i--) if((ans^a[i])>ans) ans^=a[i]; //求最大值
    printf("%lld\n",ans);
    cin.get(),cin.get();
    return 0;
}
```
