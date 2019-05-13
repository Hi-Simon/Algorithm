# [Codeforces Global Round 2-D. Frets On Fire-二分](https://codeforces.com/contest/1119/problem/D)

## 【Description】

```cpp
Miyako came to the flea kingdom with a ukulele. She became good friends with 
local flea residents and played beautiful music for them every day.

In return, the fleas made a bigger ukulele for her: it has n strings, and each 
string has (1018+1) frets numerated from 0 to 1018. The fleas use the array s1,
s2,…,sn to describe the ukulele's tuning, that is, the pitch of the j-th fret 
on the i-th string is the integer si+j.

Miyako is about to leave the kingdom, but the fleas hope that Miyako will 
answer some last questions for them.

Each question is in the form of: "How many different pitches are there, if we 
consider frets between l and r (inclusive) on all strings?"

Miyako is about to visit the cricket kingdom and has no time to answer all the 
questions. Please help her with this task!

Formally, you are given a matrix with n rows and (1018+1) columns, where the 
cell in the i-th row and j-th column (0≤j≤1018) contains the integer si+j. You 
are to answer q queries, in the k-th query you have to answer the number of 
distinct integers in the matrix from the lk-th to the rk-th columns, inclusive.
```

## 【Input】

```cpp
The first line contains an integer n (1≤n≤100000) — the number of strings.

The second line contains n integers s1,s2,…,sn (0≤si≤1018) — the tuning of the 
ukulele.

The third line contains an integer q (1≤q≤100000) — the number of questions.

The k-th among the following q lines contains two integers lk，rk (0≤lk≤rk≤1018)
— a question from the fleas.
```

## 【Output】

```cpp
Output one number for each question, separated by spaces — the number of 
different pitches.
```

------



## 【Examples】 

#### Sample Input

```cpp
6
3 1 4 1 5 9
3
7 7
0 2
8 17
```

#### Sample Output

```cpp
5 10 18
```
#### Sample Input

```cpp
2
1 500000000000000000
2
1000000000000000000 1000000000000000000
0 1000000000000000000
```

#### Sample Output

```cpp
2 1500000000000000000
```
------



## 【Problem Description】

```cpp
给你n个数，第i个数表示第i行的起始数字为a[i],定义第i行的第j个数，就是a[i]+j
q个询问，每个询问问n行，[l,r]列区间内有多少个不同的数字。

附样例一：
7~7： 8,10,11,12,16.
```
$$
\begin{matrix} \textbf{Fret} & \textbf{0} & \textbf{1} & \textbf{2} & \textbf{3} & \textbf{4} & \textbf{5} & \textbf{6} & \textbf{7} & \ldots \\ s_1: & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & \dots \\ s_2: & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & \dots \\ s_3: & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & \dots \\ s_4: & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & \dots \\ s_5: & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & \dots \\ s_6: & 9 & 10 & 11 & 12 & 13 & 14 & 15 & 16 & \dots \end{matrix}
$$
## 【Solution】

```cpp
首先对n个数从小到大排序，对样例一排序后为：1 1 3 4 5 9

其次要知道，查询区间[l,r]与查询区间[0,r-l+1]是一样的
以下的r都为：r=r-l+1
考虑每一行对整个区间的最大贡献即r为无穷大时：
对于样例一举例来说：
    对于第一行的1来说，它对答案是没有贡献的，无论查询区间是什么，它所产生的数字，在第二
    行一定已经产生过了
    对于第二行的1来说，它对答案的最大贡献为2，即可以产生其他行无法产生的1和2两个数字。
    对于第三行的3来说，他对答案的最大贡献为1，即可以产生其他行无法产生的3数字。
    ……
    ……
    类似的可以得到第i行对答案的最大贡献就是a[i+1]-a[i],第n行对答案的最大贡献为r-a[n]
    所以答案就是所有行的贡献之和

    那如果任意[0,r]区间时，每行的贡献是多少呢？
    第i行的贡献就是min(r,a[i+1]-a[i])，意义：最多能产生的数字就是min(r,a[i+1]-a[i])

    有q个查询，每次都遍历n行，时间复杂度为O(n*q)肯定不行

    再看：现在知道[1,n-1]行的最大贡献为 0 2 1 1 4，对其排个序：0 1 1 2 4
    如果要查询区间为[0,r]，那么只要知道不大于r的最大位置t是什么不就好了。
    [1,t]的贡献都是小于r的，[t+1,n-1]都是大于r的
    所以答案就是sum[1,t]+((n-1)-t)*r+最后一行的贡献r,整理以下=sum[1,t]+(n-t)*r
    找t的位置可以用二分来查找，sum前缀和数组要预处理出来。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-04-06 22:35:16 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-04-07 13:35:25
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int a[maxn],b[maxn],s[maxn];
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    sort(a+1,a+n+1);
    for(int i=2;i<=n;i++){
        b[i-1]=a[i]-a[i-1]; //每行对答案的最大贡献
    }
    sort(b+1,b+n); //从小到大排序
    for(int i=1;i<n;i++) s[i]=s[i-1]+b[i]; //预处理前缀和
    int q;cin>>q;
    while(q--){
        int x,y;cin>>x>>y;y=y-x+1;
        int t=upper_bound(b+1,b+n,y)-b-1; //二分查找不大于y的最大位置t
        cout<<s[t]+(n-t)*y<<' ';
    }
    cout<<endl;
    cin.get(),cin.get();
    return 0;
}
```
