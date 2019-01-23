#  [2017ACM北京网络赛-HihoCode-1580-C-Matrix-dp](https://vjudge.net/contest/249612#problem/C)

[TOC]



## Description 

> ```
> Once upon a time, there was a little dog YK. One day, he went to an antique shop 
> and was impressed by a beautiful picture. YK loved it very much.
> 
> However, YK did not have money to buy it. He begged the shopkeeper whether he could 
> have it without spending money.
> 
> Fortunately, the shopkeeper enjoyed puzzle game. So he drew a n × m matrix on the
> paper with integer value ai,j in each cell. He wanted to find 4 numbers x, y, x2,
> and y2(x ≤ x2, y ≤ y2), so that the sum of values in the sub-matrix from (x, y) to 
> (x2, y2) would be the largest.
> 
> To make it more interesting, the shopkeeper ordered YK to change exactly one cell's 
> value into P, then to solve the puzzle game. (That means, YK must change one cell's
> value into P.)
> 
> If YK could come up with the correct answer, the shopkeeper would give the picture
> to YK as a prize.
> 
>     YK needed your help to find the maximum sum among all possible choices.
> 
> ```

## Input

> ```
>     There are multiple test cases.
> 
>     The first line of each case contains three integers n, m and P. (1 ≤ n, m ≤ 300, -1000 ≤ P ≤ 1000).
> 
>     Then next n lines, each line contains m integers, which means ai,j (-1000 ≤ ai,j ≤ 1000).
> 
> ```

## Output

> ```
> For each test, you should output the maximum sum.
> ```

------



## Examples 

> ### Input

> ```
> 3 3 4
> -100 4 4
> 4 -10 4
> 4 4 4
> 3 3 -1
> -2 -2 -2
> -2 -2 -2
> -2 -2 -2
> ```

> ###Output

> ```
> 24
> -1
> ```

------



## Problem Description

> ```
> 最大子矩阵和变形。
> 求最大子矩阵和的基础上，可以修改任意一个位置，将其值改为p(必须修改一次)
> ```

## Solution

> ```
> 类似求最大子矩阵和。dp。
> 定义dp数组，dp[i][0]表示前i列，不修改任何点的最大值。dp[i][1]表示前i列，修改其中一个点的最大值.
> mval[k],保存[i,j]行，第k列的最小值
> 则可写出状态转移方程
> dp[i][0]=max(dp[i-1][0],(int)0)+sum[i];//基础的最大子矩阵和方程
> dp[i][1]=max(max(dp[i-1][1]+sum[i],(int)0+sum[i]-mval[i]+p),dp[i][0]-mval[i]+p);
> 第二个方程解释一下：
> 当前状态可以从三个状态转移过来。
> dp[i-1][1]+sum[i],就是如果已经有一个点修改过并且大于0，后面就直接当求最大子矩阵和一样求就好了。
> 0+sum[i]-mval[i]+p,如果前面已经修改过，但是和小于0，则需重新修改并求和。
> dp[i][0]-mval[i]+p,如果前面没有修改，则修改后求和
> 需要特判如果最大子矩阵就是矩阵本身的情况。具体看Code。
> 不知道怎么求最大子矩阵和的建议先去学一下。下面贴个自己的博客链接。
> ```
>
> [最大子矩阵和](https://blog.csdn.net/Kente_K/article/details/82025838)

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-25 09:14:15 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-25 18:50:15
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 1005
int dp[maxn][2];//dp[i][0]表示前i列，不修改任何点的最大值。dp[i][1]表示前i列，修改其中一个点的最大值
int a[maxn][maxn];
int mval[maxn];//保存[i,j]行，第k列的最小值
int sum[maxn];//保存[i,j]行,第k列的和
int solve(int r1,int r,int c,int p,int n)
{
    for(int i=1;i<=c;i++)
    {
        sum[i]+=a[r][i];
        mval[i]=min(mval[i],a[r][i]);
    }
    memset(dp,-INF,sizeof(dp));//必须初始化为无穷小---
    int ans=-INF,flag=1;//flag用于标记最大值是否是整个矩阵,flag=1则是，否则不是
    for(int i=1;i<=c;i++)
    {
        if(dp[i-1][0]<0) flag=i;
        dp[i][0]=max(dp[i-1][0],(int)0)+sum[i];
        dp[i][1]=max(max(dp[i-1][1]+sum[i],(int)0+sum[i]-mval[i]+p),dp[i][0]-mval[i]+p);//如果前一个值是修改过的值，直接加上当前列,否则需要修改一个值
        if(r1==1&&r==n&&i==c&&flag==1) ans=max(ans,dp[i][1]);//如果是整个矩阵的和，必须要修改一个
        else ans=max(ans,max(dp[i][0],dp[i][1]));//如果子矩阵，最大子矩阵内可以不修改
    }
    return ans;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m,p;
    while(cin>>n>>m>>p)
    {
        for(int i=1;i<=n;i++)
            for(int j=1;j<=m;j++)
                cin>>a[i][j];
        int ans=-INF;
        for(int r1=1;r1<=n;r1++)
        {
            memset(mval,INF,sizeof(mval));
            memset(sum,0,sizeof(sum));
            for(int r2=r1;r2<=n;r2++)
            {
                ans=max(ans,solve(r1,r2,m,p,n));
            }
        }
        cout<<ans<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
