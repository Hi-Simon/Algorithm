#[Codeforces Round #582 (Div. 3)-G. Path Queries-并查集](https://codeforces.com/contest/1213)

![](H:\GitHub\Algorithm\Codeforces\https___codeforces.com_contest_1213_problem_G.png)

------



## 【Problem Description】

给你一棵树，求有多少条简单路径$(u,v)$，满足$u$到$v$这条路径上的最大值不超过$k$。$q$次查询。

## 【Solution】

并查集

将所有边按权值从小到大排序，查询值$k_i$也从小到大排序。对于每次查询的值$k_i$，将边权小于等于$k_i$的边通过并查集合并在一起。对于合并后的联通块，每个联通块对答案的贡献为$\frac{size\times(size-1)}{2}$，所有联通块的贡献直接求和即可。所以每次合并两个节点时，先将这两个节点所在的联通块的贡献减去，再加上合并后的贡献即可。

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-09-07 10:00:30 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-09-07 10:29:11
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 200005
struct node{
    int u,v,w;
    node(){}
    node(int u,int v,int w):u(u),v(v),w(w){}
    bool operator <(const node&a)const{
        return w<a.w;
    }
}g[maxn],q[maxn];
int Set[maxn],Rank[maxn];
void init(int n){ //初始化
    for(int i=0;i<=n;i++){
        Set[i]=i;Rank[i]=1;
    }
}
int cal(int x){ //计算联通块的贡献
    return x*(x-1)/2;
}
int Find(int u){ //并查集的查找根节点
    return Set[u]==u?u:Set[u]=Find(Set[u]);
}
int res=0;
int ans[maxn];
void merge(int u,int v){ //并查集的合并
    u=Find(u),v=Find(v);
    if(Rank[u]<Rank[v]) swap(u,v);
    res-=cal(Rank[u]),res-=cal(Rank[v]); //减去u，v所在联通块的贡献
    Set[v]=u;Rank[u]+=Rank[v]; //合并两个联通块
    res+=cal(Rank[u]); //再加上合并后的联通块的贡献
}
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    int n,m;cin>>n>>m;init(n);
    for(int i=1;i<n;i++){
        cin>>g[i].u>>g[i].v>>g[i].w;
    }
    sort(g+1,g+n);
    for(int i=1;i<=m;i++){
        cin>>q[i].w;q[i].u=i;
    }
    sort(q+1,q+m+1);
    int pos=1;
    for(int i=1;i<=m;i++){
        while(pos<n&&g[pos].w<=q[i].w){
            merge(g[pos].u,g[pos].v);
            pos++;
        }
        ans[q[i].u]=res;
    }
    for(int i=1;i<=m;i++) cout<<ans[i]<<" \n"[i==m];
#ifndef ONLINE_JUDGE
    cout<<endl;system("pause");
#endif
    return 0;
}
```
