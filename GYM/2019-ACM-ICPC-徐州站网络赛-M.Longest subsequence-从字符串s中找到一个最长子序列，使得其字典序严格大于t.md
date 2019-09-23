#[2019-ACM-ICPC-徐州站网络赛-M.Longest subsequence-从字符串s中找到一个最长子序列，使得其字典序严格大于t](https://nanti.jisuanke.com/t/41395)

![](H:\GitHub\Algorithm\GYM\https___nanti.jisuanke.com_t_41395.png)

------



## 【Problem Description】

​		从字符串$s$中找到一个最长子序列，使得其字典序严格大于$t$。

## 【Solution】

​		对于答案字符串来说，一定是和$t$串的前面部分一致，从一个字母开始比$t$的字符大，以后的字符就都取上就行了。从前到后扫描$s$串，同时用一个数组$sum[26]$维护$t$中字母最后出现的位置，对于$s$中每个字符，用每一个小于字符$s_i$并在此之前出现的字母，逐步更新答案即可。**注意特判两字符完全相同的情况。**



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-09-07 15:43:51 
 * @Last Modified by:   Simon 
 * @Last Modified time: 2019-09-07 15:43:51 
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 2000005
char s[maxn],p[maxn];
int sum[30]/*每个字母最后出现的位置*/,pos=1;
bool vis[30];//每个字母是否出现过
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m;cin>>n>>m;
    cin>>s+1>>p+1;
    int ans=0;
    for(int i=1;i<=n;i++){
        for(int j=0;j<26;j++){
            if(vis[j]&&s[i]-'a'>j) ans=max(ans,sum[j]+n-i);
        }
        if (s[i] > p[pos]) ans = max(ans, pos + n - i);
        else if(s[i]==p[pos]){
            vis[p[pos]-'a']=1;
            sum[p[pos]-'a']=pos++;
        }
        if(pos>m){
            if(i!=n) ans=max(ans,m+n-i); //两个字符串不能完全相同
            break;
        }
    }
    if(!ans) cout<<"-1"<<endl;
    else cout<<ans<<endl;
#ifndef ONLINE_JUDGE
    cout<<endl;system("pause");
#endif
    return 0;
}
```
