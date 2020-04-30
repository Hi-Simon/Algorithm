# [Codeforces Round #598 (Div. 3)- E. Yet Another Division Into Teams - 动态规划](https://codeforces.com/contest/1256) 

![](H:\GitHub\Algorithm\Codeforces\https___codeforces.com_contest_1256_problem_E.png)

------



## 【Problem Description】

给你$n$个数，将其划分为多组，对于每个组定义其$d$值为 组内的最大值减最小值，问如何划分使得最终所有组的$d$值之和最小。每个组至少要保证有$3$个数。

## 【Solution】

将所有值从小到大排序，然后我们知道最多有$5$个人划分到同一组中，如果有$6$个人，那么划分为两组一定比划分为一组更优。

定义$dp[i]$表示前$i-1$个人划分后的最小$d$值和为$dp[i]$，假设前$i-1$个人已经划分好了，然后就是确定哪些人与第$i$个人分为一组，题目要求至少$3$个人，而我们又知道最多$5$个人，所以枚举第$j\in[i+2,i+4]$个人，选择$a[j]-a[i]$最小的那个$j$，将$[i,j]$这些人分为一组即可。

------



## 【Code】

```cpp
/*
 * @Author: _Simon_
 * @Date: 2019-11-06 10:55:21 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-11-06 10:55:21
 */ 
#include<bits/stdc++.h>
using namespace std;
#define int long long
#define maxn 200005
#define INF 0x3f3f3f3f
pair<int,int>a[maxn];
int dp[maxn],p[maxn]; //dp[i]表示前i-1个人划分好后的最小d值和
int ans[maxn]/*每个人分在第几组*/,root,cnt/*总共有多少个组*/; 
signed main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int n;cin>>n;
	for(int i=1;i<=n;i++) cin>>a[i].first,a[i].second=i;
	sort(a+1,a+n+1); //从小到大排序
	memset(dp,INF,sizeof(dp));dp[1]=0;
	for(int i=1;i<=n;i++){
		for(int j=2;j<=4&&i+j<=n;j++){
			int diff=a[i+j].first-a[i].first;
			if(dp[i+j+1]>dp[i]+diff){
				dp[i+j+1]=dp[i]+diff;
				p[i+j+1]=i; //记录方案，表示[i,i+j]这些人分为一组
			}
		}
	}
	root=n+1;cnt=1;
	while(root!=1){
		for(int i=root-1;i>=p[root];i--){ //[p[root], root-1]这些人为同一组
			ans[a[i].second]=cnt;
		}
		cnt++;root=p[root]; //枚举下一组
	}
	cout<<dp[n+1]<<' '<<cnt-1<<endl;
	for(int i=1;i<=n;i++) cout<<ans[i]<<' ';cout<<endl;
	return 0;
}
```
