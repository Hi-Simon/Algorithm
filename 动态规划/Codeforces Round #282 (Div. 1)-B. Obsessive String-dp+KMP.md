#  [Codeforces Round #282 (Div. 1)-B. Obsessive String-dp+KMP](http://codeforces.com/contest/494/problem/B)



## 【Description】

> ```
> Hamed has recently found a string t and suddenly became quite fond of it. He spent 
> several days trying to find all occurrences of t in other strings he had. Finally 
> he became tired and started thinking about the following problem. Given a string s
> how many ways are there to extract k ≥ 1 non-overlapping substrings from it such 
> that each of them contains string t as a substring? More formally, you need to 
> calculate the number of ways to choose two sequences a1, a2, ..., ak and b1, b2, ..., 
> bk satisfying the following requirements:
> ```
>
> $$
> k\ge1\\
> \forall i\,(1\le i \le k)\, 1\le a_i,b_i\le |s|\\
> \forall i\,(1\le i \le k)\, b_i\ge a_i\\
> \forall i\,(2\le i \le k)\, a_i\ge b{i-1}\\
> \forall i\,(1\le i \le k)\\, t\,is\, a\, substring\, of\, string\, s_{ai} \,s_{ai + 1}\,... s_{bi}\, (string\, s\, is\, considered\, as\, 1-indexed).
> $$
>

> ```
> As the number of ways can be rather large print it modulo 10^9 + 7.
> ```

## 【Input】

> ```
> Input consists of two lines containing strings s and t (1 ≤ |s|, |t| ≤ 105). Each string consists of lowercase Latin letters.
> ```

## 【Output】

> ```
>  Print the answer in a single line.
> ```

------



## 【Examples】 

> ### Sample Input

> ```c++
> ababa
> aba
> ```

> ###Sample Output

> ```c++
> 5
> ```

>   ###Sample Input

>   ```c++
>   welcometoroundtwohundredandeightytwo
>   d
>   ```

>   ### Sample Output

>   ```c++
>   274201
>   ```

>   ### Sample Input

>   ```c++
>   ddd
>   d
>   ```

>   ### Sample Output

>   ```c++
>   12
>   ```

------



## 【Problem Description】

> ```
> 问字符串s中有多少个字串其中存在字串t
> ```

## 【Solution】

> ```
> dp，KMP字符串匹配
> 
> 定义dp[i]为前i个位置满足条件的答案数。val[i]为最后一个完全匹配时的初始位置，比如
> s="abcde",t="bcd",则val[1]=val[2]=val[3]=0,val[4]=val[5]=2,sum[i]为dp数组的前缀和，
> 则状态转移方程为:
> dp[i]=dp[i-1]+(dp[0]+1)+(dp[1]+1)+(dp[2]+1)+(dp[3]+1)+...+(dp[val[i]-1]+1),
> 其中dp[0]=0,整理一下得dp[i]=dp[i-1]+dp[1]+dp[2]+dp[3]+...+dp[i-t.size()]+i-t.size()+1
>  			 			 =dp[i-1]+sum[val[i]-1]+val[i];
> val数组可以用kmp预处理出来。
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-31 19:24:53 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-31 21:11:57
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
#define mod 1000000009
int Next[maxn];//KMP next数组
int val[maxn];//完全匹配时的起始位置，比如ababa-aba,val[3]=1,val[5]=3
int dp[maxn];//前i个长度，满足条件的总数
int sum[maxn];//dp数组的前缀和
char s[maxn],t[maxn];
void getval()//KMP字符串匹配
{
    int j=1,k=0;
    int tsize=strlen(t+1),ssize=strlen(s+1);
    while(j<tsize)
    {
        if(k==0||t[j]==t[k])
        {
            k++;
            j++;
            Next[j]=k;
        }
        else k=Next[k];
    }
    int i=1;j=1;
    while(i<ssize)
    {
        if(j==tsize&&s[i]==t[j])
        {
            val[i]=i-tsize+1;//如果完全匹配，保存起始位置
            j=Next[j];
        }
        if(j==0||s[i]==t[j])
        {
            i++;
            j++;
        }
        else j=Next[j];
    }
    if (j == tsize && s[i] == t[j])
    {
        val[i] = i - tsize + 1;
        j = Next[j];
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin>>s+1>>t+1;
    getval();
    for(int i=1;i<=strlen(s+1);i++) if(!val[i]) val[i]=val[i-1];
    for(int i=1;i<=strlen(s+1);i++)
    {
        dp[i]=dp[i-1]%mod;
        if(val[i]) dp[i]=(dp[i]+sum[val[i]-1]+val[i])%mod;//从第一个完全匹配开始才有计数
        sum[i]=(sum[i-1]+dp[i])%mod;//求dp数组前缀和
    }
    cout<<dp[strlen(s+1)]%mod<<endl;
    cin.get(),cin.get();
    return 0;
}
```
