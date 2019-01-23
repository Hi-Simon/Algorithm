#  [HDU-3401-Trade-dp-单调队列优化](https://vjudge.net/problem/HDU-3401)

---



## 【Description】

> ```markdown
> Recently, lxhgww is addicted to stock, he finds some regular patterns after a few
> days' study.
> He forecasts the next T days' stock market. On the i'th day, you can buy one stock
> with the price APi or sell one stock to get BPi.
> There are some other limits, one can buy at most ASi stocks on the i'th day and at
> most sell BSi stocks.
> Two trading days should have a interval of more than W days. That is to say,suppose
> you traded (any buy or sell stocks is regarded as a trade)on the i'th day, the next
> trading day must be on the (i+W+1)th day or later.
> What's more, one can own no more than MaxP stocks at any time.
> 
> Before the first day, lxhgww already has infinitely money but no stocks, of course
> he wants to earn as much money as possible from the stock market. So the question
> comes, how much at most can he earn? 
> ```

## 【Input】

> ```
> The first line is an integer t, the case number.
> The first line of each case are three integers T , MaxP , W .
> (0 <= W < T <= 2000, 1 <= MaxP <= 2000) .
> The next T lines each has four integers APi，BPi，ASi，BSi
> ( 1<=BPi<=APi<=1000,1<=ASi,BSi<=MaxP), which are mentioned above. 
> ```

## 【Output】

> ```
> The most money lxhgww can earn. 
> ```

------



## 【Examples 】

> ### Sample Input

> ```
> 1
> 5 2 0
> 2 1 1 1
> 2 1 1 1
> 3 2 1 1
> 4 3 1 1
> 5 4 1 1
> ```

> ###Sample Output

> ```
> 3
> ```

------



## 【Problem Description】

> ```
>  知道未来T天的股票买入价格和卖出价格，在满足以下条件下求所能获得的最大收益。
>  1、一次买入或卖出操作后，必须间隔W天才能进行第二次操作。
>  2、每天买入和卖出的股票数有限制。
> ```

## 【Solution】

> ```
>  dp，单调队列优化
>  定义dp数组dp[i][j],表示第i天买j只股票所能获得的最大收益。
>  则买入的状态转移方程：dp[i][j]=max(dp[i][j],dp[i-W-1][k]-(j-k)*Ap[i])
>                              =max(dp[i][j],dp[i-W-1][k]+k*Ap[i]-j*Ap[i])
>    卖出的状态转移方程：dp[i][j]=max(dp[i][j],dp[i-W-1][k]+(k-j)*Bp[i])
>                              =max(dp[i][j],dp[i-W-1][k]+k*Bp[i]-j*Bp[i])
>  无论是买入或卖出，dp[i-W-1][k]+k*Xp[i]是跟k相关的值，j*Xp[i]是与j相关的值
>  而且题目要求是最大收益，对于同一个j，枚举所有的k求dp[i-W-1][k]+k*Xp[i]的最大
>  值就好了，那么就可以将这个式子通过单调队列来维护。
>  注意最后求值的时候，还要保证abs(k-j)<=Xs[i]，即买入或卖出的数量不超过限制。
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon
 * @Date: 2018-08-17 09:12:27
 * @Last Modified by:   Simon
 * @Last Modified time: 2018-08-17 09:35:57
 */
#include <bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 2015
int Api[maxn]; int Bpi[maxn];
int Asi[maxn]; int Bsi[maxn];
int f[maxn][maxn];
int head,tail;
int T, MaxP, W;
struct node
{
    int val,num;
}q[maxn*2];
void buy(int i,int j)
{
    while(head<tail&&q[tail-1].val<f[i-W-1][j]+j*Api[i]) tail--;//dp[i][k]+k*Api[i];
    q[tail].val=f[i-W-1][j]+j*Api[i];
    q[tail++].num=j;
    while(head==-1||(head<tail&&j-q[head].num>Asi[i])) head++;//保证买的数目不超过限制
    f[i][j]=max(f[i][j],q[head].val-j*Api[i]);//dp[i][j]=max(dp[i][j],dp[i][k]-(j-k)*Api[i])
}
void sale(int i,int j)
{
    while(head<tail&&q[tail-1].val<f[i-W-1][j]+j*Bpi[i]) tail--;//dp[i][k]+k*Bpi[i]
    q[tail].val=f[i-W-1][j]+j*Bpi[i];
    q[tail++].num=j;
    while(head==-1||(head<tail&&q[head].num-j>Bsi[i])) head++;//保证卖的数目不超过限制
    f[i][j]=max(f[i][j],q[head].val-j*Bpi[i]);//dp[i][j]=max(dp[i][j],dp[i][k]+(k-j)*Bpi[i])
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin >> t;
    while (t--)
    {
        cin >> T >> MaxP >> W;
        for (int i = 1; i <= T; i++) cin >> Api[i] >> Bpi[i] >> Asi[i] >> Bsi[i];
        for(int i=0;i<=T;i++)
            for(int j=0;j<=MaxP;j++) f[i][j]=-1e8;

        for(int i=1;i<=T;i++) f[i][0]=0;//任意一天不买不卖所获得的收益为0
        for(int i=1;i<=W+1;i++)
        {
            for(int j=0;j<=Asi[i];j++)
            {
                f[i][j]=-j*Api[i];//前w+1天，只能买进
            }
        }
        f[0][0]=0;
        for(int i=2;i<=T;i++)
        {
            head=0,tail=0;
            for(int j=0;j<=MaxP;j++) f[i][j]=max(f[i-1][j],f[i][j]);//不买不卖时的最大值
            if(i<=W+1) continue;
            for(int j=0;j<=MaxP;j++) buy(i,j);//因为是买进，所以比应该正序，保证比当前买的数目小的状态都已算过
            head=0,tail=0;
            for(int j=MaxP;j>=0;j--) sale(i,j);//相反...
        }
        int ans=-1e8;
        for(int j=0;j<=MaxP;j++) ans=max(ans,f[T][j]);
        cout<<ans<<endl;
    }
    cin.get(), cin.get();
    return 0;
}
```
