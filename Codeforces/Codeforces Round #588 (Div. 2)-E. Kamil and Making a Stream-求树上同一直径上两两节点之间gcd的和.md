# [Codeforces Round #588 (Div. 2)-E. Kamil and Making a Stream-求树上同一直径上两两节点之间gcd的和](https://codeforces.com/contest/1230)

![](/Users/simon/Documents/GitHub/Algorithm/Codeforces/https___codeforces.com_contest_1230_problem_E.png)

---



## 【Problem Description】

给你一棵树，树上每个节点都有一个权值。定义$1\sim v$的最短路径所经过的所有节点$u$称为$v$节点的祖先。定义函数$f(u,v)=gcd(u,t1,t2,\dots,v)$，其中$u,t1,t2,\dots$都是$v$的祖先。求$\sum f(u,v)$。

## 【Solution】

对于每一个节点$v$维护一个$vector$数组，记录其所有祖先$u$对$v$的$f(u,v)$的取值，以及$f(u,v)$出现的次数。那么对于节点$v$的儿子节点$s$，其所有的$f(u,s)$取值就为所有$f(u,v)$的取值与$a[s]$的$gcd$。对总答案的贡献，只要将取值乘以出现的次数即可。（其实就是很暴力的做法）

------



## 【Code】

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 200000
const int mod=1e9+7;
int a[maxn];
vector<int>g[maxn];
set<int>s[maxn];
int ans=0;
int gcd(int a,int b){
    return b==0?a:gcd(b,a%b);
}
map<int,int>mp[maxn];
void dfs(int u,int p){
    for(auto v:g[u]){
        if(v==p) continue;
        for(auto vv:s[u]){ //通过父节点进行转移
            int t=gcd(vv,a[v]);
            (ans+=t%mod*mp[u][vv]%mod)%=mod; //贡献为取值乘以出现的次数。
            mp[v][t]+=mp[u][vv]; //更新t值出现的次数
            s[v].insert(t);
        }
        s[v].insert(a[v]);(ans+=a[v]%mod)%=mod; //最后把自己放入
        mp[v][a[v]]++;
        dfs(v,u);
    }
}
signed main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i];
    for(int i=1;i<n;i++){
        int u,v;cin>>u>>v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    s[1].insert(a[1]);ans=a[1]%mod;mp[1][a[1]]=1;
    dfs(1,-1);
    cout<<ans<<endl;
    return 0;
}

```
