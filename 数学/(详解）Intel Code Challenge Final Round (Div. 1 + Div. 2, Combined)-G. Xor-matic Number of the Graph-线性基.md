#  [(详解）Intel Code Challenge Final Round (Div. 1 + Div. 2, Combined)-G. Xor-matic Number of the Graph-线性基](http://codeforces.com/contest/1100/problem/F)



## 【Description】

```cpp
You are given an undirected graph, constisting of n vertices and m edges. Each edge of 
the graph has some non-negative integer written on it.
    
Let's call a triple (u, v, s) interesting, if 1 ≤ u < v ≤ n and there is a path (possibly non-
simple, i.e. it can visit the same vertices and edges multiple times) between vertices u
and v such that xor of all numbers written on the edges of this path is equal to s. When 
we compute the value s for some path, each edge is counted in xor as many times, as it 
appear on this path. It's not hard to prove that there are finite number of such triples.
    
Calculate the sum over modulo 109 + 7 of the values of s over all interesting triples.
```

## 【Input】

```cpp
The first line of the input contains two integers n and m (1 ≤ n ≤ 100 000, 0 ≤ m ≤ 200 000) —
numbers of vertices and edges in the given graph.
    
The follow m lines contain three integers ui, vi and ti (1 ≤ ui, vi ≤ n, 0 ≤ ti ≤ 10^18, ui ≠ 
vi) — vertices connected by the edge and integer written on it. It is guaranteed that 
graph doesn't contain self-loops and multiple edges.
```

## 【Output】

```cpp
Print the single integer, equal to the described sum over modulo 10^9 + 7.
```

------



## 【Examples】 

#### Sample Input

```cpp
4 4
1 2 1
1 3 2
2 3 3
3 4 1
```

#### Sample Output

```cpp
12
```
#### Sample Input

```cpp
4 4
1 2 1
2 3 2
3 4 4
4 1 8
```

#### Sample Output

```cpp
90
```
#### Sample Input

```cpp
8 6
1 2 2
2 3 1
2 4 4
4 5 5
4 6 3
7 8 5
```

#### Sample Output

```cpp
62
```
------



## 【Problem Description】

```cpp
给你一个图（可能不连通），定义一个三元组为（u，v，s)，1<=u<v<=n，其中u,v表示一条路径的起点和终点，s为这
条路径中所有边的权值异或和。让你求给定的图中所有三元组的s的和。每条边和每个顶点可以多次使用。
```

## 【Solution】

```cpp
前面做过BZOJ2115,类似的，对于连通的情况，我们先求出图中所有环的异或值，组成线性基。
因为要求所有三元组的s的和，所以我们考虑每个二进制位对最终答案的贡献。

在求图中所有环的时候，可以顺便求得dis[v]的值，dis[v]表示图中一条路径1~v中所有边的异或和
那么对于任意一条路径u~v，它的异或和就等于t=dis[u]^dis[v]，对于第i个二进制为，它只能是0或1。

如果是0，对于这条边来说，它要对最终答案中的第i个二进制位i产生贡献，它就必须经过一个环并且这个环的异或和的第
i个二进制位为1. 那么就在线性基中查找是否有某个基向量的第i位为1，如果有肯定只有一个，那么其他的基向量可以随
意取，有2^(r-1)中取法。如果没有就不能产生贡献。

如果是1，对于这条边来说，他要对最终答案中的第i个二进制位产生贡献，就不能经过某个环，这个环的异或和的第i个二
进制位为1，那么就在线性基中查找是否存在某个基向量第i位为1，如果有，这个基向量不能选，剩下有2^(r-1)中取
法。如果没有，任意的基向量都可以选有2^r中取法。

最终对这个二进制位的贡献就等于（取法数）*（2^i)

若考虑整个图，统计所有的顶点v，dis[v]中第i个二进制位为0的个数zeors，和为1的个数ones，那么所有起点终点的
组合中，使得t为1的方案数有zeors*ones种，使得t为0的方案数有zeors*(zeors-1)+ones*(ones-1)种

那么最终整个图上对第i个二进制位的贡献就为 方案数*上面所已求得的贡献，然后将两种情况相加即可。

图可能不连通，所以要遍历所有顶点进行dfs
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-03-05 19:56:35 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-03-05 22:13:42
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
const int mod=1e9+7;
int a[65],head[maxn],cnt=0,dis[maxn];//线性基，邻接表辅助数组，边数，1~i路径的异或值，
struct node{//邻接表建图
    int v,w,next;
    node(){}
    node(int v,int w,int next):v(v),w(w),next(next){}
}g[maxn<<2];
void addedge(int u,int v,int w){//建图
    g[cnt]=node(v,w,head[u]);
    head[u]=cnt++;
}
bool vis[maxn];
int num=0;
void insert(int val){//线性基的插入
    for(int i=62;~i;i--){
        if(val>>i&1LL){
            if(!a[i]){a[i]=val;break;}
            val^=a[i];
        }
    }
}
int ccnt=0,vet[maxn];
void dfs(int u){//求环
    vis[u]=1; vet[++ccnt]=u;//记录此次搜索所经过的所有顶点
    for(int i=head[u];~i;i=g[i].next){
        int v=g[i].v;
        if(vis[v]) insert(dis[u] ^ g[i].w ^ dis[v]);//形成环，则将其插入线性基中
        else{
            dis[v]=dis[u]^g[i].w;dfs(v);
        }
    }
}
int fpow(int a,int b){//快速幂
    int ans=1;ans%=mod;
    while(b){
        if(b&1LL) ans=ans*a%mod;
        a=a*a%mod;
        b>>=1LL;
    }
    return ans;
}
int cot[5];
int solve(int n){
    for(int i=0;i<=62;i++) if(a[i]) num++;//统计基向量的个数
    int ans=0;
    for(int i=0;i<=62;i++){
        cot[0]=cot[1]=0;bool flag=0;//第i个二进制位为0或1的个数，
        for(int j=0;j<=62;j++)if(a[j]>>i&1LL) {flag=1;break;}//是否存在某个基向量的第i个二进制位为1
        for(int j=1;j<=ccnt;j++){cot[(dis[vet[j]]>>i&1LL)]++;}//统计当前连通块所有顶点的第i个二进制位的值的个数
        int now=cot[0]*(cot[0]-1LL)/2LL+cot[1]*(cot[1]-1LL)/2LL; now%=mod;//所有起点终点组合中异或后第i个二进制位为0的方案数
        if(flag){
            if(num>=1) now=(now*fpow(2,num-1))%mod;
            now=now*fpow(2,i)%mod;//对第i位的贡献
            ans=(ans+now)%mod;
        }
        now=cot[0]*cot[1];now%=mod;//所有起点终点组合中异或后第i个二进制位为1的方案数
        if(flag) {if(num>=1) now=now*fpow(2,num-1)%mod;}//如果存在某个基向量的第i个二进制位为1，则剩余基向量任意组合有2^(num-1)中情况
        else now=now*fpow(2,num)%mod;//否则有2^num中情况
        now=now*fpow(2,i)%mod;ans=(ans+now)%mod;//对第i位的贡献
    }
    return ans;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    memset(head,-1,sizeof(head));
    int n,m;scanf("%lld%lld",&n,&m);
    for(int i=1;i<=m;i++){
        int u,v,w;scanf("%lld%lld%lld",&u,&v,&w);
        addedge(u,v,w);addedge(v,u,w);
    }
    int ans=0;
    for(int i=1;i<=n;i++){//图可能不连通，所以遍历所有顶点
        if(vis[i]) continue;
        ccnt=0; memset(a,0,sizeof(a));num=0;
        dfs(i);ans=(ans+solve(n))%mod;
    }
    printf("%lld\n",ans);
    cin.get(),cin.get();
    return 0;
}
```
