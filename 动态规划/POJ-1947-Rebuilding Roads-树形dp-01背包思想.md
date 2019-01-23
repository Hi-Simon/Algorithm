#  [POJ-1947-Rebuilding Roads-树形dp-01背包思想](https://vjudge.net/problem/POJ-1947)

[TOC]



## Description 

> ```
> The cows have reconstructed Farmer John's farm, with its N barns (1 <= N <= 150,
> number 1..N) after the terrible earthquake last May. The cows didn't have time to 
> rebuild any extra roads, so now there is exactly one way to get from any given barn
> to any other barn. Thus, the farm transportation system can be represented as a 
> tree.
> 
> Farmer John wants to know how much damage another earthquake could do. He wants to
> know the minimum number of roads whose destruction would isolate a subtree of 
> exactly P (1 <= P <= N) barns from the rest of the barns. 
> ```

## Input

> ```
> * Line 1: Two integers, N and P
> 
> * Lines 2..N: N-1 lines, each with two integers I and J. Node I is node J's parent 
> in the tree of roads. 
> ```

## Output

> ```
> A single line containing the integer that is the minimum number of roads that need 
> to be destroyed for a subtree of P nodes to be isolated. 
> ```

------



## Examples 

> ### Input

> ```
> 11 6
> 1 2
> 1 3
> 1 4
> 1 5
> 2 6
> 2 7
> 2 8
> 4 9
> 4 10
> 4 11
> ```

> ###Hint

> ```
> [A subtree with nodes (1, 2, 3, 6, 7, 8) will become isolated if roads 1-4 and 1-5
> are destroyed.] 
> ```

------



## Problem Description

> ```
> 给你一棵树，要最后剩下P个点的子树，最少删去多少条边。
> ```

## Solution

> ```
> 树形dp。背包思想。
> 定义dp数组，dp[i][j],为以i为根节点，保留j个节点最少需要删除dp[i][j]条边。
> 
> 然后我们通过样例来说状态转移。
> 对于每个节点u，dp[u][1],即只保留一个节点即自己的时候，需要删除与u相连的所有边。例：dp[1][1]=4。
> 就等于u节点的出度。
> 
> 对于dp[u][(2...P)],也就是dp[u][j]=min(dp[u][j],dp[u][k]+dp[v][j-k]-2);(u为根节点，v为u
> 的子节点且2<=j<=p，1<=k<j)
> 例：dp[2][2]=min(dp[2][2],dp[2][1]+dp[6][1]-2)=dp[2][1]+dp[6][1]-2=2号节点的出度+6号节
> 点的出度-2=4+1-2=3.
> 
> 可以把样例的图画出来，结合上面的例子，来理解，很好理解的...
> 最重要的就是最后的（-2），其实也好理解，dp[2][2]表示要保留两个顶点需要删除的边数，如果这两个顶点是
> 2和6，那么不就等于dp[2][1]+dp[6][1],仔细观察一下，保留2和6两个顶点，并且形成子树，肯定要将2和6
> 相连，即2-6这条边不能删除，但dp[2][1]和dp[6][1]都将其删除并计数了一次，所以我们需要将其和-2.
> 
> 对于j倒序的原因，用两张图来解释一下。其实就是01背包问题v倒序的原理。
> 输出模式：
> dp[u][j](t1)=dp[u][k](t1)+dp[v][j-k](t3)-2，其中dp[i][j](t) 表示dp[i][j]==t。
> 倒序：									  
> ```
>
> ![1535254490175](C:\Users\asus\AppData\Local\Temp\1535254490175.png)
>
> ```
> 正序：
> ```
>
> ![1535254740700](C:\Users\asus\AppData\Local\Temp\1535254740700.png)

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-25 20:41:07 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-25 22:53:01
 */
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 155
vector<int> g[maxn];
int du[maxn];
int dp[maxn][maxn];//为以i为根节点，保留j个节点最少需要删除dp[i][j]条边。
int n, p;
int dfs(int u, int r = -1)
{
    dp[u][1] = du[u];//以当前节点为根节点，剩余1个节点需删除的节点个数为其儿子总数
    for (int i = 0; i < g[u].size(); i++)
    {
        int v = g[u][i];
        if (v != r)
        {
            dfs(v, u);
            for (int j = p; j >= 1; j--)//与背包问题倒序一个原理
                for (int k = 1; k < j; k++)//当前节点从小到大求，儿子节点已经求过，可以从大到小求
                {
                    dp[u][j] = min(dp[u][j], dp[u][k] + dp[v][j - k] - 2);//-2是因为某一条连接根节点和儿子节点的边被计数两次。具体可以将下面一行的注释取消，看输出。
                    // printf("dp[ %I64d ][ %I64d ]( %I64d ) = dp[ %I64d ][ %I64d ]( %I64d ]+dp[ %I64d ][ %I64d ]( %I64d ) - 2\n", u, j, dp[u][j], u, k, dp[u][k], v, j - k, dp[v][j - k]);
                }
        }
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n >> p;
    for (int i = 1; i < n; i++)
    {
        int u, v;
        cin >> u >> v;
        du[u]++, du[v]++;//求出每个节点的出度，就是其作为总根时，其儿子个数
        g[u].push_back(v);
        g[v].push_back(u);
    }
    memset(dp, INF, sizeof(dp));
    dfs(1);
    int ans = INF;
    for (int i = 1; i <= n; i++)
        ans = min(ans, dp[i][p]);
    cout << ans << endl;
    cin.get(), cin.get();
    return 0;
}
```
