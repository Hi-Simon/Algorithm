#  [HDU-4347-One hundred layer-dp-单调队列优化](https://vjudge.net/problem/HDU-4374)



## 【Description】

> ```
> Now there is a game called the new man down 100th floor. The rules of this game is:
> 　　 1.  At first you are at the 1st floor. And the floor moves up. Of course you 
> can choose which part you will stay in the first time.
> 　　 2.  Every floor is divided into M parts. You can only walk in a direction (left
> or right). And you can jump to the next floor in any part, however if you are now
> in part “y”, you can only jump to part “y” in the next floor! (1<=y<=M)
> 　　 3.  There are jags on the ceils, so you can only move at most T parts each floor.
> 　　 4.  Each part has a score. And the score is the sum of the parts’ score sum you
> passed by.
> Now we want to know after you get the 100th floor, what’s the highest score you can
> get. 
> ```

## 【Input】

> ```
> The first line of each case has four integer N, M, X, T(1<=N<=100, 1<=M<=10000, 
> 1<=X， T<=M). N indicates the number of layers; M indicates the number of parts. At 
> first you are in the X-th part. You can move at most T parts in every floor in only 
> one direction.
> Followed N lines, each line has M integers indicating the score. (-500<=score<=500)
> ```

## 【Output】

> ```
> Output the highest score you can get.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 3 3 2 1
> 7 8 1 
> 4 5 6 
> 1 2 3 
> ```

> ###Sample Output

> ```
> 29
> ```

------



## 【Problem Description】

> ```
> 给你一个n*m的矩阵，从第一行第x列开始走，一直到最后一行，求所经过格子的值的和的最大值。
> 1、同一列只能选择一个方向，向左或右。
> 2、同一列走的最大距离不超过t
> 样例的路径:
> 8->7->4->5->2->3
> ```

## 【Solution】

> ```
> dp,单调队列优化
> 定义dp数组dp[i][j],表示到第i行，第j列所能获得最大值。
> 则dp[i][j]=max(dp[i][j],dp[i-1][k]-sum[i][k-1]+sum[i][j])向右走
>   dp[i][j]=max(dp[i][j],dp[i-1][k]+sum[i][k]-sum[i][j-1])向左走
> 剩下的就是单调队列模板了...
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-30 14:15:55 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-30 16:36:57
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 105
#define maxm 10005
int a[maxn][maxm];
int dp[maxn][maxm];
int sum[maxn][maxm];
int n, m, x, t;
int head,tail;
struct node
{
    int val,k;
}q[maxm*2];
void right(int i,int j)
{
    while(head<tail&&dp[i-1][j]-sum[i][j-1]>=q[tail-1].val) tail--;//dp[i-1][k]-sum[i][k]
    q[tail].val=dp[i-1][j]-sum[i][j-1];
    q[tail++].k=j;
    while(head==-1||(head<tail&&j-q[head].k>t)) head++;//保证步数不大于限制条件
    dp[i][j]=max(dp[i][j],q[head].val+sum[i][j]);//dp[i][j]=max(dp[i][j],dp[i-1][k]-sum[i][k-1]+sum[i][j])
}
void left(int i,int j)
{
    while(head<tail&&dp[i-1][j]+sum[i][j]>=q[tail-1].val) tail--;//dp[i-1][k]+sum[i][k];
    q[tail].val=dp[i-1][j]+sum[i][j];
    q[tail++].k=j;
    while(head==-1||(head<tail&&q[head].k-j>t)) head++;//保证步数不大于限制条件
    dp[i][j]=max(dp[i][j],q[head].val-sum[i][j-1]);//dp[i][j]=max(dp[i][j],dp[i-1][k]+sum[i][k]-sum[i][j-1])
}
void solve()
{
    for(int i=1;i<=n;i++) for(int j=1;j<=m;j++) dp[i][j]=-INF;
    for(int i=1;i<=m;i++)//第一行初始化
    {
        if(abs(x-i)>t) continue;
        if(i<=x) dp[1][i]=sum[1][x]-sum[1][i-1];
        else dp[1][i]=sum[1][i]-sum[1][x-1];
    }
    for(int i=2;i<=n;i++)
    {
        head=tail=0;
        for(int j=m;j>=1;j--) left(i,j);
        head=tail=0;
        for(int j=1;j<=m;j++) right(i,j);
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    while (cin >> n >> m >> x >> t)
    {
        memset(sum, 0, sizeof(sum));
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= m; j++)
            {
                cin >> a[i][j];
                sum[i][j] = sum[i][j - 1] + a[i][j];
            }
        solve();
        int ans = dp[n][1];
        for (int i = 2; i <= m; i++)
            ans = max(ans, dp[n][i]);
        cout << ans << endl;
    }
    cin.get(), cin.get();
    return 0;
}
```
