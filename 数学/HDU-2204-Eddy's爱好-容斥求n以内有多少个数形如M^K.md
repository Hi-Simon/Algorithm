# [HDU-2204-Eddy's爱好-容斥求n以内有多少个数形如$M^K$](http://acm.hdu.edu.cn/showproblem.php?pid=2204)

![](H:\GitHub\Algorithm\数学\http___acm.hdu.edu.cn_showproblem.php_pid=2204.png)

------



## 【Problem Description】

略

## 【Solution】

对于一个指数$k$，找到一个最大的$m$使得$m^k\le n$，则$k$这个指数对答案的贡献为$m$，因为对于$i\in[1,m]$中的数$i^k$一定小于等于$n$。而$m=n^{\frac{1}{k}}$。由唯一分解定理可知，$k$一定能表示为一些素数的乘积。所以只需要考虑$64$以内的素数即可。但是会出现重复的值，例如$8^2=4^3=2^{2\times 3}$，所以需要用容斥去重即可。

------



## 【Code】

```cpp
#include<iostream>
#include<cmath>
#include<algorithm>
#include<cstring>
#include<cstdio>
using namespace std;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 65
int prime[maxn],cnt=0;
bool vis[maxn]={1,1};
void Euler(){ //欧拉筛素数
	for(int i=2;i<maxn;i++){
		if(!vis[i]) prime[++cnt]=i;
		for(int j=1;j<=cnt&&i*prime[j]<maxn;j++){
			vis[i*prime[j]]=1;
			if(i%prime[j]==0) break;
		}
	}
}
int n,ans=0;
int fpow(int a,int b){ //快速幂
	int ans=1;
	while(b){
		if(b&1) ans*=a;
		a*=a;
		b>>=1;
	}
	return ans;
}
void dfs(int pos,int num,int val){ //容斥
	if(val>64) return ; //最大值不超过64
	int tmp=pow(n,1.0/val)+0.1; //求最大的m
	if(fpow(tmp,val)>n) tmp--;tmp--; //精度判断
	if(num) if(num&1) ans+=tmp;
	else ans-=tmp;
	for(int i=pos+1;i<=cnt;i++){
		dfs(i,num+1,val*prime[i]);
	}
}
signed main(){
	ios::sync_with_stdio(false);
	cin.tie(0);Euler();
	while(cin>>n){
		ans=1;//1一定满足条件
		dfs(0,0,1);
		cout<<ans<<endl;
	}
	return 0;
}

```
