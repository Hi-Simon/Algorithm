# [ACM International Collegiate Programming Contest, Tishreen Collegiate Programming Contest (2017)- K. Poor Ramzi -dp+记忆化搜索](https://codeforces.com/gym/101915) 

![](H:\GitHub\Algorithm\GYM\https___codeforces.com_gym_101915_problem_K.png)  

------



## 【Problem Description】

给你一串$01$字符串，将其划分，使得划分后，分别求出每组$01$之和后是回文的，求划分的方案数，

## 【Solution】

定义$dp[l][r]$表示，区间$[l,r]$中满足条件的方案数是多少。$dfs(i+1,j-1)$表示在$i,i+1$之间插入一个隔板，在$j-1,j$之间插入一个隔板。枚举所有可能隔板的插入位置来转移即可。注意自身整体也算是一种方案。

------



## 【Code】

```cpp
/*
 * @Author: _Simon_
 * @Date: 2019-11-07 11:31:32 
 * @Last Modified by: _Simon_
 * @Last Modified time: 2019-11-07 11:31:32
 */ 
#include<bits/stdc++.h>
using namespace std;
#define int long long
#define maxn 205
#define INF 0x3f3f3f3f
const int mod=1e9+7;
string s;
int dp[maxn][maxn]; //dp[l][r]表示区间[l,r]的划分方案数
int dfs(int l,int r){ 
	if(l>=r) return 1;
	if(dp[l][r]) return dp[l][r];
	dp[l][r]=1; //自身也是一种方案
	int left=0,right=0;
	for(int i=l;i<r;i++){ //枚举隔板插入的位置
		left+=s[i]-'0';right=0;
		for(int j=r;j>i;j--){
			right+=s[j]-'0'; //记录和
			if(left==right) (dp[l][r]+=dfs(i+1,j-1))%=mod; 如果满足条件，统计方案数
		}
	}
	return dp[l][r];
}
signed main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int T;cin>>T;
	while(T--){
		memset(dp,0,sizeof(dp));
		cin>>s;int n=s.size();
		cout<<dfs(0,n-1)<<endl;
	}
	return 0;
}
```
