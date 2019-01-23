#  [Educational Codeforces Round 49 (Rated for Div. 2)-E. Inverse Coloring-基础dp](http://codeforces.com/contest/1027/problem/E)

[TOC]



## Description 

> ```
> You are given a square board, consisting of n rows and n
> 
> columns. Each tile in it should be colored either white or black.
> 
> Let's call some coloring beautiful if each pair of adjacent rows are either the same or
> different in every position. The same condition should be held for the columns as well.
> 
> Let's call some coloring suitable if it is beautiful and there is no rectangle of the
> single color, consisting of at least k tiles.
> 
> Your task is to count the number of suitable colorings of the board of the given size.
> 
> Since the answer can be very large, print it modulo 998244353.
> ```

## Input

> ```
> A single line contains two integers n and k (1≤n≤500, 1≤k≤n^2) — the number of rows and 
> columns of the board and the maximum number of tiles inside the rectangle of the single
> color, respectively.
> ```

## Output

> ```
> Print a single integer — the number of suitable colorings of the board of the given size
> modulo 998244353.
> ```

------



## Examples 

> ### Input

> ```
> 1 1
> ```

> ###Output

> ```
> 0
> ```

> ###Input

> ```m
> 2 3
> ```

> ###Output

> ```
> 6
> ```

> ###Input

> ```
> 49 1808
> ```

> ###Output

> ```
> 359087121
> ```

------



## Problem Description

> ```
> 给你n*n的矩阵格子，矩阵能只能填入黑色或白色，并且任意两行或两列的颜色要么相同，要么相反。求使得矩阵
> 内连续的相同颜色形成的矩形的面积小于k的填法次数。
> ```

## Solution

> ```
> 基础dp
> 矩形面积就是长*宽，也就是连续黑色或白色方块的个数。
> 所以我们通过dp算出所有可能情况，枚举相乘，保证面积小于k，求和相加即可得到答案。
> ```

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-22 19:42:38 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-22 21:31:02
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 505
#define mod 998244353
int dp[maxn][maxn];//前i个长度，最长连续不超过j的方案数
int ans[maxn];
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,k;
    cin>>n>>k;
    for(int i=1;i<=n;i++) dp[0][i]=1;
    for(int i=1;i<=n;i++)//枚举所有情况
    {
        for(int j=1;j<=n;j++)
        {
            for(int k=1;k<=min(i,j);k++)
            {
                dp[i][j]=(dp[i][j]+dp[i-k][j])%mod;
            }
        }
    }
    for(int i=1;i<=n;i++)
    {
        ans[i]=(dp[n][i]-dp[n][i-1]+mod)%mod;//前n个长度，连续个数为i的方案数。
    }
    int cnt=0;
    for(int i=1;i<=n;i++)//枚举长和宽相乘
    {
        for(int j=1;j<=n;j++)
        {
            if(i*j>=k) continue;//保证面积大小符合条件
            cnt=(cnt+ans[i]*ans[j]%mod)%mod;//求和最终方案数
        }
    }
    cout<<(cnt*2)%mod<<endl;//乘2是因为，要加上整张图颜色全部取反的方案数
    cin.get(),cin.get();
    return 0;
}
```
