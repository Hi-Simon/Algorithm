#  [2018-ACM-ICPC-焦作赛区网络赛-K. Transport Ship-多重背包裸题？](https://nanti.jisuanke.com/t/31720)



## 【Description】

> ```
> There are NNN different kinds of transport ships on the port. The ithi^{th}ith kind 
> of ship can carry the weight of V[i]V[i]V[i] and the number of the ithi^{th}ith 
> kind of ship is 2C[i]−12^{C[i]} - 12C[i]−1. How many different schemes there are if
> you want to use these ships to transport cargo with a total weight of SSS?
> 
> It is required that each ship must be full-filled. Two schemes are considered to be
> the same if they use the same kinds of ships and the same number for each kind.
> ```

## 【Input】

> ```
> The first line contains an integer T(1≤T≤20)T(1 \le T \le 20)T(1≤T≤20), which is 
> the number of test cases.
> 
> For each test case:
> 
> The first line contains two integers: N(1≤N≤20),Q(1≤Q≤10000)N(1 \le N \le 20), Q(1
> \le Q \le 10000)N(1≤N≤20),Q(1≤Q≤10000), representing the number of kinds of ships
> and the number of queries.
> 
> For the next NNN lines, each line contains two integers: V[i](1≤V[i]≤20),C[i]
> (1≤C[i]≤20)V[i](1 \le V[i] \le 20), C[i](1 \le C[i] \le 20)V[i](1≤V[i]≤20),C[i]
> (1≤C[i]≤20), representing the weight the ithi^{th}ith kind of ship can carry, and
> the number of the ithi^{th}ith kind of ship is 2C[i]−12^{C[i]} - 12C[i]−1.
> 
> For the next QQQ lines, each line contains a single integer: S(1≤S≤10000)S(1 \le S
> \le 10000)S(1≤S≤10000), representing the queried weight.
> ```

## 【Output】

> ```
> For each query, output one line containing a single integer which represents the
> number of schemes for arranging ships. Since the answer may be very large, output
> the answer modulo 1000000007。
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 1
> 1 2
> 2 1
> 1
> 2
> ```

> ###Sample Output

> ```
> 0
> 1
> ```

------



## 【Problem Description】

> ```
> n种船，每种船的数量为2^c[i]-1,容量为v[i]
> 有q个询问，每次给你一个s代表总的需要运输的体积。
> 用这些船来运输物品，问有多少种不同的方案使得总运输物品的体积刚好为s。
> ps：每艘船每次运输时必须完全装满。
> ```

## 【Solution】

> ```
> 妥妥的多重背包裸题啊，，
> 定义dp[k]为运输总容量为k时的方案数.
> 对于每种船有2^c[i]-1个，那么它等于:
> ```
>
> $$
> 2^{c[i]}-1=1+2^1+2^2+\dots+2^{c[i]-1}
> $$
>
> ```
> 所以可以将每一种船i分为个数都为1个，容量分别为1*v[i],2*v[i],4*v[i],2^(c[i]-1)*v[i]的船。
> 就转换为了01背包问题。
> 状态转移方程：
> dp[k]=(dp[k]+dp[k-t*v[i]])%mod;
> （其实说的都是废话，背包九讲里面很清楚....)
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-15 21:21:15 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-15 22:08:34
 */
#include<bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 105
const int mod=1e9+7;
int v[maxn],c[maxn];
int dp[10005];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n,q;
        scanf("%d%d",&n,&q);
        for(int i=1;i<=n;i++)
        {
            scanf("%d%d",v+i,c+i);
        }
        memset(dp,0,sizeof(dp));
        dp[0]=1;
        for(int i=1;i<=n;i++)
        {
            int t=1;
            for(int j=1;j<=c[i];j++)
            {
                for(int k=10000;k>=t*v[i];k--)
                {
                    dp[k]=(dp[k]+dp[k-t*v[i]])%mod;
                }
                t<<=1;
            }
        }
        while(q--)
        {
            int x;
            scanf("%d",&x);
            printf("%d\n",dp[x]);
        }
    }
    cin.get(),cin.get();
    return 0;
}
```
