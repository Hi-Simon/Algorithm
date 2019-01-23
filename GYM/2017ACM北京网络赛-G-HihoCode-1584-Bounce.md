#  [2017ACM北京网络赛-G-HihoCode-1584-Bounce](https://vjudge.net/problem/HihoCoder-1584)

[TOC]



## Description 

> ```
> For Argo, it is very interesting watching a circle bouncing in a rectangle.
> 
> As shown in the figure below, the rectangle is divided into N×M grids, and the 
> circle fits exactly one grid.
> 
> The bouncing rule is simple:
> 
> 1. The circle always starts from the left upper corner and moves towards lower
> right.
> 
> 2. If the circle touches any edge of the rectangle, it will bounce.
> 
> 3. If the circle reaches any corner of the rectangle after starting, it will stop
> there.
> ```
>
> ![img](https://odzkskevi.qnssl.com/3fb59b2517881e23f1d0645abee4dd98?v=1534648011)
>
> ```
> Argo wants to know how many grids the circle will go through only once until it 
> first reaches another corner. Can you help him?
> ```

## Input

> ```
> The input consists of multiple test cases. (Up to 105)
> 
> For each test case:
> 
> One line contains two integers N and M, indicating the number of rows and columns 
> of the rectangle. (2 ≤ N, M ≤ 10^9)
> 
> ```

## Output

> ```
> For each test case, output one line containing one integer, the number of grids 
> that the circle will go through exactly once until it stops (the starting grid and
> the ending grid also count).
> ```

------



## Examples 

> ### Input

> ```
> 2 2
> 2 3
> 3 4
> 3 5
> 4 5
> 4 6
> 4 7
> 5 6
> 5 7
> 9 15
> ```

> ###Output

> ```
> 2
> 3
> 5
> 5
> 7
> 8
> 7
> 9
> 11
> 39
> ```

------



## Problem Description

> ```
> N*M的矩阵格子，一个小球从左上角开始，向右下角45°方向发射，如果遇到边，则反弹，直到遇到角。
> 问整个过程中有多少个格子只经过1次。
> ```

## Solution

> ```
> GCD
> 可以这么去思考：
> 遇到边以后的会反弹，但这个过程是具有对称性质的，不局限于n*m的矩阵，先将这个矩阵横向拉伸，每次反弹画
> 出它的对称路线。然后再将这个矩阵纵向拉伸，再将每次反弹画出它关于行的对称路线，你会发现其实就是求一
> 个正方形的边长。而这个正方形的边长就等于用长和宽分别为(n-1,m-1)长方形所能拼出的边长最小的正方形的边长，再加1.
> 求出来的是经过的所有格子的个数。还要去掉经过多次的。
> emmm..还没想通，有人知道请告知我一下...不胜感激。
> ```

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-24 16:29:45 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-24 20:11:56
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m;
    while(cin>>n>>m)
    {
        int ans1=(n-1)*(m-1)/__gcd(n-1,m-1)+1;//经过的格子的个数
        int ans2=((n-1)/__gcd(n-1,m-1)-1)*((m-1)/__gcd(n-1,m-1)-1);//重复经过的格子的个数
        cout<<ans1-ans2<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
