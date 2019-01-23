#  [Most Powerful-ZOJ-3471](https://vjudge.net/problem/17143/origin)动态规划-状压dp

[TOC]



## ``Description ``

>  Recently, researchers on Mars have discovered N powerful atoms. All  of them are different. These atoms have some properties. When two of  these atoms collide, one of them disappears and a lot of power is  produced. Researchers know the way every two atoms perform when collided  and the power every two atoms can produce. 
>
>  You are to write a program to make it most powerful, which  means that the sum of power produced during all the collides is maximal.  

## `Input`

>  There are multiple cases. The first line of each case has an integer N (2 <= N <= 10), which means there are N atoms: A1, A2, ... , AN. Then N lines follow. There are N integers in each line. The j-th integer on the i-th line is the power produced when Ai and Aj collide with Aj gone. All integers are positive and not larger than 10000. 
>
>  The last case is followed by a 0 in one line. 
>
>  There will be no more than 500 cases including no more than 50 large cases that N is 10. 

## `Output`

> Output the maximal power these N atoms can produce in a line for each case.  

## `Examples` 

> ### `Input`

> ```
> 2
> 0 4
> 1 0
> 3
> 0 20 1
> 12 0 1
> 1 10 0
> 0
> ```

> ###Ouput

> ```
> 4
> 22
> ```

## `Problem description`

> 有n种气体，每种气体之间能发生碰撞，并产生能量，i与j碰撞，j消失，j与i碰撞，i消失，并且两种碰撞产生的能量不同，将各种碰撞组合以矩阵的方式给你，i行j列的数表示i与j碰撞，j消失所产生的能量。
>
> 问，它们如何碰撞才能使所有产生的能量总和最大。

## `Solution`

> 状压dp
>
> 
> $$
> 定义dp[i]表示在第i个状态所能获得的最大能量。
> $$
> 状态 i 用2进制表示，比如 n 等于 4 时，1100 （从右往左）表示第2种和第3种气体消失，剩余第0种和第1种气体。
>
> 那么下一次碰撞有两种选择，即用第1种气体碰撞第0种，或者用第0种碰撞第1种，状态转移方程分别为：
> $$
> dp[(1101)]=dp[13]=max(dp[13],dp[(1100)]+a[1][0]);
> $$
>
> $$
> dp[(1110)]=dp[14]=max(dp[14],dp[(1100)]+a[0][1]);
> $$
>
> $$
> a[i][j]表示用第i种气体碰撞第j种气体所产生的能量
> $$
>
> 所以我们可以知道，状态转移就是枚举所有可能的下一次碰撞，根据状态的变化关系由由上一个状态求得当前状态的最优值。
>
> 总的状态转移方程：
> $$
> dp[i|(1<<j)]=max(dp[i|(1<<j)],dp[i]+a[k][j])，i为上一次的状态，j，k为找到的两种未消失的气体
> $$
> 

## `Code`

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-14 09:32:57 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-14 11:09:24
 */
#include <bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 15
int a[maxn][maxn];
int dp[1 << 11]; //状态为i时所获得的最大能量
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    while (cin >> n && n)
    {
        memset(dp, 0, sizeof(dp));
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                cin >> a[i][j]; //i碰撞j，j消失所获得的能量
            }
        }
        int ans = 0;
        for (int i = 0; i < (1 << n); i++)
        {
            for (int j = 0; j < n; j++) //枚举每一个位置的气体,找到一个未消失的其他
            {
                if ((i & (1 << j)) != 0)
                    continue;               //如果当前比特位的值为1，代表此气体已消失，跳过此状态
                for (int k = 0; k < n; k++) //再找一个还未消失的气体
                {
                    if ((i & (1 << k)) == 0 && j != k) //第一个判断，同j，还要保证两个气体不是同一个
                        dp[i | (1 << j)] = max(dp[i | (1 << j)], dp[i] + a[k][j]);
                    ans = max(ans, dp[i | (1 << j)]); //所有状态里找最大的
                }
            }
        }
        cout << ans << endl;
    }
    cin.get(), cin.get();
    return 0;
}
```

