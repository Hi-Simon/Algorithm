#  [POJ-2411-Mondriaan's Dream-状压dp-dfs](https://vjudge.net/problem/POJ-2411)

[TOC]



## Description 

> ```
> Squares and rectangles fascinated the famous Dutch painter Piet Mondriaan. One night, after 
> producing the drawings in his 'toilet series' (where he had to use his toilet paper to draw 
> on, for all of his paper was filled with squares and rectangles), he dreamt of filling a 
> large rectangle with small rectangles of width 2 and height 1 in varying ways. 
> ```
> ![img](https://odzkskevi.qnssl.com/91cd2a849209448a71d50fa577e5d5ed?v=1534772138) 
>
> ```
> Expert as he was in this material, he saw at a glance that he'll need a computer to 
> calculate the number of ways to fill the large rectangle whose dimensions were integer 
> values, as well. Help him, so that his dream won't turn into a nightmare! 
> ```
>
> ![img](https://odzkskevi.qnssl.com/9275391b5c2fd4e3cb556e4ab10140b5?v=1534772138) 

## Input

> ```
> The input contains several test cases. Each test case is made up of two integer numbers: 
> the height h and the width w of the large rectangle. Input is terminated by h=w=0. 
> Otherwise, 1<=h,w<=11. 
> ```

## Output

> ```
> For each test case, output the number of different ways the given rectangle can be filled 
> with small rectangles of size 2 times 1. Assume the given large rectangle is oriented, i.e.
> count symmetrical tilings multiple times. 
> ```

------



## Examples 

> ### Input

> ```
> 1 2
> 1 3
> 1 4
> 2 2
> 2 3
> 2 4
> 2 11
> 4 11
> 0 0
> ```

> ###Output

> ```
> 1
> 0
> 1
> 2
> 3
> 5
> 144
> 51205
> ```

------



## Problem Description

> ```
> 求用2*1或1*2大小的矩阵铺满h*w的矩阵的方案数
> ```

## Solution

> ```
> 状压dp
> w最大只有11，所以可以用11个二进制位表示每个格子的状态。
> 同一行连续的两个1表示1*2的矩阵，当前行当前位为1，前一行相同位为0，表示2*1的矩阵。
> 枚举所有行，再枚举当前行状态，前一行的状态，如果前一行的状态可以转移到当前行，则计数。
> 
> 定义k为二进制的第k位(从左到右)。例：0110101 k==2时的值为0
> 能够转移需满足一下条件：
> 伪代码：
>    如果当前行第k位为1
>       如果前一行的第k位也为1
>           如果k+2>w，或者当前行或前一行的第k+1位为0。则不能够转移;(即两行其中一行不能横着放)
>           否则k+=2;
>       如果前一行的第k位为0 则k++;(能够竖着放)
>    否则当前行第k位为0
>        如果前一行的第k为1，k++;(当前行为0时，前一行一定得为1，00状态未定义，不满足条件)
>        否则 不能够转移;
>   能够转移。
>   
> 这题用dfs其实更好写，而且时间复杂度更低。代码贴在后面，理解上面是一样的。          
> ```

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-23 18:40:35 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-23 22:06:41
 */
#include<iostream>
#include<algorithm>
#include<cstring>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 15
int ans[maxn][1<<11+5];
int dp[maxn][1<<11+5];
int check(int i,int j,int w)//题解伪代码部分。判定是否能够转移。
{
    for(int k=0;k<w;)
    {
        if((j&(1<<k)))
        {
            if((i&(1<<k)))
            {
                if(k==w-1||(j&(1<<(k+1)))==0||(i&(1<<(k+1)))==0) return 0;
                k+=2;
            }
            else k++;
        }
        else
        {
            if((i&(1<<k))==0) return 0;
            k++;
        }
    }
    return 1;
}
int is(int i,int w)//初始化第一行的状态
{
    for(int k=0;k<w;)
    {
        if((i&(1<<k)))//如果不能横着放则不满足转移条件
        {
            if(k==w-1) return 0;
            if((i&(1<<(k+1)))) k+=2;
            else return 0;
        }
        else k++;
    }
    return 1;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int h,w;
    memset(ans,-1,sizeof(ans));
    while(cin>>h>>w&&h&&w)
    {
        if(h<w) swap(h,w);
        if(ans[h][w]!=-1) {cout<<ans[h][w]<<endl; continue;}//用一个数组记忆化
        if((h&1)&&(w&1)) {cout<<0<<endl; continue;}//如果行和列都是奇数，则不能铺满
        memset(dp,0,sizeof(dp));
        for(int i=0;i<(1<<w);i++) 
        {
            if(is(i,w)) dp[1][i]=1; 
        }
        for(int r=2;r<=h;r++)//枚举行
        {
            for(int i=0;i<(1<<w);i++)//前一行的状态
            {
                for(int j=0;j<(1<<w);j++)//当前行的状态
                {
                    if(check(i,j,w)) dp[r][j]+=dp[r-1][i];
                }
            }
        }
        cout<<(ans[h][w]=dp[h][(1<<w)-1])<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-23 20:30:51 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-23 22:07:56
 */
#include<iostream>
#include<algorithm>
#include<cstring>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 15
int ans[maxn][maxn];
int dp[maxn][1<<11+5];
int h, w;
void dfs(int r,int c,int now,int pre)
{
    if(c>=w) {dp[r][now]+=dp[r-1][pre]; return ;}
    if(c+1<=w)//竖着放
    {
        dfs(r,c+1,(now<<1)|1,pre<<1);
        dfs(r,c+1,(now<<1),(pre<<1)|1);
    }
    if(c+2<=w) dfs(r,c+2,(now<<2)|3,(pre<<2)|3);//横着放
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    memset(ans,-1,sizeof(ans));
    while(cin>>h>>w&&h&&w)
    {
        memset(dp,0,sizeof(dp));
        if(h<w) swap(h,w);
        if((h&1)&&(w&1)) {cout<<0<<endl; continue;}
        if(ans[h][w]!=-1) {cout<<ans[h][w]<<endl; continue;}//记忆化
        dp[0][(1<<w)-1]=1;//初始化
        for(int i=1;i<=h;i++) dfs(i,0,0,0);//枚举所有行
        cout<<(ans[h][w]=dp[h][(1<<w)-1])<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```

