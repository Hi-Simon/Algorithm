# [Codeforces Round #495 (Div. 2)](http://codeforces.com/contest/1004) - E. Sonya and Ice Cream 

[TOC]



## ``Description ``

> Sonya likes ice cream very much. She eats it even during programming  competitions. That is why the girl decided that she wants to open her  own ice cream shops.
>
> Sonya lives in a city with n junctions and n−1 streets between them. All streets are two-way and connect two  junctions. It is possible to travel from any junction to any other using one or more streets. City Hall allows opening shops only on junctions.  The girl cannot open shops in the middle of streets.  
>
> Sonya has exactly k friends whom she can trust. If she opens a shop, one of her friends has to work there and not to allow anybody to eat an ice cream not paying  for it. Since Sonya does not want to skip an important competition, she  will not work in shops personally. 
>
> Sonya wants all her ice cream shops to form a simple path of the length r (1≤r≤k), i.e. to be located in different junctions f1,f2,…,fr and there is street between fi and fi+1 for each i from 1 to r−1. 
>
> The girl takes care of potential buyers, so she also wants to minimize  the maximum distance between the junctions to the nearest ice cream  shop. The distance between two junctions a and b is equal to the sum of all the street lengths that you need to pass to get from the junction a to the junction b. So Sonya wants to minimize 
> $$
> \max_{a} \min_{1 \le i \le r} d_{a,f_i}
> $$
> where a takes a value of all possible n junctions, fi — the junction where the i-th Sonya's shop is located, and dx,y — the distance between the junctions x and y. 
>
> Sonya is not sure that she can find the optimal shops locations, that is why she is asking you to help her to open not more than k shops that will form a simple path and the maximum distance between any junction and the nearest shop would be minimal.  

## `Input`

> The first line contains two integers n and k (1≤k≤n≤105) — the number of junctions and friends respectively. 
>
> Each of the next n−1 lines contains three integers ui, vi, and di (1≤ui,vi≤n, vi≠ui, 1≤d≤104) — junctions that are connected by a street and the length of this street. It is guaranteed that each pair of junctions is connected by at most  one street. It is guaranteed that you can get from any junctions to any  other. 

## `Output`

> Print one number — the minimal possible maximum distance that you need  to pass to get from any junction to the nearest ice cream shop. Sonya's  shops must form a simple path and the number of shops must be at most k. 

## `Examples` 

> ### `Input`

> ```
> 6 2
> 1 2 3
> 2 3 4
> 4 5 2
> 4 6 3
> 2 4 6
> ```

> ###Ouput

> ```
> 4
> ```

> ###Input

> ```
> 10 3
> 1 2 5
> 5 7 2
> 3 2 6
> 10 6 3
> 3 8 1
> 6 4 2
> 4 1 6
> 6 9 4
> 5 2 5
> ```

> ###Output

> ```
> 7
> ```

## `题意`

> 给你一个树，让你找到连续的k个点，保证，其余各点，到最近的属于k个点的 点的最大距离最小。
>
> 给个图理解一下。
>
> ![img](http://codeforces.com/predownloaded/91/be/91be4ef94421267b1f73b388ce9c02e61b7ba025.png) 

## `题解`

> 贪心算法。
>
> ①：将所有叶子节点与其相邻节点的距离从小到大排序（实时排序，每更新一个节点就要排序一次，可以通过特殊容器自动进行）。
>
> ②：从距离最小的开始，删除当前叶子节点，并将其距离值加到相邻的非叶子节点的路径值上。
>
> 循环步骤②直到剩余节点数为k。

## `代码`

```
#include <bits/stdc++.h>
using namespace std;
#define maxn 100005
set<pair<int, int>> s, d[maxn]; //存储图
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int n, k;
    cin >> n >> k;
    for (int i = 2; i <= n; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        d[u].insert(make_pair(v, w));
        d[v].insert(make_pair(u, w)); //无向图，双向存储
    }
    for (int i = 1; i <= n; i++)
    {
        if (d[i].size() == 1) //找到叶子节点，并存储
        {
            s.insert(make_pair((*d[i].begin()).second, i));
        }
    }
    int ans;
    while (n > k || s.size() > 2) //剩余节点个数等于k时，结束循环
    {
        ans = (*s.begin()).first; //剩余节点数为n个时，的最长距离
        int i = (*s.begin()).second;
        s.erase(*s.begin());
        int next = (*d[i].begin()).first;
        d[next].erase(d[next].lower_bound(make_pair(i, 0))); //删除当前叶子节点
        n--;                                                 //节点个数减1
        if (d[next].size() == 1)                             //重新检测，删除上一个节点以后，当前节点是否为叶子节点。
        {
            s.insert(make_pair((*d[next].begin()).second + ans, next));
        }
    }
    cout << ans << endl;
    return 0;
}
```

