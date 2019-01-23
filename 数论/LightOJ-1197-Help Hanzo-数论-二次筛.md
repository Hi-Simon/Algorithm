#  [LightOJ-1197-Help Hanzo-数论-二次筛](https://vjudge.net/problem/LightOJ-1197)



## 【Description】

> ```
> Amakusa, the evil spiritual leader has captured the beautiful princess Nakururu. 
> The reason behind this is he had a little problem with Hanzo Hattori, the best 
> ninja and the love of Nakururu. After hearing the news Hanzo got extremely angry. 
> But he is clever and smart, so, he kept himself cool and made a plan to face 
> Amakusa.
> 
> Before reaching Amakusa's castle, Hanzo has to pass some territories. The 
> territories are numbered as a, a+1, a+2, a+3 ... b. But not all the territories are
> safe for Hanzo because there can be other fighters waiting for him. Actually he is 
> not afraid of them, but as he is facing Amakusa, he has to save his stamina as much 
> as possible.
> 
> He calculated that the territories which are primes are safe for him. Now given a 
> and b he needs to know how many territories are safe for him. But he is busy with 
> other plans, so he hired you to solve this small problem!
> ```

## 【Input】

> ```
> Input starts with an integer T (≤ 200), denoting the number of test cases.
> 
> Each case contains a line containing two integers a and b (1 ≤ a ≤ b < 2^31, b - a 
> ≤ 100000).
> 
> ```

## 【Output】

> ```
>  For each case, print the case number and the number of safe territories.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 3
> 2 36
> 3 73
> 3 11
> ```

> ###Sample Output

> ```
> Case 1: 11
> Case 2: 20
> Case 3: 4
> ```

------



## 【Problem Description】

> ```
> 求[a,b]内素数的个数
> ```

## 【Solution】

> ```
> 数论，二次筛
> 先将sqrt(b)内的素数筛选出来，然后用这部分素数去筛[a,b]内的素数。
> 可行的原因就在于b-a<=1e5
> 最后注意一下，如果a==1，答案要减1，因为1不是素数。
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-12 13:06:06 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-12 14:46:51
 */
#include <bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int prime[maxn];
bool vis[maxn]={1,1};
int cnt;
void get_prime()
{
    for (int i = 2; i <= maxn; i++)
    {
        if (!vis[i]) prime[++cnt] = i;
        for (int j = 1; j <= cnt && i * prime[j] <= maxn; j++)
        {
            vis[i * prime[j]] = 1;
            if (i % prime[j] == 0) break;
        }
    }
}
int solve(int a, int b)
{
    int vis[maxn] = {0}, ans = 0;
    for (int i = 1; prime[i] * prime[i] <= b; i++)
    {
        int t = (a + prime[i] - 1) / prime[i];
        if (t < 2) t = 2;
        while (t * prime[i] <= b)
        {
            vis[t*prime[i] - a] = 1;
            t++;
        }
    }
    for (int i = 0; i <= b - a; i++) ans += !vis[i];
    return ans;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    get_prime();
    int t;
    cin >> t;
    for (int ca = 1; ca <= t; ca++)
    {
        cout << "Case " << ca << ": ";
        int a, b; cin >> a >> b;
        int ans = solve(a, b);
        cout << ans-(a==1?1:0) << endl;
    }
    cin.get(), cin.get();
    return 0;
}
```
