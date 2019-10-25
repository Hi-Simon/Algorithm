# [2018 China Collegiate Programming Contest Final (CCPC-Final 2018)-K - Mr. Panda and Kakin-中国剩余定理+同余定理](https://codeforces.com/gym/102055) 

![1571809608347](H:\GitHub\Algorithm\GYM\https___codeforces.com_gym_102055_problem_K.png)

------



## 【Problem Description】

$$
求解x^{2^{30}+3}=c\pmod n
$$

其中$n=p\cdot q$，$p$为小于$x$的最大素数，$q$为大于$x$的最小素数，$x$为$[10^5,10^9]$内随机选择的数。$0< c<n$。

## 【Solution】

令$a=2^{30}+3$，所以有$x^a=c\pmod n\Leftrightarrow x^{a\ mod \ \varphi(n)}=c\pmod n$。又因为$n=p\cdot q$，所以有$x^{a\ mod \ (p-1)\cdot (q-1)}=c\pmod n$，求出$a$在模$(p-1)\cdot (q-1)$意义下的逆元$d$，则$x=c^d\pmod n$。然后用中国剩余定理求解答案$x$即可。

至于$p,q$，由题意可知，$p,q$都在$\sqrt{n}$附近，所以暴力求解即可。

------



## 【Code】

```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long
int exgcd(int a,int b,int &x,int &y){ //扩展欧几里得
	if(a==0&&b==0) return -1;
	if(b==0){
		x=1;y=0;
		return a;
	}
	int gcd=exgcd(b,a%b,y,x);
	y-=a/b*x;
	return gcd;
}
int solve(int a,int b,int c){ //求逆元
	int x,y;
	int gcd=exgcd(a,b,x,y);
	if(c%gcd!=0) return -1;
	return (x%b+b)%b;
}
int fpow(int a,int b,int mod){ //快速幂
	int ans=1;a%=mod;
	while(b){
		if(b&1) (ans*=a)%=mod;
		(a*=a)%=mod;
		b>>=1;
	}
	return ans;
}
int fmul(int a,int b,int mod){ //快速乘
	int ans=0;a%=mod;
	while(b){
		if(b&1) ans=(ans+a)%mod;
		a=(a+a)%mod;
		b>>=1;
	}
	return ans;
}
int crt(int ai[], int mi[], int len) { //中国剩余定理
    int ans = 0, lcm = 1;
    for (int i = 0; i < len; i++) lcm *= mi[i];
    for (int i = 0; i < len; i++) {
        int Mi = lcm / mi[i];
        int inv = fpow(Mi, mi[i] - 2, mi[i]);
        int x = fmul(fmul(inv, Mi, lcm), ai[i], lcm); //若lcm大于1e9需要用快速乘fmul
        ans = (ans + x) % lcm;
    }
    return ans;
}
int mi[5],ai[5];
signed main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int T,ca=0;cin>>T;
	while(T--){
		cout<<"Case "<<++ca<<": ";
		int n,c,p,q;cin>>n>>c;
		for(int i=sqrt(n);i>=0;i--) if(n%i==0){
			p=i;q=n/i;
			break;
		}
		int d=solve((1LL<<30)+3,(p-1)*(q-1),1);
		if(d==-1){
			cout<<"-1"<<endl;
			continue;
		}
		ai[0]=fpow(c,d,p);ai[1]=fpow(c,d,q);
		mi[0]=p;mi[1]=q;
		cout<<crt(ai,mi,2)<<endl;
	}
	return 0;
}

```
