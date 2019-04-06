# [Codeforces Round #544 (Div. 3)-E. K Balanced Teams-在一定条件下n个学生分成k组，所能分配的最大学生数-dp or 记忆化搜索](https://codeforces.com/contest/1133/problem/E)

## 【Description】

```cpp
You are a coach at your local university. There are n students under your 
supervision, the programming skill of the i-th student is ai.

You have to form k teams for yet another new programming competition. As you 
know, the more students are involved in competition the more probable the 
victory of your university is! So you have to form no more than k (and at least 
one) non-empty teams so that the total number of students in them is maximized. 
But you also know that each team should be balanced. It means that the 
programming skill of each pair of students in each team should differ by no 
more than 5. Teams are independent from one another (it means that the 
difference between programming skills of two students from two different teams 
does not matter).

It is possible that some students not be included in any team at all.

Your task is to report the maximum possible total number of students in no more 
than k (and at least one) non-empty balanced teams.

If you are Python programmer, consider using PyPy instead of Python when you 
submit your code.
```

## 【Input】

```cpp
The first line of the input contains two integers n and k (1≤k≤n≤5000) — the 
number of students and the maximum number of teams, correspondingly.

The second line of the input contains n integers a1,a2,…,an (1≤ai≤109), where 
ai is a programming skill of the i-th student.
```

## 【Output】

```cpp
Print one integer — the maximum possible total number of students in no more 
than k (and at least one) non-empty balanced teams.
```

------



## 【Examples】 

#### Sample Input

```cpp
5 2
1 2 15 15 15
```

#### Sample Output

```cpp
5
```
#### Sample Input

```cpp
6 1
36 4 1 25 9 16
```

#### Sample Output

```cpp
2
```
#### Sample Input

```cpp
4 4
1 10 100 1000
```

#### Sample Output

```cpp
4
```

------



## 【Problem Description】

```cpp
给你一个长度为n的序列，表示有n个学生，每个学生的技巧值为ai,将这n个学生分成k各组，并且每
个组里的任意两个学生的技巧值之差不超过5，问最多能分配多少个学生。
```

## 【Solution】

```cpp
记忆化搜索式dp
    对初始序列排序后，首先O(n)复杂度预处理出，对于每个起点t（1<=t<=n)，所对应的终点位
    置在哪。也就是第一个位置i,满足a[i]-a[t]>5的位置。
    定义dp[i][j]数组，表示前n-i+1个学生，已组成j个组，所分配的最大学生数
    动态转移方程：
        dp[i][j]=max(dp[i+1][j],dp[Next[i]][j-1]+Next[i]-i); 
        //Next[i]表示以i为起点，所对应的终点位置为Next[i]
        dp[i+1][j]表示，第n-(i+1)+1与第n-i+1个人同时分在第j组
        dp[Next[i]][j-1]+Next[i]-i表示，第n-Next[i]+1个人分在第j-1组，第n-i+1个人
        分在第j组。

递推式dp
    大体的状态转移过程不变，但是有很多细节处理。具体看代码注释。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-03-31 16:29:11 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-04-01 13:34:33
 */
#include<bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 5005
int a[maxn],Next[maxn];
int dp[maxn][maxn];int n; //n-i+1个学生，分为j组，所能分配的最大学生数
int dfs(int x,int k){
    if(x==n+1) return 0;
    if(dp[x][k]!=-1) return dp[x][k];
    return dp[x][k]=max(dfs(x+1,k),(k>0?dfs(Next[x],k-1)+Next[x]-x:0)); //第x+1个学生与第x个学生在同一组；使用一个新组
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    memset(dp,-1,sizeof(dp));
    int k;cin>>n>>k;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    sort(a+1,a+n+1);a[n+1]=INF;
    int i=1;
    for(int t=1;t<=n;t++){ //预处理
        while(a[i]-a[t]<=5){
            i++;
        }
        Next[t]=i; //处理每个起点对应的终点位置。
    }
    cout<<dfs(1,k)<<endl;
    cin.get(),cin.get();
    return 0;
}
```

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-04-01 07:59:09 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-04-01 08:57:46
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 5005
int a[maxn];
int dp[maxn][maxn];//包含a[i]的前i个学生，分为j组，所能分配的最大学生数
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,k;cin>>n>>k;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    sort(a+1,a+n+1);a[n+1]=INF;
    for(int j=1;j<=k+1;j++){ //j要循环到k+1是因为要能够更新到dp[j][k],看下面的转移方程
        int next=0;
        for(int i=1;i<=n;i++){
            while(a[next]-a[i]<=5) next++; //找出下个组的起点
            dp[i][j-1]=max(dp[i][j-1],dp[i-1][j-1]); //第i个数与第i-1个数分在同一组
            dp[next][j]=max(dp[next][j],dp[i][j-1]+next-i); //第i个数与第next个数不在一组，要增加一组
        }
        dp[n+1][j-1]=max(dp[n+1][j-1],dp[n][j-1]); //因为next指向下个组的起点，如果是最后一组，那么next就指向n+1
    }                                                     //所以要更新第n+1个位置，即答案
    cout<<dp[n+1][k]<<endl;
    cin.get(),cin.get();
    return 0;
}
```