# [2018-2019 ACM-ICPC, Asia Xuzhou Regional Contest- H. Rikka with A Long Colour Palette -思维+贪心](https://codeforces.com/gym/102012) 

![](H:\GitHub\Algorithm\GYM\https___codeforces.com_gym_102012_problem_H.png)

------



## 【Problem Description】

有$k$种颜色，给你$n$个区间段，选择一种合适的方案给每个区间段染色，使得最终染色次数等于$k$次的长度和最大。

## 【Solution】

将左右端点放在一起排序，但是标记出它是左端点，还是右端点，并且记录每个端点属于第几个区间，排序后，对于每个左端点，从未使用过的颜色中选一个染色此区间，若连续染色超过$k$次，则多于的区间放入栈中备用，当某次染色小于$k$次时拿出来染色。当遇到右端点时，若这段区间染过色，则将此颜色收回，若没染过色，随意染色即可。

**最重要的，一定要按格式输出，末尾不能有空格**

------



## 【Code】

```cpp
/*
 * @Author: _Simon_
 * @Date: 2019-10-30 10:44:14 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-10-30 10:44:14
 */ 
#include<bits/stdc++.h>
using namespace std;
#define int long long
#define maxn 200005
#define INF 0x3f3f3f3f
struct node{
	int first,second,ord;
	node(){}
	node(int first,int second,int ord):first(first),second(second),ord(ord){}
	bool operator <(const node&a) const{
		if(first==a.first) return second<a.second;
		return first<a.first;
	}
};
vector<node>a;
int ans[maxn];
signed main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	int T;cin>>T;
	while(T--){
		memset(ans,0,sizeof(ans));
		a.clear();
		int n,k;cin>>n>>k;
		for(int i=1;i<=n;i++){
			int l,r;cin>>l>>r;
			a.push_back({l,1,i});
			a.push_back({r,-1,i});
		}
		sort(a.begin(),a.end());
		queue<int>q;stack<int>st;
		for(int i=1;i<=k;i++) q.push(i);
		int tmp=0,pret=0,preidx=0;
		for(int i=0;i<a.size();i++){
			if(a[i].second>0){ //左端点
				if(tmp<k){ //连续染色小于k次
					tmp++;
					ans[a[i].ord]=q.front();q.pop(); //从未染色的颜色中选出一个染色
				}else{
					st.push(a[i].ord); //染色大于k次，备用
				}
			}else{ //右端点
				if(ans[a[i].ord]){ //若所在区间染过色
					q.push(ans[a[i].ord]); //收回颜色
					tmp--;
				}else{
					ans[a[i].ord]=1; //否则随意染色
				}
			}
			if(i+1<a.size()&&a[i].first==a[i+1].first) continue; //同样端点一起处理
			while(tmp<k){ //若染色小于k次，从备用端点中选取
				while(!st.empty()&&ans[st.top()]) st.pop();
				if(st.empty()) break;
				tmp++;
				ans[st.top()]=q.front();st.pop();q.pop();
			}
			if(pret>=k) ans[0]+=a[i].first-preidx; //计算染色次数等于k次的长度
			preidx=a[i].first;pret=tmp;
		}
		cout<<ans[0]<<endl;
		for(int i=1;i<=n;i++) cout<<ans[i]<<" \n"[i==n]; //按格式输出
	}
	return 0;
}

```
