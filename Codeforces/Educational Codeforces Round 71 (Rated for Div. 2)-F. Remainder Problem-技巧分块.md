#[Educational Codeforces Round 71 (Rated for Div. 2)-F. Remainder Problem-技巧分块](https://codeforces.com/contest/1207)

![](H:\GitHub\Algorithm\Codeforces\https___codeforces.com_contest_1207_problem_F.png)

------



## 【Problem Description】

​		初始$[1,500000]$都为0，后续有两种操作：

​				$1$、将$a[x]$的值加上$y$。

​				$2$、求所有满足$i\ mod\ x=y$的$a[i]$的和。

## 【Solution】

​		具体做法就是，对于前$\sqrt{500000}=708$个数，定义$dp[j][k]$表示所有满足$i\ mod\ j=k$的$a[i]$的和。每次进行$1$操作的时候，$O(\sqrt{500000})$预处理一下。查询时可$O(1)$查询。

​		对于大于$O(\sqrt{500000})$的数，可暴力求解：$i\ mod\ x=y\Leftrightarrow i+x\cdot t=y$。所以只需要枚举$\frac{500000}{x}$次即可。而$x$不小于$\sqrt{500000})=708$，所以枚举次数不大于$708$次，所以总复杂度为$O(500000^{\frac{3}{2}})$。

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-28 14:34:47 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-28 15:02:25
 */
#include<bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 1005
#define maxm 500005
int dp[maxn][maxn]/*下标满足模i余数为j的 值的和*/,a[maxm];
int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int q,m=ceil(sqrt(maxm));cin>>q;
    while(q--){
        int p,x,y;cin>>p>>x>>y;
        if(p==1){
            a[x]+=y;
            for(int i=1;i<=m;i++) dp[i][x%i]+=y;
        }
        else{
            if(x<=m) cout<<dp[x][y]<<endl;
            else{
                int ans=0;
                for(int i=y;i<maxm;i+=x) ans+=a[i];
                cout<<ans<<endl;
            }
        }
    }
#ifndef ONLINE_JUDGE
    cout<<endl;system("pause");
#endif
    return 0;
}
```
