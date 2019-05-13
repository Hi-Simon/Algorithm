# [2018-ACM-CCPC-湘潭邀请赛-I-Longest Increasing Subsequence-思维-最长上升子序列变形](<https://vjudge.net/problem/HDU-6284>)

## 【Description】

```cpp
Bobo has a sequence a1,a2,…,an. 
Let f(x) be the length of longest *strictly* increasing subsequence after replacing all
the occurrence of 0 with x. 
He would like to find ∑ni=1i⋅f(i). 

Note that the length of longest strictly increasing subsequence of sequence s1,s2,…,sm
isthe largest k 
such that there exists 1≤i1<i2<⋯<ik≤m satisfying si1<si2<⋯<sik. 
```

## 【Input】

```cpp
The input consists of several test cases and is terminated by end-of-file. 

The first line of each test case contains an integer n. 
The second line contains n integers a1,a2,…,an. 
```

## 【Output】

```cpp
For each test case, print an integer which denotes the result. 

Constraint 

* 1≤n≤105 
* 0≤ai≤n 
* The sum of n does not exceed 250,000. 
```

------



## 【Examples】 

#### Sample Input

```cpp
2
1 1
3
1 0 3
6
4 0 6 1 0 3
```

#### Sample Output

```cpp
3
14
49
```

------



## 【Problem Description】

```cpp
给你一个长度为n的序列，定义f(i)表示将序列中的数字0 替换为 i的最长严格上升子序列长度。
求和 1*f(1)+2*f(2)+……+n*f(n)
```

## 【Solution】

```cpp
首先，len<=f(x)<=len+1是显然的。其中len为初始序列去除0之后的最长严格上升子序列

然后，对于每个x，我们说它对答案有额外的贡献，当且仅当f(x)=len+1。而要存在某个x使得f(x)=len+1,当且仅当存
在某个为0的位置，在它之前的最长上升子序列长度为t，此子序列末尾元素为a；在它之后的最长上升子序列为len-t,此
子序列起始元素为b,并且a<b-1,例如：1 0 4 5，len=3,t=1,a=1,b=4。
对答案有额外贡献的x有2，3。

所以做法就明确了：
	nlog(n)预处理出，以i位置为终点的最长严格上升子序列长度a[i]
				   以i位置为起点的最长严格上升子序列长度b[i]
				   记c[i]为原始数组
				   
	对于每个位置i找到在它之后的且b[j]=len-a[i]的位置j，且c[j]的值最大的那个。那么对答案有额外贡献的x区
间就为[a[i]+1,c[j]-1](可以根据差分思想定义一个d[]数组，使得d[a[i]+1]++,d[c[j]]--;最后求一个前缀和，
d[i]大于0的位置即对答案有额外贡献的位置。)
    那么怎么得到c[j]呢？ 可以用一个额外的数组f[],记录f[len-a[i]]的最大值即可。
    	
	由于找的位置j一定在i的后面，所以要从后往前扫，同时在遇到0之后，维护f数组。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-05-07 17:30:19 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-05-07 22:07:16
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int a[maxn],b[maxn],c[maxn],f[maxn],len=1,d[maxn];
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    while(~scanf("%lld",&n)){
        memset(a,0,sizeof(int)*(n+2));
        memset(b,0,sizeof(int)*(n+2));
        memset(d,0,sizeof(int)*(n+2));
        for(int i=1;i<=n;i++) scanf("%lld",c+i);
        memset(f,INF,sizeof(int)*(n+2));len=1;
        for(int i=1;i<=n;i++){ //nlog(n)求最长上升子序列
            if(!c[i]) continue; //不处理0
            int j=lower_bound(f+1,f+n+1,c[i])-f;
            a[i]=j;f[j]=c[i]; //以i为终点的最长严格上升子序列长度为j
        }
        memset(f,0,sizeof(int)*(n+2));
        for(int i=n;i>=1;i--){
            if(!c[i]) continue; //不处理0
            int j=lower_bound(f+1,f+n+1,-c[i])-f;
            b[i]=j;f[j]=-c[i]; //以i为起点的最长严格上升子序列的长度为j
        }
        while(f[len+1]) len++; //去除0之后的最长上升子序列长度
        memset(f,0,sizeof(int)*(n+2));
        for(int i=n;i>=1;i--){ //从后往前扫
            int j=i;
            while(c[j]){
                int t=len-a[j]; 
                if(f[t]-1>c[j]){ 
                    d[c[j]+1]++;d[f[t]]--; //有额外贡献的区间
                }
                j--;
            }
            f[0]=n+1;//a[j]=len时的f值 例如，1，2，0，0
            for(int k=i;k>j;k--) f[b[k]]=max(f[b[k]],c[k]); //更新f数组最大值
            if(f[len]-1>c[j]&&j){ //c[j]=0时，a[j]=0。例如，0 1
                d[c[j]+1]++;d[f[len]]--;
            }
            i=j;
        }
        int ans=0;
        for(int i=1;i<=n;i++) d[i]+=d[i-1],ans+=i*(d[i]?len+1:len);
        printf("%lld\n",ans);
    }
    cin.get(),cin.get();
    return 0;
}
```
