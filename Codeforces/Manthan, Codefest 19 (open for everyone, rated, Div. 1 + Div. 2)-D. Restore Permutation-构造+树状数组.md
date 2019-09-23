# [Manthan, Codefest 19 (open for everyone, rated, Div. 1 + Div. 2)-D. Restore Permutation-构造+树状数组](https://codeforces.com/contest/1208)

![](H:\GitHub\Algorithm\Codeforces\Problem - D - Codeforces.png)

------



## 【Problem Description】

​		给你一个长度为$n$的数组，第$i$个元素$s_i$表示一个排列中第$i$个元素之前，并且小于$p_i$的元素的和。求出满足此条件的排列。

## 【Solution】

​		假设$n=5$，$s[]=\{0,0,3,7,3\}$。从后往前看，最后一个值为$3$，表示存在一个数$x$，使得$1+2+\dots+x=3$。易知$x=2$，所以$p_5=x+1=3$。然后第二个值为$7$，表示存在一个数$x$，使得$1+2+\dots +x-3=7$。减$3$是因为$p_5=3$，不可能出现在$p_4$之前。所以可知$x=4$，所以$p_4=x+1=5$。同理利用此方法可以求出此排列。

​		上述方法其实就是在找一个最大的$x$，使得将所有满足$1\le y\le x$，并且未出现过的$y$值相加使其等于$s_i$，则$p_i=x+1$。可以想到用树状数组维护前缀和，用二分查找最大的满足条件的$x$。每求得一个数，就将此数清零即可。

​		但其实用树状数组中数组的特性，有更巧妙的方法。我们知道在树状数组中，对于数组$tree[i]$，它所维护的区间为$[i-lowbit(i)+1,i]$，所以对于$tree[2^i]$，它所维护的区间就为$[1,2^i]$。所以就可以利用此特性加上树状数组的操作，维护一个类似倍增方法，并且支持在线修改操作。

​		例如求$sum[1+2+\dots10]=tree[2^3]+tree[2^3+2^1]$

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-26 18:14:20 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-26 20:12:53
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 200005
int a[maxn],tree[maxn],ans[maxn];
inline int lowbit(int x){
    return x&(-x);
}
inline void update(int x,int val){
    for(int i=x;i<maxn;i+=lowbit(i)){
        tree[i]+=val;
    }
}
inline int query(int x){
    int ans=0;
    for(int i=x;i>0;i-=lowbit(i)){
        ans+=tree[i];
    }
    return ans;
}
int solve(int k,int n){
    int num=0,sum=0;
    for(int i=21;i>=0;i--){
        if(num+(1<<i)<=n&&sum+tree[num+(1<<i)]<=k){//求一个最大num，使得sum[1+2+...+num]=k
            num+=1<<i;
            sum+=tree[num];
        }
    }
    return num+1;
}
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;cin>>n;
    for(int i=1;i<=n;i++){
        update(i,i)/*初始所有值都存在*/;cin>>a[i];
    }
    for(int i=n;i>=1;i--){
        ans[i]=solve(a[i],n);
        update(ans[i],-ans[i]);//将ans[i]清零。
    }
    for(int i=1;i<=n;i++) cout<<ans[i]<<' ';
    cout<<endl;
#ifndef ONLINE_JUDGE
    system("pause");
#endif
    return 0;
}
```
