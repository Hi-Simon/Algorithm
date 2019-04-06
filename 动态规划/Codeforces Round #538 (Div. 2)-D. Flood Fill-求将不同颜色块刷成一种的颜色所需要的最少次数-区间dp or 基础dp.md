# [Codeforces Round #538 (Div. 2)-D. Flood Fill-求将不同颜色块刷成一种的颜色所需要的最少次数-区间dp or 基础dp](https://codeforces.com/contest/1114/problem/D) 

## 【Description】

```cpp
You are given a line of n colored squares in a row, numbered from 1 to n from 
left to right. The i-th square initially has the color ci.

Let's say, that two squares i and j belong to the same connected component if 
ci=cj, and ci=ck for all k satisfying i<k<j. In other words, all squares on the 
segment from i to j should have the same color.

For example, the line [3,3,3] has 1 connected component, while the line [5,2,4,
4] has 3 connected components.

The game "flood fill" is played on the given line as follows:

At the start of the game you pick any starting square (this is not counted as a 
turn).

Then, in each game turn, change the color of the connected component containing 
the starting square to any other color.

Find the minimum number of turns needed for the entire line to be changed into 
a single color.
```

## 【Input】

```cpp
The first line contains a single integer n (1≤n≤5000) — the number of squares.

The second line contains integers c1,c2,…,cn (1≤ci≤5000) — the initial colors 
of the squares.
```

## 【Output】

```cpp
Print a single integer — the minimum number of the turns needed.
```

------



## 【Examples】 

#### Sample Input

```cpp
4
5 2 2 1
```

#### Sample Output

```cpp
2
```
#### Sample Input

```cpp
8
4 5 2 2 1 3 5 5
```

#### Sample Output

```cpp
4
```
#### Sample Input

```cpp
1
4
```

#### Sample Output

```cpp
0
```

------



## 【Problem Description】

```cpp
给你一个长度为n的序列，每个数字代表一种颜色，每次可以将一种颜色刷成另一种颜色，问将此序
列刷成同一种颜色所需要的最少次数。
```

## 【Solution】

```cpp
方法1：
    对于每个序列，先对其进行去重。比如：
        因为每次只能对一种颜色操作，所以对于4 4 4 5 5 4 4这个序列，与序列4 5 4在
    操作上是一致的。

        对于去重之后的数，若每个数都是独一无二的，比如1 2 3，那么答案就是 个数-1=2次
    若存在重复的数，比如4 5 4，本来需要2次，但是第三个位置和第一个位置相同所以可以节省
    一次，最终就是 个数-1-节省的次数=1次

    对于这个序列，3 2 4 5 4 1 3，可节省的次数就为2次，因为去掉2和1后为：3 4 5 4 3
    所以最终就是找最长回文子序列。
    找最长回文子序列有两种方法：
    方法1：
        对去重后的序列a取反，得到序列b，然后对序列a和b求最长公共子序列，求得的长度就是
        回文子序列的长度。
    方法2：
        区间dp，定义数组dp[i][j]，表示区间[i,j]中的最长回文子序列长度为dp[i][j]
        状态转移方程为：
            if(a[i]==a[j]) dp[i][j]=dp[i+1][j-1]+1);
            else dp[i][j]=max(dp[i+1][j],dp[i][j-1]);
方法2：
    直接区间dp
    定义数组dp[i][j][2],dp[i][j][0]表示将区间[i,j]刷成a[i]颜色所需要的最少次数
    dp[i][j][1]表示将区间[i,j]刷成a[j]颜色所需要的最少次数

    若将区间[i,j]刷成a[i]颜色所需要的最少次数为 将区间[i+1,j]刷成a[i+1]颜色所需的最少
    次数和将区间[i+1,j]刷成a[j]颜色所需要的最少次数中 最少的那个：
        dp[i][j][0]=min(dp[i+1][j][0]+(a[i+1]!=a[i]),dp[i+1][j][1]+(a[j]!=a[i]));

    若将区间[i,j]刷成a[j]颜色所需要的最少次数为 将区间[i,j-1]刷成a[i]颜色所需要的最少
    次数和将区间[i,j-1]刷成a[j-1]颜色所需要的最少次数中 最少的那个：
        dp[i][j][1]=min(dp[i][j-1][0]+(a[i]!=a[j]),dp[i][j-1][1]+(a[j-1]!=a[j]));

    ps:后面的加上a[x]!=a[y]是表示 如果a[x]==a[y]，表示两个区间颜色相同，不需要刷，如
    果a[x]!=a[y]则需要刷一次，才能使他们区间颜色相同
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-03-30 14:47:55 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-03-31 14:13:19
 */
#include<bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 5005
int a[maxn],dp[maxn][maxn][2];
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;cin>>n;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            dp[i][j][0]=dp[i][j][1]=INF;
        }
    }
    for(int i=1;i<=n;i++){
        cin>>a[i];dp[i][i][0]=dp[i][i][1]=0;
    }
    for(int x=1;x<n;x++){
        for(int i=1;i+x<=n;i++){
            int j=x+i;
                dp[i][j][0]=min(dp[i][j][0],dp[i+1][j][0]+( a[i+1]!=a[i] ));
                dp[i][j][0]=min(dp[i][j][0],dp[i+1][j][1]+( a[j]!=a[i] ));
                dp[i][j][1]=min(dp[i][j][1],dp[i][j-1][0]+( a[i]!=a[j] ));
                dp[i][j][1]=min(dp[i][j][1],dp[i][j-1][1]+( a[j-1]!=a[j] )); 
        }
    }
    int ans=min(dp[1][n][0],dp[1][n][1]);
    if(ans==INF) cout<<0<<endl;
    else cout<<ans<<endl;
    cin.get(),cin.get();
    return 0;
}
```

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-03-31 12:36:54 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-03-31 13:02:23
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 5005
int a[maxn],dp[maxn][maxn],b[maxn];
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i];
    n=unique(a+1,a+n+1)-a-1; //去重
    //方法一：
    for(int i=1;i<=n;i++) b[n-i+1]=a[i]; //对a取反
    for(int i=1;i<=n;i++){ //对a和b数组求最长公共子序列，即数组a的最长回文子序列
        for(int j=1;j<=n;j++){
            if(a[i]==b[j]) dp[i][j]=max(dp[i][j],dp[i-1][j-1]+1);
            else dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
        }
    }
    cout<<n-(dp[n][n]+1)/2<<endl; //对于3 2 3，dp[n][n]求出来的是3，所以需要加1除以2
    //方法二：
    // for(int x=0;x<=n;x++){//区间dp求最长回文子序列
    //     for(int i=1;i+x<=n;i++){
    //         int j=x+i;
    //         if(a[i]==a[j]) dp[i][j]=dp[i+1][j-1]+1;
    //         else dp[i][j]=max(dp[i+1][j],dp[i][j-1]);
    //     }
    // }
    // cout<<n-dp[1][n]<<endl; //对于3 2 3，dp[1][n]求出来的就是2
    cin.get(),cin.get();
    return 0;
}
```