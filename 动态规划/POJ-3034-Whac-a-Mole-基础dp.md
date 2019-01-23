#  [POJ-3034-Whac-a-Mole-基础dp](https://vjudge.net/problem/POJ-3034)

[TOC]



## Description 

> ```
> While visiting a traveling fun fair you suddenly have an urge to break the high score in
> the Whac-a-Mole game. The goal of the Whac-a-Mole game is to… well… whack moles. With a 
> hammer. To make the job easier you have first consulted the fortune teller and now you know 
> the exact appearance patterns of the moles.
> The moles appear out of holes occupying the n2 integer points (x, y) satisfying 0 ≤ x, y < 
> n in a two-dimensional coordinate system. At each time step, some moles will appear and 
> then disappear again before the next time step. After the moles appear but before they 
> disappear, you are able to move your hammer in a straight line to any position (x2, y2)
> that is at distance at most d from your current position (x1, y1). For simplicity, we 
> assume that you can only move your hammer to a point having integer coordinates. A mole is 
> whacked if the center of the hole it appears out of is located on the line between (x1, y1)
> and (x2, y2) (including the two endpoints). Every mole whacked earns you a point. When the 
> game starts, before the first time step, you are able to place your hammer anywhere you see
> fit.
> ```

## Input

> ```
>     The input consists of several test cases. Each test case starts with a line containing
> three integers n, d and m, where n and d are as described above, and m is the total number 
> of moles that will appear (1 ≤ n ≤ 20, 1 ≤ d ≤ 5, and 1 ≤ m ≤ 1000). Then follow m lines, 
> each containing three integers x, y and t giving the position and time of the appearance of
> a mole (0 ≤ x, y < n and 1 ≤ t ≤ 10). No two moles will appear at the same place at the 
> same time.
> 
>     The input is ended with a test case where n = d = m = 0. This case should not be 
> processed.
> 
> ```

## Output

> ```
> For each test case output a single line containing a single integer, the maximum possible 
> score achievable.
> ```

------



## Examples 

> ### Input

> ```
> 4 2 6
> 0 0 1
> 3 1 3
> 0 1 2
> 0 2 2
> 1 0 2
> 2 0 2
> 5 4 3
> 0 0 1
> 1 2 1
> 2 4 1
> 0 0 0
> ```

> ###Output

> ```
> 4
> 2
> ```

------



## Problem Description

> ```
> 打地鼠。
> 给你n*n矩阵，给你地鼠出现的时间和坐标(保证同一时间，同一坐标只出现一个地鼠),求最大得分。
> 1秒内锤子只能选择一个方向沿直线移动，且最大移动距离不超过d，锤子可以移动到任意位置，即可以移动到矩阵之外。
> 一次得分即为1秒内锤子所经过路径地鼠出现的个数。
> ```

## Solution

> ```
> 基础dp
> 概况：枚举每个时间，在每个时间内枚举每个起点，在每个起点再枚举所能到达的满足条件的终点，求出在这个时间，这个起点，这个终点的得分。在同一个时间，同一个起点，不同终点的得分可以通过递推得到：当前终点的得分=前一个位置的得分+终点所具有的分数。特判起点和终点在同一位置的得分。
> 
> 1、锤子可以移动到任意位置，所以在保存地鼠出现坐标的时候，横坐标和纵坐标都要增加max(d)=5的偏移量。
> 由（1）得：枚举起点的时候，横坐标范围为[-5,n+5],纵坐标范围为[-5,n+5],增加偏移量以后都为[0,n+10];
> 
> 2、终点与起点的距离不能超过d，即以起点为圆心，半径为d的圆内所有的点。
> 在枚举终点的时候，可以通过矢量的形式简化，将每个方向分解为x轴方向和y轴方向的偏移量。
> 
> 结束。具体看code注释。
> ```

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-23 10:15:39 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-23 13:19:25
 */
#include <iostream>
#include <algorithm>
#include <cmath>
#include <cstring>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 35
int n, d, m;
int a[maxn][maxn][maxn];//时间t，坐标(x,y)
int dir[4][2] = {1, 1, 1, -1, -1, 1, -1, -1};//上下左右四个方向
int dp[maxn][maxn][maxn];//在前t个时间内，以(x,y)为起点所能获得的最大得分
inline int gcd(int a, int b)
{
    return b == 0 ? a : gcd(b, a % b);
}
inline int dis(int x1, int y1, int x2, int y2)//两点之间距离
{
    return (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2);
}
int solve(int t, int x, int y)//枚举以(x,y)为起点的，满足条件的所有终点
{
    int dt[maxn][maxn]={0};//以(tx,ty)为终点的得分
    for (int i = 0; i <= d; i++)//横坐标的移动距离
    {
        for (int j = 0; j <= d; j++)//纵坐标的移动距离
        {
            if (!i && !j)//终点坐标就是自己
            {
                dp[t][x][y] += a[t][x][y];
                dt[x][y] = dp[t][x][y];
                dp[t + 1][x][y] = max(dp[t + 1][x][y], dt[x][y]);
                continue;
            }
            if (dis(x, y, x + i, y + j) > d * d) continue;//保证移动距离小于等于d
            for (int k = 0; k < 4; k++)//枚举四个方向
            {
                int tx = x + i * dir[k][0];//终点横坐标
                int ty = y + j * dir[k][1];//终点纵坐标
                if (tx < 1 || tx > n || ty < 1 || ty > n) continue;
                int g = gcd( abs(x - tx), abs(y - ty)); 
                dt[tx][ty] = dt[tx + (x - tx) / g][ty + (y - ty) / g] + a[t][tx][ty];//前一个位置的得分加上终点的分数
                dp[t + 1][tx][ty] = max(dp[t + 1][tx][ty], dt[tx][ty]);
            }
        }
    }
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    while (cin >> n >> d >> m && n && d && m)
    {
        memset(dp, 0, sizeof(dp));
        memset(a,0,sizeof(a));
        int Maxt = 0;
        n += 10;
        for (int i = 1; i <= m; i++)
        {
            int x, y, t;
            cin >> x >> y >> t;
            a[t][x + 5][y + 5] = 1;//锤子可以移动到任意位置，但最大移动距离不超过5,保证负方向也能移动
            Maxt = max(Maxt, t);//最后一个出现的时间
        }
        for (int t = 1; t <= Maxt; t++)//枚举每个时间，每个位置为起始点
        {
            for (int i = 0; i < n; i++)
            {
                for (int j = 0; j < n; j++)
                {
                    solve(t, i, j);//函数内枚举终点
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                ans = max(ans, dp[Maxt + 1][i][j]);//求得分最大值
            }
        }
        cout << ans << endl;
    }
    cin.get(), cin.get();
    return 0;
}
```
