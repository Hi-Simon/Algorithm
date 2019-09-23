#[2019-ACM-ICPC-南京区网络赛-D. Robots-DAG图上概率动态规划](https://nanti.jisuanke.com/t/41301)

![](H:\GitHub\Algorithm\GYM\https___nanti.jisuanke.com_t_41301.png)

------



## 【Problem Description】

​		有向无环图中，有个机器人从$1$号节点出发，每天等概率的走到下一个节点或者停在当前节点，并且第$i$天消耗$i$的耐久度。求它到达$n$号节点时期望消耗的耐久度是多少？

​		**题目保证只有一个入度为$0$的节点，只有一个出度为$0$的节点。**

## 【Solution】

​		概率$dp$。

​		假设每天消耗$1$点耐久度。定义$dp[u]$表示从$u$节点走到$n$节点的期望消耗的耐久度。定义$v$为$u$的后继节点。$du[u]$表示$u$节点的出度。则有：
$$
dp[u]=\frac{\sum(dp[v]+1)}{du[u]+1}+\frac{dp[u]+1}{du[u]+1}
$$
表示$u$到$n$的期望消耗的耐久度为从$u$开始不停留走到$n$的期望消耗的耐久度+从$u$开始停留一天再走到$n$所消耗的耐久度。此时求出来的可以等价为第$i$天期望消耗的耐久度。

再用同样的公式求得答案即可：
$$
ans[u]=\frac{\sum(ans[v]+dp[v]+1)}{du[u]+1}+\frac{ans[u]+dp[u]+1}{du[u]+1}
$$

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-09-05 20:22:25 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-09-05 21:26:57
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
vector<int>g[maxn];
bool vis[maxn];
double dp[maxn],dp1[maxn];
void dfs(int u,int n){
    if (vis[u]) return; //判断不能放在for循环中,否则就缺少一层回溯
    if(u==n) return; 
    vis[u] = 1;
    int du=0;
    for(auto v:g[u]){
        dfs(v,n);
        dp[u]+=dp[v]+1;
        dp1[u]+=dp1[v]+dp[v]+1;
        du++; //统计出度
    }
    dp[u]=(dp[u]+1)/du;
    dp1[u]=(dp1[u]+dp[u]+1)/du;
}
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;cin>>T;
    while(T--){
        int n,m;cin>>n>>m;
        memset(dp,0,sizeof(dp));
        memset(dp1,0,sizeof(dp1));
        memset(vis,0,sizeof(vis));
        for(int i=1;i<=m;i++){
            int u,v;cin>>u>>v;
            g[u].push_back(v);
        }
        dfs(1,n);
        cout<<setiosflags(ios::fixed)<<setprecision(2);
        cout<<dp1[1]<<endl;
        for(int i=0;i<=n;i++) g[i].clear();
    }
#ifndef ONLINE_JUDGE
    cout<<endl;system("pause");
#endif
    return 0;
}
```
