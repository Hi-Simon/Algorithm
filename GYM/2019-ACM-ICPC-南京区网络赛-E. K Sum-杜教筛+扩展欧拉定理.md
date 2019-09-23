#[2019-ACM-ICPC-南京区网络赛-E. K Sum-杜教筛+欧拉定理](https://nanti.jisuanke.com/t/41302)

![](H:\GitHub\Algorithm\GYM\https___nanti.jisuanke.com_t_41302.png)

------



## 【Problem Description】

令$f_n(k)=\sum_{l_1=1}^n\sum_{l_2=1}^n\dots\sum_{l_k=1}^n gcd(l_1,l_2,\dots,l_k)$。求$\sum_{i=2}^kf_n(i)\ mod \ (10^9+7)$。

## 【Solution】

对于$f_n(k)$有：
$$
\sum_{l_1=1}^n\sum_{l_2=1}^n\dots\sum_{l_k=1}^ngcd(l_1,l_2,\dots,l_k)=\sum_{d=1}^n\sum_{l_1=1}^{\frac{n}{d}}\sum_{l_2=1}^{\frac{n}{d}}\dots\sum_{l_k=1}^{\frac{n}{d}}[gcd(l_1,l_2,\dots,l_k)=1]\cdot d^2
\\=\sum_{d=1}^nd^2\sum_{t=1}^{n}\mu(t)\sum_{l_1=1}^{\frac{n}{dt}}\sum_{l_2=1}^{\frac{n}{dt}}\dots\sum_{l_k=1}^{\frac{n}{dt}}=\sum_{d=1}^nd^2\sum_{t=1}^n\mu(t)\lfloor\frac{n}{dt}\rfloor^k
$$
令$T=dt$得：
$$
f_n(k)=\sum_{T=1}^{n}\sum_{t|T}\mu(t)\cdot\frac{T^2}{t^2}\cdot \lfloor\frac{n}{T}\rfloor^k
$$
则$\sum_{i=2}^kf_n(i)$为：
$$
\sum_{i=2}^kf_n(i)=\sum_{i=2}^k\sum_{T=1}^n\sum_{t|T}\mu(t)\cdot \frac{T^2}{t^2}\cdot \lfloor\frac{n}{T}\rfloor^k=\sum_{T=1}^n\sum_{t|T}\mu(t)\frac{T^2}{t^2}\sum_{i=2}^k\lfloor\frac{n}{T}\rfloor^k
\\=\sum_{T=1}^n\sum_{t|T}\mu(t)\frac{T^2}{t^2}\Big(\frac{\lfloor\frac{n}{T}\rfloor^{k+1}-1}{\lfloor\frac{n}{T}\rfloor-1}-\lfloor\frac{n}{T}\rfloor-1\Big)
$$
因为$k\le 10^{10^5}$，所以用欧拉定理降幂取模即可。注意特判$\lfloor \frac{n}{T}\rfloor=1$的情况。

其中令$g(T)=\sum_{t|T}\mu(t)\cdot \frac{T^2}{t^2}，\Phi(n)=\sum_{T=1}^ng(T)$。对于$T$小的部分可以通过线性筛求得：

1.  当$T$为素数时，$g(T)=T^2-1$。
2.  若$T$中无平方质因子时$T=p_1\cdot p_2\dots p_k$，因为$g(T)$为积形函数，则有$g(T)=g(p_1)\cdot g(p_2)\dots g(p_k)$。
3.  若$T$中有平方质因子时，有$g(T\cdot p)=g(T)\cdot p^2$。

对于$T$大的部分，我们发现$g(T)=\mu*id^2(T)$，则$g*I(T)=\mu*I*id^2(T)=e*id^2(T)=id^2(T)=T^2$。

则有：
$$
\sum_{T=1}^nT^2=\sum_{T=1}^n\sum_{d|T}g(d)=\sum_{T=1}^n\sum_{d|T}\sum_{t|d}\mu(t)\cdot \frac{d^2}{t^2}=\sum_{T=1}^n\sum_{d=1}^{\frac{n}{T}}\sum_{t|d}\mu(t)\cdot\frac{d^2}{t^2}
\\=\sum_{T=1}^n\Phi(\frac{n}{T}),则\Phi(n)=\frac{n\cdot (n+1)\cdot (2\cdot n+1)}{6}-\sum_{T=2}^n\Phi(\frac{n}{T})
$$

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-09-04 15:07:56 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-09-04 16:22:26
 */
#include<bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 1000005
typedef long long ll;
const int mod=1e9+7;
const int Mod=3e6;
int prime[maxn],cnt=0,inv6;
ll phi[maxn],sum[maxn];
bool vis[maxn]={1,1};
void Euler(){ //线性筛
    phi[1]=1;
    for(int i=2;i<maxn;i++){
        if(!vis[i]){
            prime[++cnt]=i;
            phi[i]=((i*1LL*i%mod-1)%mod+mod)%mod;
        }
        for(int j=1;j<=cnt&&i*1LL*prime[j]<maxn;j++){
            vis[i*prime[j]]=1;
            if(i%prime[j]==0){
                phi[i*prime[j]]=phi[i]*1LL*prime[j]%mod*prime[j]%mod;
                break;
            }
            phi[i*prime[j]]=phi[i]*1LL*phi[prime[j]];
        }
    }
    for(int i=1;i<maxn;i++) sum[i]=(sum[i-1]+phi[i])%mod;
}
int fpow(int a,int b,int mod){
    a%=mod; int ans=1;
    while(b){
        if(b&1) ans=1LL*ans*a%mod;
        a=1LL*a*a%mod;
        b>>=1;
    }
    return ans;
}
unordered_map<ll,ll>mp;
int cal(int n,int k1,int k2){ //等比数列求和公式
    if(n==1) return (k2-1)%mod; //特判
    int t1=fpow(n,k1+1,mod)-1LL*n*n%mod,t2=n-1;
    return 1LL*t1*fpow(t2,mod-2,mod)%mod;
}
int sum_2(int n){
    n%=mod;
    return 1LL*n%mod*(n+1)%mod*(2*n+1)%mod*inv6%mod;
}
int dfs(int n){ //Phi(n)
    if(n<maxn) return sum[n];
    if(mp[n]) return mp[n];
    int sum=0;
    for(int i=2,j;i<=n;i=j+1){
        j=n/(n/i);
        sum=(sum+(j-i+1)*1LL*dfs(n/i)%mod)%mod;
    }
    sum=(sum_2(n)-sum)%mod;
    mp[n]=sum;
    return sum;
}
int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);Euler(); inv6=fpow(6,mod-2,mod);
    int T;cin>>T;
    while(T--){
        mp.clear();
        int n;string k;cin>>n>>k;
        int t1=0,t2=0;
        for(int i=0;i<k.size();i++){
            t1=(1LL*t1*10+(k[i]-'0'))%(mod-1);
            t2=(1LL*t2*10+(k[i]-'0'))%mod;
        }
        int ans=0;
        for(int i=1,j;i<=n;i=j+1){
            j=n/(n/i);
            ans=(ans*1LL+(dfs(j)-dfs(i-1))%mod*1LL*cal(n/i,t1,t2)%mod)%mod;
        }
        cout<<(ans+mod)%mod<<endl;
    }
#ifndef ONLINE_JUDGE
    cout<<endl;system("pause");
#endif
    return 0;
}
```
