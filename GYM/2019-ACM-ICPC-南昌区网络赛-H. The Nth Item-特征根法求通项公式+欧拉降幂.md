#[2019-ACM-ICPC-南昌区网络赛-H. The Nth Item-特征根法求通项公式+欧拉降幂](https://nanti.jisuanke.com/t/41355) 

![](H:\GitHub\Algorithm\GYM\https___nanti.jisuanke.com_t_41355.png)

------



## 【Problem Description】

​		已知$f(n)=3\cdot f(n-1)+2\cdot f(n-2),(n\ge 2)$，求$f(n)\pmod {998244353}$。

## 【Solution】

​		利用特征根法求得通项公式为$a_n=\frac{\sqrt{17}}{17}\cdot\Bigg(\Big(\frac{3+\sqrt{17}}{2} \Big)^n-\Big(\frac{3-\sqrt{17}}{2} \Big)^n\Bigg)$。$\sqrt{17}\pmod {998244353} $可以用二次剩余求得。然后就可以用快速幂在$O(log(n))$的时间复杂度内求得$a_n$。但是因为$T\le 10^7$，所以还需优化。

​		$n\le 10^{18}$，进行欧拉降幂后$n\le 10^9$，令$k=\lfloor \sqrt{n}\rfloor$，则：
$$
n=k\cdot t+r\\
x^n=x^{k\cdot t+r}\Leftrightarrow x^n=x^{k\cdot t}+x^r(t,r\le k)
$$
然后预处理出$x^r,r\le k$以及$x^{k\cdot t},t\le k$。则$x^n$就可以$O(1)$查询了。

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-09-08 13:13:29 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-09-17 17:44:39
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
const int mod=998244353;
int inv17;
/*
 * Author: Simon
 * 功能: 求解x^2=n(mod p),即x=sqrt(n)(mod p)
 * 复杂度: O(sqrt(p))
 */

/*类似复数 单位元为w(复数的单位元为-1)*/
struct Complex {
    int x, y, w;
    Complex() {}
    Complex(int x, int y, int w) : x(x), y(y), w(w) {}
};
/*类复数乘法 */
Complex mul(Complex a, Complex b, int p) {
    Complex ans;
    ans.x = (a.x * b.x % p + a.y * b.y % p * a.w % p) % p;
    ans.y = (a.x * b.y % p + a.y * b.x % p) % p;
    ans.w = a.w;
    return ans;
}
/*类复数快速幂 */
Complex Complexfpow(Complex a, int b, int mod) {
    Complex ans = Complex(1, 0, a.w);
    while (b) {
        if (b & 1) ans = mul(ans, a, mod);
        a = mul(a, a, mod);
        b >>= 1;
    }
    return ans;
}
int fpow(int a, int b, int mod) {
    int ans = 1;
    a %= mod;
    while (b) {
        if (b & 1) (ans *= a) %= mod;
        (a *= a) %= mod;
        b >>= 1;
    }
    return ans;
}
/*求解x^2=n(mod p) */
int solve(int n, int p) {
    n %= p;
    if (n == 0) return 0;
    if (p == 2) return n;
    if (fpow(n, (p - 1) / 2, p) == p - 1) return -1; /*勒让德定理判断n不是p的二次剩余 */
    mt19937 rnd(time(0));
    int a, t, w;
    do {
        a = rnd() % p;
        t = a * a - n;
        w = (t % p + p) % p;                    /*构造w=a^2-n */
    } while (fpow(w, (p - 1) / 2, p) != p - 1); /*找到一个w不是p的二次剩余 */
    Complex ans = Complex(a, 1, w);
    ans = Complexfpow(ans, (p + 1) / 2, p); /*答案为(a+w)^{(p+1)/2} */
    return ans.x;
}
pair<int,int> bit1[maxn],bit2[maxn];
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);inv17=fpow(17,mod-2,mod);
    int x=solve(17,mod); //二次剩余
    int lim=ceil(sqrt(1e9));
    int s1=(3+x)%mod*fpow(2,mod-2,mod)%mod,
        s2=(3-x)%mod*fpow(2,mod-2,mod)%mod;
    bit1[0].first=bit1[0].second=bit2[0].first=bit2[0].second=1;
    for(int i=1;i<=lim;i++){ //预处理
        bit1[i].first=bit1[i-1].first*s1%mod;
        bit1[i].second=bit1[i-1].second*s2%mod;
        bit2[i].first=fpow(s1,i*lim,mod);
        bit2[i].second=fpow(s2,i*lim,mod);
    }
    int q,n;cin>>q>>n;
    int ans=0;
    while(q--){
        int tmp=0;
        if(n==0) tmp=0;
        else if(n==1) tmp=1;
        else{
            int t=n%(mod-1);
            int t2=t/lim,t1=t%lim;
            tmp=(bit1[t1].first*bit2[t2].first%mod-bit1[t1].second*bit2[t2].second%mod)%mod*x%mod*inv17%mod;
        }
        tmp=(tmp+mod)%mod;
        n=tmp*tmp^n; ans^=tmp;
        // cout<<n<<endl;
    }
    cout<<(ans+mod)%mod<<endl;
#ifndef ONLINE_JUDGE
    cout<<endl;system("pause");
#endif
    return 0;
}
```
