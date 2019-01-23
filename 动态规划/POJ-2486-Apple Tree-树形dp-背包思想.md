#  [POJ-2486-Apple Tree-树形dp-背包思想](https://vjudge.net/problem/POJ-2486)

[TOC]



## Description 

> ```
> Wshxzt is a lovely girl. She likes apple very much. One day HX takes her to an 
> apple tree. There are N nodes in the tree. Each node has an amount of apples. 
> Wshxzt starts her happy trip at one node. She can eat up all the apples in the 
> nodes she reaches. HX is a kind guy. He knows that eating too many can make the 
> lovely girl become fat. So he doesn’t allow Wshxzt to go more than K steps in the 
> tree. It costs one step when she goes from one node to another adjacent node. 
> Wshxzt likes apple very much. So she wants to eat as many as she can. Can you tell 
> how many apples she can eat in at most K steps. 
> ```

## Input

> ```
> There are several test cases in the input
> Each test case contains three parts.
> The first part is two numbers N K, whose meanings we have talked about just now. We 
> denote the nodes by 1 2 ... N. Since it is a tree, each node can reach any other in 
> only one route. (1<=N<=100, 0<=K<=200)
> The second part contains N integers (All integers are nonnegative and not bigger 
> than 1000). The ith number is the amount of apples in Node i.
> The third part contains N-1 line. There are two numbers A,B in each line, meaning
> that Node A and Node B are adjacent.
> Input will be ended by the end of file.
> 
> Note: Wshxzt starts at Node 1. 
> ```

## Output

> ```
> For each test case, output the maximal numbers of apples Wshxzt can eat at a line. 
> ```

------



## Examples 

> ### Input

> ```
> 2 1 
> 0 11
> 1 2
> 3 2
> 0 1 2
> 1 2
> 1 3
> ```

> ###Output

> ```
> 11
> 2
> ```

------



## Problem Description

> ```
> 一棵树，有n个节点，1号节点是根节点，每个点有ai个苹果，从根节点出发，经过的点数为其路程(起始点不算一
> 步），求总路程不大于k，所能获得的最大苹果数是多少。（苹果摘过以后该节点的苹果数就为0）
> ```

## Solution

> ```
> 树形dp，背包思想。
> 定义dp数组dp[u][p][1]表示以u为根节点，走p步且返回节点u所能获得的最大值为dp[u][p][1]。
> dp[u][p][0]表示以u为根节点，走p步但不返回u节点所能获得的最大值为dp[u][p][0]
> 
> 状态转移方程：
> 如果不回到根节点有两种可能(简化为二叉树来说明)：
> 
>  先走了左边并且停在右子树内(v就是右儿子)dp[u][q][1]就是遍历左子树并且回到根节点的值
>  dp[u][p][0] = max(dp[u][p][0], dp[u][q][1] + dp[v][p - q - 1][0])
>  (1<=p<=k，0<=q<=p-1）
>  
>  先走了右边并且停在左子树内(v还是右儿子)dp[v][p-q-2][1]就是遍历右子树并回到根节点的值
>  dp[u][p][0] = max(dp[u][p][0], dp[u][q][0] + dp[v][p - q - 2][1])
>  (1<=p<=k, 0<=q<=p-2）
>  
> 如果回到跟节点
>  dp[u][p][1] = max(dp[u][p][1], dp[u][q][1] + dp[v][p - q - 2][1])
>  (1<=p<=k，0<=q<=p-2）
>  
> p，q的范围问题，很好理解，p不用说，q的范围应该写成这样：p>=p-q-1>=0,也就是将p拆分为q和p-q-1
> 减1的原因就是根节点到儿子节点有一步。比如1->2这条边
> dp[1][1][0]=dp[1][0][0]+dp[2][0][0];
> 减2的原因就是根节点到儿子节点，再从儿子节点回到根节点总共2步。
> ```

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-27 09:42:37 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-27 18:39:23
 */
#include <iostream>
#include <vector>
#include <cstring>
#include <algorithm>
#include <cstdio>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 150
vector<int> g[maxn];
int val[maxn];
int dp[maxn][maxn * 2][2];//以i为根节点，走j步且返回节点i所能获得的最大值为dp[i][j][1]。 以i为根节点，走j步但不返回i节点所能获得的最大值为dp[i][j][0]
int n, k;
int ans = 0;
void dfs(int u, int r = -1)
{
    dp[u][0][1] = dp[u][0][0] = val[u];//走0步，所获得的值就是节点本身的值
    for (int i = 0; i < g[u].size(); i++)
    {
        int v = g[u][i];
        if (v != r)
        {
            dfs(v, u);
            for (int p = k; p >= 1; p--)//枚举走的总步数，倒序是01背包思想
            {
                for (int q = 0; p - q - 1 >= 0; q++)//枚举分割点
                {
                    dp[u][p][0] = max(dp[u][p][0], dp[u][q][1] + dp[v][p - q - 1][0]);//不回到根节点，而停在儿子节点为根的子树内
                    if (p - q - 2 >= 0)
                    {
                        dp[u][p][0] = max(dp[u][p][0], dp[u][q][0] + dp[v][p - q - 2][1]);//不回到根节点，而停在儿子节点
                        dp[u][p][1] = max(dp[u][p][1], dp[u][q][1] + dp[v][p - q - 2][1]);//回到根节点
                    }
                }
            }
        }
    }
}
int main()
{
    while (scanf("%d%d", &n, &k) != EOF)
    {
        ans = 0; memset(dp, 0, sizeof(dp));
        for (int i = 1; i <= n; i++) scanf("%d", val + i);
        for (int i = 1; i < n; i++)
        {
            int u, v;
            scanf("%d%d", &u, &v);
            g[u].push_back(v);
            g[v].push_back(u);
        }
        dfs(1);
        for (int i = 0; i <= n; i++) g[i].clear();
        for (int i = 0; i <= k; i++) ans = max(ans, max(dp[1][i][0], dp[1][i][1]));
        printf("%d\n", ans);
    }
    cin.get(), cin.get();
    return 0;
}
```
