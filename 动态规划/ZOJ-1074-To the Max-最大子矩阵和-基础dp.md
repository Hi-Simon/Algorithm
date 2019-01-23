#  [ZOJ-1074-To the Max-最大子矩阵和-基础dp](https://vjudge.net/problem/ZOJ-1074)

[TOC]



## Description 

> ```
> 
> Given a two-dimensional array of positive and negative integers, a sub-rectangle is
> any contiguous sub-array of size 1 x 1 or greater located within the whole array. 
> The sum of a rectangle is the sum of all the elements in that rectangle. In this 
> problem the sub-rectangle with the largest sum is referred to as the maximal sub-
> rectangle.
> 
> As an example, the maximal sub-rectangle of the array:
> 
> 0 -2 -7 0
> 9 2 -6 2
> -4 1 -4 1
> -1 8 0 -2
> 
> is in the lower left corner:
> 
> 9 2
> -4 1
> -1 8
> 
> and has a sum of 15.
> ```

## Input

> ```
> he input consists of an N x N array of integers. The input begins with a single 
> positive integer N on a line by itself, indicating the size of the square two-
> dimensional array. This is followed by N 2 integers separated by whitespace (spaces 
> and newlines). These are the N 2 integers of the array, presented in row-major 
> order. That is, all numbers in the first row, left to right, then all numbers in 
> the second row, left to right, etc. N may be as large as 100. The numbers in the 
> array will be in the range [-127,127].
> ```

## Output

> ```
> Output the sum of the maximal sub-rectangle.
> ```

------



## Examples 

> ### Input

> ```
> 4
> 0 -2 -7 0 9 2 -6 2
> -4 1 -4 1 -1
> 8 0 -2
> ```

> ###Output

> ```
> 15 
> ```

------



## Problem Description

> ```
> 求最大子矩阵和
> ```

## Solution

> ```
> 基础dp
> 类似求最大子序列和，将所有数纵向压缩到一行便可以用最大子序列和的方法求最大值了。
> 具体做法就是枚举行i，j。i为压缩的起始行，j为压缩的结束行，即将[i,j]行压缩到i行。然后每次压缩后用最
> 大子序列和的方法求个最大值。最终输出最大的即为答案。
> ```

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-24 21:25:05 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-24 21:42:41
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 105
int dp[maxn][maxn];;
int sum[maxn];
int Maxsub(int r,int n)
{
    for(int c=1;c<=n;c++)//sum[c]即为[i,j]列的和
        sum[c]+=dp[r][c];
    int ans=0,Max=0;
    for(int c=1;c<=n;c++)//最大子序列和
    {
        ans+=sum[c];
        if(ans<0) ans=0;
        Max=max(Max,ans);
    }
    return Max;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=n;j++)
        {
            cin>>dp[i][j];
        }
    }
    int ans=0;
    for(int r1=1;r1<=n;r1++)//压缩起始行
    {
        memset(sum,0,sizeof(sum));
        for(int r2=r1;r2<=n;r2++)//压缩结束行
        {
            ans=max(ans,Maxsub(r2,n));
        }
    }
    cout<<ans<<endl;
    cin.get(),cin.get();
    return 0;
}
```
