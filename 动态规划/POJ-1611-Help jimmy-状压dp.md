#  [POJ-1611-Help Jimmy](https://vjudge.net/problem/23662/origin)

[TOC]



## ``Description ``

> "Help Jimmy" 是在下图所示的场景上完成的游戏。  
>
> ![img](https://odzkskevi.qnssl.com/0b9f30603f1b35df0918f9dc606d9d8b?v=1533955220) 
>
> 场景中包括多个长度和高度各不相同的平台。地面是最低的平台，高度为零，长度无限。      Jimmy老鼠在时刻0从高于所有平台的某处开始下落，它的下落速度始终为1米/秒。当Jimmy落到某个平台上时，游戏者选择让它向左还是向右跑，它跑动的速度也是1米/秒。当Jimmy跑到平台的边缘时，开始继续下落。Jimmy每次下落的高度不能超过MAX米，不然就会摔死，游戏也会结束。     设计一个程序，计算Jimmy到底地面时可能的最早时间。 

## `Input`

> 第一行是测试数据的组数t（0 <= t <=  20）。每组测试数据的第一行是四个整数N，X，Y，MAX，用空格分隔。N是平台的数目（不包括地面），X和Y是Jimmy开始下落的位置的横竖坐标，MAX是一次下落的最大高度。接下来的N行每行描述一个平台，包括三个整数，X1[i]，X2[i]和H[i]。H[i]表示平台的高度，X1[i]和X2[i]表示平台左右端点的横坐标。1 <= N <= 1000，-20000 <= X, X1[i], X2[i] <= 20000，0 < H[i] < Y <= 20000（i = 1..N）。所有坐标的单位都是米。      Jimmy的大小和平台的厚度均忽略不计。如果Jimmy恰好落在某个平台的边缘，被视为落在平台上。所有的平台均不重叠或相连。测试数据保证问题一定有解。  

## `Output`

> 对输入的每组测试数据，输出一个整数，Jimmy到底地面时可能的最早时间。 

## `Examples` 

> ### `Input`

> ```
> 1
> 3 8 17 20
> 0 10 8
> 0 10 13
> 4 14 3
> ```

> ###Ouput

> ```
> 23
> ```

> ###Input

> ```
> 
> ```

> ###Output

> ```
> 
> ```

## `题意`

> 中文，就不解释了。

## `题解`

> $$
> dp[i][0]为在第i个高度从左边下去到地面所花费的最短时间
> $$
>
> $$
> dp[i][1]为在第i个高度从右边下去到地面所花费的最短时间
> $$
>
> 在每个高度我们都只有有两个选择，向左走或向右走。所以我们当前的状态也只能由前面两个状态转移过来。
>
> 在每个高度，我们的**决策和状态都是一样的**即：如果当前选择向左走，那么最优值为 **min(前面每个状态花费的时间+相应高度差+相应水平差)**  也就等于 **min(前面每个状态花费的时间+相应水平差)+相应高度差**
> $$
> dp[i][0] = min(dp[i][0], min(dp[k][0] + a[i].x - a[k].x, dp[k][1] + a[k].y - a[i].x) + a[i].h - a[k].h)
> $$
> 如果选择向右走，类似
> $$
> dp[i][1] = min(dp[i][1], min(dp[k][0] + a[i].y - a[k].x, dp[k][1] + a[k].y - a[i].y) + a[i].h - a[k].h)
> $$
> 其中
> $$
> 0<=k<i，a[i].x为当前板子的左端位置，a[i].y为右端位置，a[i].h为高度
> $$
> 在用状态转移方程的时候还要判断从当前层的左端（右端）下去能否到达第k层。
>
> 所以要加两个判断条件，具体看代码里的注释。
>
> 还要特别判断是否能直接到达地面。

## `代码`

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-12 18:27:40 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-12 19:18:14
 */
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 20005
struct node
{
    int x, y, h;
} a[maxn];
bool cmp(node a, node b)
{
    if (a.h < b.h)
        return 1;
    return 0;
}
int dp[maxn][2]; //第i个高度向左或向右走所花费的最小时间
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin >> t;
    while (t--)
    {
        int n, x, y, max;
        cin >> n >> x >> y >> max;
        for (int i = 1; i <= n; i++)
        {
            cin >> a[i].x >> a[i].y >> a[i].h;
        }
        sort(a + 1, a + n + 1, cmp);
        a[n + 1].x = x, a[n + 1].y = x, a[n + 1].h = y; //将起始点放入
        memset(dp, INF, sizeof(dp));
        dp[1][0] = dp[1][1] = a[1].h; //初始化第一层
        bool flag0 = 0, flag1 = 0;
        for (int i = 2; i <= n + 1; i++)
        {
            flag0 = 0, flag1 = 0;            //俩个flag分别表示左 右两边是否已经遇到过板子
            for (int k = i - 1; k >= 0; k--) //k==0表示地面的状态
            {
                if (a[i].h - a[k].h <= max && k) //高度差满足条件
                {
                    if (a[i].y >= a[k].x && a[k].y >= a[i].y && !flag1)//保证第k层的板子在我要降落的范围内
                    {
                        flag1 = 1;//从当前层向右走的最小时间= 当前层到第k层的高度差+当前层右边与第k层左边或右边的水平距离差最小的那个
                        dp[i][1] = min(dp[i][1], min(dp[k][0] + a[i].y - a[k].x, dp[k][1] + a[k].y - a[i].y) + a[i].h - a[k].h);
                    }
                    if (a[i].x >= a[k].x && a[k].y >= a[i].x && !flag0)//保证第k层的板子在我要降落的范围内
                    {
                        flag0 = 1;//同上
                        dp[i][0] = min(dp[i][0], min(dp[k][0] + a[i].x - a[k].x, dp[k][1] + a[k].y - a[i].x) + a[i].h - a[k].h);
                    }
                }
                else if (k == 0 && a[i].h <= max)//直接跳到地面花费的时间，用flag来保证能直接跳到地面
                {
                    if (!flag0)
                        dp[i][0] = min(dp[i][0], a[i].h);
                    if (!flag1)
                        dp[i][1] = min(dp[i][1], a[i].h);
                }
            }
        }
        cout << dp[n + 1][0] << endl;
    }
    cin.get(), cin.get();
    return 0;
}
```

