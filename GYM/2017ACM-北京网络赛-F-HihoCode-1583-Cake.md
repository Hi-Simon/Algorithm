#  [2017ACM-北京网络赛-F-HihoCode-1583-Cake](https://vjudge.net/problem/HihoCoder-1583)

[TOC]



## Description 

> ```
>     To celebrate that WF-2018 will be held in PKU, Alice, Bob, and Cate are asked 
> to make
> 
>     N cakes.
> 
>     Every cake i needs to go through 3 steps in restrict order:
> 
>     1. Alice mixes flour and water for ai minutes;
> 
>     2. Bob carefully bakes it for bi minutes;
> 
>     3. Cate makes cream decoration for ci minutes.
> 
>     Since Cate wants to have different kinds of cakes, the third step of any cake i
> is always not less time-consuming than the second step of any cake j. Also, it is
> reasonable that once anyone starts to process a cake, the procedure cannot be
> stopped then be resumed.
> 
>     To have these cakes done as soon as possible, they need your help.
> 
> ```

## Input

> ```
>     There are several cases (less than 15 cases).
> 
>     The first line of every case contains an integer N (1 ≤ N ≤ 105)—the number of
> cakes to prepare.
> 
>     After that, N lines follow, each of them contains three integers ai, bi and ci
> (1 ≤ i ≤ N; 0 <ai, bi, ci < 106)—time that needs to be spent on the three steps of
> cake i respectively.
> 
>     It is guaranteed that for any i and any j, bi is no greater than cj.
> 
>     The input ends with N = 0.
> 
> ```

## Output

> ```
> For every case, print in a single line the least possible time to make all cakes.
> ```

------



## Examples 

> ### Input

> ```
> 3
> 5 3 4
> 3 2 9
> 3 4 8
> 0
> ```

> ###Output

> ```
> 26
> ```

------



## Problem Description

> ```
> 做蛋糕有三个工序，分别三个人负责，而且必须严格按照步骤来。并且一旦开始做一块蛋糕，这个过程不能停。
> 否则就得重做。
> 总共要做n个蛋糕，做第i个蛋糕三个步骤的时间分别为ai，bi，ci.(保证max(bi)<=min(ci) ).
> 
> ```

## Solution

> ```
> 题解很简单，但是很难想…
> ```
>
> $$
> ans=max(\sum_i^na_i+min_{1\le i \le n}(b_i+c_i),\sum_i^nc_i+min_{1\le i \le n}(a_i+b_i))
> $$
>
> ```
> 大致意思就是，所有a的和为suma，所有b的和为sumb，两者最大的一个加上对应剩余两个工序相加的最小值。
> ```
>
>

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-24 14:46:18 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-24 19:15:35
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int a[maxn];
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    while(cin>>n&&n)
    { 
        int Minxy=INF,Minyz=INF,ans1=0,ans2=0;
        for(int i=1;i<=n;i++)
        {
            int x,y,z;
            cin>>x>>y>>z;
            ans1+=x;
            ans2+=z;
            Minxy=min(x+y,Minxy);
            Minyz=min(y+z,Minyz);
        }
        if(ans1>ans2)
        {
            cout<<ans1+Minyz<<endl;
        }
        else cout<<ans2+Minxy<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
