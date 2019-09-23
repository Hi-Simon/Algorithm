#[Educational Codeforces Round 72 (Rated for Div. 2)-D. Coloring Edges-拓扑排序](https://codeforces.com/contest/1217)

![](H:\GitHub\Algorithm\Codeforces\https___codeforces.com_contest_1217_problem_D.png)

------



## 【Problem Description】

​		给你一个有向图，给用最少的颜色给每条边染色，要保证不存在一个环中的所有边都是同一个颜色。

## 【Solution】

​		用拓扑排序判断图中是否存在环，若图中不存在环，则所有边都是同一种颜色。否则，同一个环中，只要用两种颜色就可以满足题目条件，所以总的颜色数就是两种，对于一个环，**一定会存在两种边**：1、节点号小的顶点指向节点号大的顶点，2、节点号大的顶点指向节点号小的顶点。所以将这两种不同的边，染成不同的颜色即可。

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-09-16 10:46:37 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-09-16 10:54:35
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 5005
vector<int>ans,g[maxn];
int deg[maxn];
bool bfs(int n){ //拓扑排序判断是否有环
    queue<int>q;
    for(int i=1;i<=n;i++){
        if(deg[i]==0) q.push(i);
    }
    while(!q.empty()){
        int u=q.front();q.pop();
        for(auto v:g[u]){
            deg[v]--;
            if(deg[v]==0) q.push(v);
        }
    }
    for(int i=1;i<=n;i++) if(deg[i]) return 1;
    return 0;
}
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m;cin>>n>>m;
    for(int i=1;i<=m;i++){
        int u,v;cin>>u>>v;
        g[u].push_back(v);
        if(u>v) ans.push_back(1); //根据边类型染色。
        else ans.push_back(2);
        deg[v]++;
    }
    if(bfs(n)){
        cout<<"2"<<endl;
        for(auto v:ans) cout<<v<<' ';
    }else{
        cout<<"1"<<endl;
        for(int i=1;i<=m;i++) cout<<1<<' ';
    }
    cout<<endl;
#ifndef ONLINE_JUDGE
    cout<<endl;system("pause");
#endif
    return 0;
}
```
