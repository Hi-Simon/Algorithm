#  [2018-ACM-ICPC-徐州赛区网络赛-B. BE, GE or NE-博弈论+dp-记忆化搜索](https://nanti.jisuanke.com/t/31454)



## 【Description】

> ```
> In a world where ordinary people cannot reach, a boy named "Koutarou" and a girl 
> named "Sena" are playing a video game. The game system of this video game is quite 
> unique: in the process of playing this game, you need to constantly face the 
> choice, each time you choose the game will provide 1−31-31−3 options, the player 
> can only choose one of them. Each option has an effect on a "score" parameter in 
> the game. Some options will increase the score, some options will reduce the score, 
> and some options will change the score to a value multiplied by −1-1−1 .
> That is, if there are three options in a selection, the score will be increased by 
> 111, decreased by 111, or multiplied by −1-1−1. The score before the selection is 
> 888. Then selecting option 111 will make the score become 999, and selecting option
> 222 will make the score 777 and select option 333 to make the score −8-8−8. Note 
> that the score has an upper limit of 100100100 and a lower limit of −100-100−100. 
> If the score is 999999 at this time, an option that makes the score +2+2+2 is 
> selected. After that, the score will change to 100100100 and vice versa .
> After all the choices have been made, the score will affect the ending of the game. 
> If the score is greater than or equal to a certain value kkk, it will enter a good 
> ending; if it is less than or equal to a certain value lll, it will enter the bad 
> ending; if both conditions are not satisfied, it will enter the normal ending. Now,
> Koutarou and Sena want to play the good endings and the bad endings respectively. 
> They refused to give up each other and finally decided to use the "one person to 
> make a choice" way to play the game, Koutarou first choose. Now assume that they 
> all know the initial score, the impact of each option, and the kkk, lll values, and 
> decide to choose in the way that works best for them. (That is, they will try their 
> best to play the ending they want. If it's impossible, they would rather normal 
> ending than the ending their rival wants.)
> Koutarou and Sena are playing very happy, but I believe you have seen through the 
> final ending. Now give you the initial score, the kkk value, the lll value, and the 
> effect of each option on the score. Can you answer the final ending of the game?
> 
> ```

## 【Input】

> ```
> The first line contains four integers n,m,k,ln,m,k,ln,m,k,l（1≤n≤10001\le n \le 
> 10001≤n≤1000, −100≤m≤100-100 \le m \le 100−100≤m≤100 , −100≤l<k≤100-100 \le l < k 
> \le 100−100≤l<k≤100 ）, represents the number of choices, the initial score, the 
> minimum score required to enter a good ending, and the highest score required to 
> enter a bad ending, respectively.
> Each of the next nnn lines contains three integers a,b,ca,b,ca,b,c（a≥0a\ge 0a≥0 ,
> b≥0b\ge0b≥0 ,c=0c=0c=0 or c=1c=1c=1）,indicates the options that appear in this 
> selection,in which a=0a=0a=0 means there is no option to increase the score in this 
> selection, a>0a>0a>0 means there is an option in this selection to increase the 
> score by aaa ; b=0b=0b=0 means there is no option to decrease the score in this 
> selection, b>0b>0b>0 means there is an option in this selection to decrease the 
> score by bbb; c=0c=0c=0 means there is no option to multiply the score by −1-1−1 in 
> this selection , c=1c=1c=1 means there is exactly an option in this selection to 
> multiply the score by −1-1−1. It is guaranteed that a,b,ca,b,ca,b,c are not equal 
> to 000 at the same time.
> ```

## 【Output】

> ```
>  One line contains the final ending of the game. If it will enter a good 
> ending,print "Good Ending"(without quotes); if it will enter a bad ending,print 
> "Bad Ending"(without quotes);otherwise print "Normal Ending"(without quotes).
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 3 -8 5 -5
> 3 1 1
> 2 0 1
> 0 2 1
> ```

> ###Sample Output

> ```
> Good Ending
> ```

>   ### Sample Input

>   ```
>   3 0 10 3
>   0 0 1
>   0 10 1
>   0 2 1
>   ```

>   ### Sample Output

>   ```
>   Bad Ending
>   ```

------



## 【Problem Description】

> ```
> 两个人玩游戏，有n个轮次，初始分数为m，每个轮次有三种选择，将m增加a，将m减少b，如果c==1,可以将m取
> 相反数。只能选择一个对m进行操作。K玩家想要使最终分数大于等于k，S玩家想要使最终分数小于等于l，K玩家
> 先手，问最终K玩家能否使分数大于等于k。
> ```

## 【Solution】

> ```
> 博弈论+dp，记忆化搜索。（挺好的题）
> 定义dp[i][j]为第i个轮次，分数为j时，所能得到的最优分数为dp[i][j]
> n个轮次，0~n-1，并且K玩家先手，所以进行搜索时，轮次t为偶数时，求最大值，否则求最小值。
> 因为分数可能为负数，所以增加100的偏移量，便于存储。
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-09 19:35:20 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-09 20:41:00
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 1005

int n, m, k, l;
struct node
{
    int a,b,c;
    node(){}
    node(int _a,int _b,int _c):a(_a),b(_b),c(_c){}
}p[maxn];
int dp[maxn][205];//dp[i][j]为第i个轮次，分数为j时，所能得到的最优分数为dp[i][j]

int dfs(int t,int score)
{
    if(t>=n) return score-100;
    if(dp[t][score]!=INF) return dp[t][score];
    if(t%2==0)//K玩家
    {
        int ans=score-100,tmp=-100;
        if(p[t].a>0)
        {
            int n_ans=min((int)100,ans+p[t].a);
            tmp=max(tmp,dfs(t+1,n_ans+100));
        }
        if(p[t].b>0)
        {
            int n_ans=max((int)-100,ans-p[t].b);
            tmp=max(tmp,dfs(t+1,n_ans+100));
        }
        if(p[t].c>0)
        {
           tmp=max(tmp,dfs(t+1,-ans+100));
        }
        dp[t][score]=tmp;
        return tmp;
    }
    else//S玩家
    {
        int ans=score-100,tmp=100;
        if(p[t].a>0)
        {
            int n_ans=min((int)100,ans+p[t].a);
            tmp=min(tmp,dfs(t+1,n_ans+100));
        }
        if(p[t].b>0)
        {
            int n_ans=max((int)-100,ans-p[t].b);
            tmp=min(tmp,dfs(t+1,n_ans+100));
        }
        if(p[t].c>0)
        {
            tmp=min(tmp,dfs(t+1,-ans+100));
        }
        dp[t][score]=tmp;
        return tmp;
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin>>n>>m>>k>>l;
    for(int i=0;i<maxn;i++) for(int j=0;j<205;j++) dp[i][j]=INF;
    for(int i=0;i<n;i++) cin>>p[i].a>>p[i].b>>p[i].c;

    int ans=dfs(0,m+100);
    if(ans>=k) puts("Good Ending");
    else if(ans<=l) puts("Bad Ending");
    else puts("Normal Ending");

    cin.get(),cin.get();
    return 0;
}
```
