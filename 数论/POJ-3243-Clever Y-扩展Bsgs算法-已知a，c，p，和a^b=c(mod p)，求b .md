#  [POJ-3243-Clever Y-扩展Bsgs算法-已知a，c，p，和a^b=c(mod p)，求b ](https://vjudge.net/problem/POJ-3243)



## 【Description】

```c++
Little Y finds there is a very interesting formula in mathematics:
```

$$
X^Y mod\:Z=K
$$

```
Given X, Y, Z, we all know how to figure out K fast. However, given X, Z, K, could you
figure out Y fast?
```

## 【Input】

```c++
Input data consists of no more than 20 test cases. For each test case, there would be 
only one line containing 3 integers X, Z, K (0 ≤ X, Z, K ≤ 10^9).
Input file ends with 3 zeros separated by spaces. 
```

## 【Output】

```c++
For each test case output one line. Write "No Solution" (without quotes) if you cannot
find a feasible Y (0 ≤ Y < Z). Otherwise output the minimum Y you find. 
```

------



## 【Examples】 

#### Sample Input

```c++
5 58 33
2 4 3
0 0 0
```

#### Sample Output

```c++
9
No Solution
```

------



## 【Problem Description】

```c++
对于题目给的式子，已知X,Z,K,求Y.
```

## 【Solution】

```c++
扩展BSGS
因为X,Z不一定为互质，无法求逆元。所以不能直接用BSGS来做，需要做一些处理。
```

$$
X^Y=K(mod\:Z)=
X^{Y-1}\frac{X}{d}=\frac{K}{d}(mod\frac{Z}{d}),其中d=gcd(X,Z).\\
不断重复此过程，直到gcd(X,\frac{Z}{\prod_{i}d_i})=1,即：\\
X^{Y-k}\frac{X^k}{\prod_{i}d_i}=\frac{K}{\prod_{i}d_i}(mod \frac{Z}{\prod_{i}d_i})\\
注意在此过程中，对于每个d_i,K\%d_i==0,若K\%d_i!=0,则无解\\
最后去掉分母得：\\
X^Y=K(mod \frac{Z}{\prod_{i}d_i}),将其直接应用BSGS即可。
$$

```c++
BSGS:
```

$$
a^b=c(mod\:p)\\
令m=\lceil\sqrt{q}\rceil,则可以暴力求出c=[1,m)的时候的b，用map保存起来。\\
c=[m,p]时：\\
必然等于a^ia^{km}=c(mod\:p)\\
则a^i=c\:a^{-km}(mod\: p)(其中i<m)\\
即c=[m,p]时通过已求得的值来查询他们的答案。知道ca^{-km}的值即可知道i的值。\\
ca^{-km}的值可以通过求a^m的逆元乘以c，然后枚举次数k即可。
$$

```c++
用STL的map会超时，所以得手写HashMap.
```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-27 20:45:58 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-27 21:46:57
 */
#include<iostream>
#include<cstring>
#include<algorithm>
#include<map>
#include<cstdio>
#include<cmath>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 1000005
const int mod = 611977;
struct HashMap //手写HashMap
{
    int head[mod+5],key[maxn],value[maxn],nxt[maxn],tol;
    inline void clear()
    {
        tol=0;memset(head,-1,sizeof(head));
    }
    HashMap()
    {
        clear();
    }
    inline void insert(int k,int v)
    {
        int idx=k%mod;
        for(int i=head[idx];~i;i=nxt[i])
        {
            if(key[i]==k) 
            {
                value[i]=min(value[i],v);
                return ;
            }
        }
        key[tol]=k;value[tol]=v;nxt[tol]=head[idx];head[idx]=tol++;
    }
    inline int operator [](const int &k) const
    {
        int idx=k%mod;
        for(int i=head[idx];~i;i=nxt[i])
        {
            if(key[i]==k) return value[i];
        }
        return -1;
    }
}mp;
inline int fpow(int a,int b,int mod) //快速幂
{
    a%=mod;int ans=1;
    while(b)
    {
        if(b&1) ans=ans*a%mod;
        a=a*a%mod;
        b>>=1;
    }
    return ans;
}
inline int exgcd(int a,int b,int &x,int &y) //扩展欧几里得
{
    if(b==0)
    {
        x=1,y=0;
        return a;
    }
    int ans=exgcd(b,a%b,y,x);
    y-=a/b*x;
    return ans;
}
inline int Bsgs(int a,int b,int mod)
{
    a%=mod,b%=mod;
    if(b==1) return 0; 
    int m=ceil(sqrt(mod)),inv,y;exgcd(fpow(a,m,mod),mod,inv,y);//扩展欧几里得求a^m的逆元
    inv=(inv%mod+mod)%mod;
    mp.insert(1,0); //b==1时，答案为0
    for(int i=1,e=1;i<m;i++) //暴力求[1,m)的答案
    {
        e=e*a%mod;
        if(mp[e]==-1) mp.insert(e,i);
    }
    for(int k=0;k<=m;k++) //查询答案
    {
        if(mp[b]!=-1) //如果存在
        {
            int ans=mp[b]; 
            return ans+k*m; //返回答案
        }
        b=b*inv%mod; //不断叠加次数
    }
    return -1;
}
inline int gcd(int a,int b)
{
    return b==0?a:gcd(b,a%b);
}
inline int exBsgs(int a,int b,int mod) //扩展BSGS, 处理a，mod不互质的情况
{
    if(b==1) return 0;
    for(int g=gcd(a,mod),i=0;g!=1;g=gcd(a,mod),i++) 
    {
        if(b%g) return -1;//保证g为a,b,mod的最大公约数
        mod/=g;
    }
    return Bsgs(a,b,mod);
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int x,z,k;
    while(scanf("%lld%lld%lld",&x,&z,&k)!=EOF&&(x+z+k))
    {
        mp.clear();
        int ans=exBsgs(x,k,z); 
        if(ans!=-1) printf("%lld\n",ans);
        else puts("No Solution");
    }
    cin.get(),cin.get();
    return 0;
}
```
