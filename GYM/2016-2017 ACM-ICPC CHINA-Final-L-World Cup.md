#  [2016-2017 ACM-ICPC CHINA-Final-L-World Cup ](http://codeforces.com/gym/101194/problem/L)

[TOC]



## ``Description ``

> Here is World Cup again, the top 32 teams come together to ght for the World Champion.
>
> The teams are assigned into 8 groups, with 4 teams in each group. Every two teams in the same
> group will play a game (so there are totally 6 games in each group), and the winner of this game
> gets 3 points, loser gets 0 point. If it is a tie game, both teams get 1 point.
>
> After all games nished, we get the scoreboard, but we forget the result of each game, can you
> help us to gure the result of each game? We only care about the win/lose/tie result of each
> game, but we don't care the goals in each game.

## `Input`

> The input starts with one line containing exactly one integer T, which is the number of test cases.
>
> Each test case contains four space-separated integers A, B, C, D, in a line, which indicate the
> points each team gets after all 6 games.

## `Output`

> For each test case, output one line containing Case #x: y, where x is the test case number
> (starting from 1) and y is "Yes" if you can point out the result of each game, or "No" if there are
> multiple game results satisfy the scoreboard, or \Wrong Scoreboard" if there is no game result
> matches the scoreboard.

## `Examples` 

> ### `Input`

> ```
> 3
> 9 6 3 0
> 6 6 6 0
> 10 6 3 0
> ```

> ###Ouput

> ```
> Case #1: Yes
> Case #2: No
> Case #3: Wrong Scoreboard
> ```

## `Problem Description`

> 有四支队伍参加比赛，两两对抗，总共要打6场比赛，给你四支队伍的最终得分，让你判断能否产生此种分数。如果能，是否唯一。
>
> 赢一场，得3分，平一场，双方各得一分，败一场，得0分。

## `Solution`

> 先预处理：暴力枚举4支队伍输赢的所有情况，算出得分，并计数。
>
> 然后根据给的四支队伍得分情况判断。

## `Code`

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-13 19:22:24 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-13 20:16:23
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 15
int vis[maxn][maxn][maxn][maxn];
int init()
{
    int op1[]={3,1,0};//主方 赢，平，败
    int op2[]={0,1,3};//副方 败，平，赢
    for(int k1=0;k1<3;k1++) for(int k2=0;k2<3;k2++) for(int k3=0;k3<3;k3++)//六只队伍胜负平情况分别枚举
    for(int k4=0;k4<3;k4++) for(int k5=0;k5<3;k5++) for(int k6=0;k6<3;k6++)
    {   //每支队伍都要打三场，所以总分数就是三场的赢，败，平情况的枚举相加
        int a=op1[k1]+op1[k2]+op1[k3];
        int b=op2[k1]+op1[k4]+op1[k5];//第二支队伍第一场已经和第一支队伍对抗过。
        int c=op2[k2]+op2[k4]+op1[k6];//第三支队伍的前两场已经分别和第一、第二支队伍对抗过。
        int d=op2[k3]+op2[k5]+op2[k6];//同理
        vis[a][b][c][d]++;//将得分计数，可通过数目判读是否出现，且唯一
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    init();
    int t;
    cin>>t;
    for(int ca=1;ca<=t;ca++)
    {
        cout<<"Case #"<<ca<<": ";
        int a,b,c,d;
        cin>>a>>b>>c>>d;
        if(vis[a][b][c][d]>1) //如果次数大于1，代表不唯一
        {
            cout<<"No"<<endl;
        }
        else if(vis[a][b][c][d]==1)//只出现一次，唯一
        {
            cout<<"Yes"<<endl;
        }
        else cout<<"Wrong Scoreboard"<<endl;//未出现过，错误的分数
    }
    cin.get(),cin.get();
    return 0;
}
```

