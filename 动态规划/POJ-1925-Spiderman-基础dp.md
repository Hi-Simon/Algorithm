#  [POJ-1925-Spiderman-基础dp](https://vjudge.net/problem/POJ-1925)





[TOC]



## Description 

> ```
> Dr. Octopus kidnapped Spiderman's girlfriend M.J. and kept her in the West Tower. Now the 
> hero, Spiderman, has to reach the tower as soon as he can to rescue her, using his own 
> weapon, the web.
> 
> From Spiderman's apartment, where he starts, to the tower there is a straight road. 
> Alongside of the road stand many tall buildings, which are definitely taller or equal to 
> his apartment. Spiderman can shoot his web to the top of any building between the tower and 
> himself (including the tower), and then swing to the other side of the building. At the 
> moment he finishes the swing, he can shoot his web to another building and make another 
> swing until he gets to the west tower. Figure-1 shows how Spiderman gets to the tower from 
> the top of his apartment – he swings from A to B, from B to C, and from C to the tower. All
> the buildings (including the tower) are treated as straight lines, and during his swings he
> can't hit the ground, which means the length of the web is shorter or equal to the height 
> of the building. Notice that during Spiderman's swings, he can never go backwards. 
> ```
> ![img](https://odzkskevi.qnssl.com/f7201b241da5cc7b2ccc00ecc0623582?v=1534717053) 
>
> ```
> You may assume that each swing takes a unit of time. As in Figure-1, Spiderman used 3 
> swings to reach the tower, and you can easily find out that there is no better way. 
> ```

## Input

> ```
> The first line of the input contains the number of test cases K (1 <= K <= 20). Each case
> starts with a line containing a single integer N (2 <= N <= 5000), the number of buildings
> (including the apartment and the tower). N lines follow and each line contains two integers
> Xi, Yi, (0 <= Xi, Yi <= 1000000) the position and height of the building. The first
> building is always the apartment and the last one is always the tower. The input is sorted
> by Xi value in ascending order and no two buildings have the same X value. 
> ```

## Output

> ```
> For each test case, output one line containing the minimum number of swings (if it's 
> possible to reach the tower) or -1 if Spiderman can't reach the tower.
> ```

------



## Examples 

> ### Input

> ```
> 2
> 6
> 0 3
> 3 5
> 4 3
> 5 5
> 7 4
> 10 4
> 3
> 0 3
> 3 4
> 10 4
> ```

> ###Output

> ```
> 3
> -1
> ```

------



## Problem Description

> ```
> 在一维坐标系内，给你n个建筑物的横坐标x，和高度h（保证所有建筑物的高度不小于第一座）。
> 蜘蛛侠能够在建筑物之间游荡。求从第一座建筑物开始到最后一座的最小游荡次数。如果不能到达则输出-1.
> 只能以建筑物最高点为圆心进行游荡。
> ```

## Solution

> ```
> 基础dp
> 概况：枚举每一座建筑i，再枚举j(j<i),j能通过i所能到达的最远距离是否大于终点的横坐标，如果不大于，
> 则将其次数加1.
> 设dp[i]为到达第i个建筑物的最小游荡次数为dp[i].
> 设第i座建筑物的坐标为(x,y).
> 则p应满足以下条件：
> ```
> $$
> 第i座建筑物最高点到(p,h)的距离不能大于自己的高度，否则就落地了。\\
> (x-p)^2+(y-h)^2\leq y^2\\
> (x-p)^2\leq y^2-(y-h)^2\\
> x-p\leq \sqrt{y^2-(y-h)^2}\\
> p\geq x- \sqrt{y^2-(y-h)^2}\\
> 其中p为能够到达第i座建筑物的最远点的横坐标。\\
> h为第一座建筑物的高度。
> 有对称性可知，每次游荡的起始高度一定为h。
> $$
>
> ```
> 枚举p到x的所有建筑物的横坐标j，如果x-j+x大于最后一座建筑物的横坐标，则能够到达终点。更新答案
> ans=min(ans,dp[j]+1);
> 否则 dp[x-j+x]=min(dp[x-j+x],dp[j]+1).(x-j+x几何意义即为j关于x的对称点的横坐标)
> ```
>

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-21 20:31:54 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-21 21:11:10
 */
#include<iostream>
#include<vector>
#include<algorithm>
#include<cstring>
#include<cmath>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 1000005
vector<pair<int,int> >a;
int dp[maxn];
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin>>t;
    while(t--)
    {
        a.clear();
        memset(dp,INF,sizeof(dp));
        int n;
        cin>>n;
        int x,y;
        cin>>x>>y;//第一座建筑物的坐标
        a.push_back(make_pair(0,0));
        for(int i=1;i<n;i++)
        {
            int u,v;
            cin>>u>>v;
            a.push_back(make_pair(u,v));
        }
        dp[0]=0;
        int ans=INF;
        for(int i=1;i<n;i++)
        {
            int tx=a[i].first,ty=a[i].second;
            for(int j=max(x,tx-(int)sqrt(1.*ty*ty-1.*(ty-y)*(ty-y)));j<tx;j++)//枚举所有能到达第i做建筑物的横坐标
            {
                if(dp[j]==INF) continue;
                int tmp=tx-j+tx;//j关于tx的对称点的横坐标
                if(tmp>=a[n-1].first)//判断能否直接到达终点
                {
                    ans=min(ans,dp[j]+1);
                }
                else dp[tmp]=min(dp[tmp],dp[j]+1);
            }
        }
        if(ans==INF)
        cout<<-1<<endl;
        else
        cout<<ans<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
