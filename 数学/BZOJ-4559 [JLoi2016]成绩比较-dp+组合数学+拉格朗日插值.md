# [BZOJ-4559 [JLoi2016]成绩比较-dp+组合数学+拉格朗日插值](https://www.lydsy.com/JudgeOnline/problem.php?id=4559)

## 【Description】

```cpp
G系共有n位同学，M门必修课。这N位同学的编号为0到N-1的整数，其中B神的编号为0号。这M门必修课编号为0到M-
1的整数。一位同学在必修课上可以获得的分数是1到Ui中的一个整数。如果在每门课上A获得的成绩均小于等于B获
得的成绩，则称A被B碾压。在B神的说法中，G系共有K位同学被他碾压（不包括他自己），而其他N-K-1位同学则没
有被他碾压。D神查到了B神每门必修课的排名。这里的排名是指：如果B神某门课的排名为R，则表示有且仅有R-1
位同学这门课的分数大于B神的分数，有且仅有N-R位同学这门课的分数小于等于B神（不包括他自己）。我们需要
求出全系所有同学每门必修课得分的情况数，使其既能满足B神的说法，也能符合D神查到的排名。这里两种情况不
同当且仅当有任意一位同学在任意一门课上获得的分数不同。你不需要像D神那么厉害，你只需要计算出情况数模1
0^9+7的余数就可以了。
```

## 【Input】

```cpp
第一行包含三个正整数N,M,K，分别表示G系的同学数量（包括B神），必修课的数量和被B神碾压的同学数量。第二
行包含M个正整数，依次表示每门课的最高分Ui。第三行包含M个正整数，依次表示B神在每门课上的排名Ri。保证1
≤Ri≤N。数据保证至少有1种情况使得B神说的话成立。N<=100,M<=100,Ui<=10^9
```

## 【Output】

```cpp
仅一行一个正整数，表示满足条件的情况数模10^9+7的余数。
```

------



## 【Examples】 

#### Sample Input

```cpp
3 2 1 
2 2 
1 2 
```

#### Sample Output

```cpp
10
```

------



## 【Problem Description】

```cpp
略
```

## 【Solution】

```cpp
定义dp数组f[i][j]表示，前i门课碾压j个人的方案数
则动态转移方程为：
```

$$
f[i][j]=f[i-1][k]\cdot C_k^j\cdot C_{n-k-1}^{n-rank[i]-j}\cdot R(i)
$$

```cpp
表示当新增加一门课程时，碾压人数从k变为了j，即有C(k,j)种方案。
对于第i门课来说，有n-rank[i]个人的分数小于等于B神，其中j个人的所有课的分数都要小于等于B神，剩余
n-rank[i]-j个人至少存在一门课的分数大于B神，这些大于B神的课一定在前i-1门课中，总人数就是未被碾压的人数
=n-k-1。有C(n-k-1,n-rank[i]-j)种方案。
其中R(i)为：
```

------

$$
R(i)=\sum_{x=1}^{u[i]}x^{n-rank[i]}\cdot(u[i]-x)^{rank[i]-1}
$$

```cpp
其中x为B神这门课的分数，有n-rank[i]个人的分数需要小于等于B神的分数，每个人有x种分数可以选择，所以有
x^{n-rank[i]}中方案，有rank[i]-1个人的分数需要大于B神的分数，每个人有u[i]-x种分数可以选择，所以有
（u[i]-x)^{rank[i]-1}种方案。
而u[i]高达1e9所以没法直接算，需要用拉格朗日插值。
根据R(i)可知，其多项式次数最大不超过99，所以我们可以预处理处前100项的值，用拉格朗日插值法求得答案。
```



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-07-05 20:36:09 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-07-06 14:12:39
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 105
const int mod=1e9+7;
int u[maxn]/*每门课的满分*/,rk[maxn]/*B神每门课的排名*/,f[maxn][maxn]/*前i门课，碾压j个人的方案数*/,y[maxn]/*拉格朗日插值前100项的值*/;
inline int fpow(int a,int b,int mod){
    a%=mod;int ans=1;
    while(b){
        if(b&1) (ans*=a)%=mod;
        (a*=a)%=mod;
        b>>=1;
    }
    return ans;
}
inline int Inv(int a,int p){
    return fpow(a,p-2,p);
}
int C[maxn][maxn];/*预处理的组合数*/
int bit[maxn];/*预处理的阶乘*/
int Lagrangian(int y[]/*值域*/,int n/*变量*/,int k/*待求y_k*/,int mod){
    if(k<=n) return y[k];
    int ubit[maxn]={k};/*预处理前n项反k阶乘即：(k-0)*(k-1)*...*(k-k)*/;
    for(int i=1;i<=n;i++) ubit[i]=ubit[i-1]*(k-i)%mod;
    int ans=0;
    for(int i=1;i<=n;i++){ /*拉格朗日插值多项式*/
        int s1=y[i]*ubit[i-1]%mod*ubit[n]%mod*fpow(ubit[i],mod-2,mod)%mod; /*分子*/
        int s2=bit[i]*bit[n-i]%mod; /*分母*/
        (ans+=((n-i)&1?-1:1)*s1%mod*fpow(s2,mod-2,mod)%mod)%=mod;
    }
    return (ans+mod)%mod;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m,d;scanf("%lld%lld%lld",&n,&m,&d);
    C[0][0] = 1; 
    for (int i = 1; i <= n + 1; i++) {
        C[i][0] = 1;
        for (int j = 1; j <= i; j++) C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % mod;
    }
    bit[0]=1;
    for(int i=1;i<=100;i++){
        bit[i]=bit[i-1]*i%mod;
    }
    for(int i=1;i<=m;i++) scanf("%lld",u+i);
    for(int i=1;i<=m;i++) scanf("%lld",rk+i);
    f[0][n-1]=1;/*初始化，0门课，全碾压时的方案数为1*/
    for(int i=1;i<=m;i++){
        for(int j=1;j<=100;j++){/*预处理前100项的值，用于拉格朗日插值*/
            y[j]=(y[j-1]+fpow(j,n-rk[i],mod)*fpow(u[i]-j,rk[i]-1,mod)%mod)%mod;
        }
        int P=Lagrangian(y,100,u[i],mod);/*拉格朗日插值求第u[i]项*/
        // cout<<P<<endl;
        for(int j=d;j<=n;j++){
            for(int k=j;k<=n;k++){
                if(n-rk[i]-j>=0) (f[i][j]+=f[i-1][k]*C[k][j]%mod*C[n-k-1][n-rk[i]-j]%mod*P%mod)%=mod;
            }
        }
    }
    printf("%lld\n",f[m][d]);
    cin.get(),cin.get();
    return 0;
}
```
