#  [ ACM-ICPC Asia Beijing Regional Contest 2017-J-Pangu and Stones-区间dp-合并石子进阶](https://vjudge.net/problem/HihoCoder-1636)

[TOC]



## Description 

> ```
>     In Chinese mythology, Pangu is the first living being and the creator of the sky and
>  the earth. He woke up from an egg and split the egg into two parts: the sky and the earth.
> 
>     At the beginning, there was no mountain on the earth, only stones all over the land.
> 
>     There were N piles of stones, numbered from 1 to N. Pangu wanted to merge all of them 
> into one pile to build a great mountain. If the sum of stones of some piles was S, Pangu 
> would need S seconds to pile them into one pile, and there would be S stones in the new 
> pile.
> 
>     Unfortunately, every time Pangu could only merge successive piles into one pile. And the number of piles he merged shouldn't be less than L or greater than R.
> 
>     Pangu wanted to finish this as soon as possible.
> 
>     Can you help him? If there was no solution, you should answer '0'.
> 
> ```

## Input

> ```
> 
> 
>     There are multiple test cases.
> 
>     The first line of each case contains three integers N,L,R as above mentioned 
>     (2<=N<=100,2<=L<=R<=N).
> 
>     The second line of each case contains N integers a1,a2 …aN (1<= ai  <=1000,i= 1…N ),
>     indicating the number of stones of  pile 1, pile 2 …pile N.
> 
>     The number of test cases is less than 110 and there are at most 5 test cases in which N
>     >= 50.
> 
> ```

## Output

> ```
> For each test case, you should output the minimum time(in seconds) Pangu had to take . If
> it was impossible for Pangu to do his job, you should output  0.
> ```

------



## Examples 

> ### Input

> ```
> 3 2 2
> 1 2 3
> 3 2 3
> 1 2 3
> 4 3 3
> 1 2 3 4
> ```

> ###Output

> ```
> 9
> 6
> 0
> ```

------



## Problem Description

> ```
> 合并石子进阶版。
> 给你一堆石子，可以合并连续的X堆，L<=x<=R,每次合并所花费的时间是所合并的石子的数量。求最小花费时间。
> ```

## Solution

> ```
> 区间dp
> 先不去管[L,R]的限制，将所有情况通过dp枚举以后，再在题目要求的[L,R]范围里求得答案就好。具体看code。
> ```

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-20 13:30:43 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-20 14:43:25
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 105
int a[maxn];
int dp[maxn][maxn][maxn];//dp[i][j][k],表示将i~j合并为k堆所需的最小花费
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,l,r;
    while(cin>>n>>l>>r)
    {
        memset(dp,INF,sizeof(dp));
        for(int i=1;i<=n;i++)
        {
            dp[i][i][1]=0;
            cin>>a[i];
            a[i]+=a[i-1];
        }
        for(int x=l-1;x<r;x++)//初始化
        {
            for(int i=1;i+x<=n;i++)
            {
                int j=x+i;
                dp[i][j][x+1]=0;// x堆石子，分为x堆的最小花费为0
                dp[i][j][1]=a[j]-a[i-1];//将所有石子合并为一堆的最小花费为其和
            }
        }
        for(int x=1;x<n;x++)//枚举区间长度
        {
            for(int i=1;i+x<=n;i++)
            {
                int j=x+i;
                for(int k=i;k<j;k++)
                {
                    for(int c=2;c<=x+1;c++)//先对相应长度进行任意划分
                    {
                        dp[i][j][c]=min(dp[i][j][c],dp[i][k][c-1]+dp[k+1][j][1]);
                    }
                }
                for(int c=l;c<=r;c++)//最后计算满足条件的值
                        dp[i][j][1]=min(dp[i][j][1],dp[i][j][c]+a[j]-a[i-1]);
            }
        }
        if(dp[1][n][1]>=INF)
        {
            cout<<0<<endl;
        }
        else cout<<dp[1][n][1]<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
