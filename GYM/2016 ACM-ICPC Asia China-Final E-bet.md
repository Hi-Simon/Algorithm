# [ 2016 ACM-ICPC Asia China-Final E-bet](http://codeforces.com/gym/101194/problem/E)

[TOC]



## ``Description ``

> The Codejamon game is on re! Fans across the world are predicting and betting on which team
> will win the game.
>
> A gambling company is providing betting odds for all teams; the odds for the ith team is Ai:Bi.
> For each team, you can bet any positive amount of money, and you do not have to bet the same
> amount on each team. If the **ith** team wins, you get your bet on that team back, plus **Bi/Ai** times
> your bet on that team.
>
> For example, suppose that there are two teams, with odds of 5:3 and 2:7 and you bet $20 on the
> first team and $10 on the second team. If the first team wins, you will lose your \$10 bet on the
> second team, but you will receive your $20 bet back, plus **3/5* 20 = 12**, so you will have a total
> of $32 at the end. If the second team wins, you will lose your \$20 bet on the rst team, but you
> will receive your $10 bet back, plus **7/2 *10 = 35**, so you will have a total of \$45 at the end. Either
> way, you will have more money than you bet ($20+$10=$30).
>
> As a greedy fan, you want to bet on as many teams as possible to make sure that as long as one
> of them wins, you will always end up with **more** money than you bet. Can you figure out how
> many teams you can bet on?

## `Input`

> The input starts with one line containing exactly one integer T, which is the number of test cases.
> Each test case starts with one line containing an integer N: the number of teams in the game.
> Then, N more lines follow. Each line is a pair of numbers in the form **Ai:Bi** (that is, a number
> Ai, followed by a colon, then a number Bi, with no spaces in between), indicating the odds for
> the **ith** team.

## `Output`

> For each test case, output one line containing \"Case #x: y", where x is the test case number
> (starting from 1) and y is the maximum number of teams that you can bet on, under the conditions
> specied in the problem statement.

## `Examples` 

> ### `Input`

> ```
> 1
> 3
> 1:1.1
> 1:0.2
> 1.5:1.7
> ```

> ###Ouput

> ```
> Case #1: 2
> ```

## `Problem description`

> 有n个队伍参加比赛，给你赔率比，输出你能投入钱的最大队伍的个数。在所有你投入的队伍中，要保证只要任何一支队伍赢，所获得的金钱大于你所投入的总金钱。（各个队伍之间独立）

## `Solution`

> $$
> 设pi为投入第i个队伍的金钱占总金钱的比例
> $$
>
> $$
> 则pi*（1+Bi/Ai）>1
> $$
>
> $$
> 即pi>Ai/(Ai+Bi)
> $$
>
> 则可按照
> $$
> Ai/(Ai+Bi)
> $$
> 从小到大排序，只要总和小于1，则从小到大一直选择。

## `Code`

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-13 21:49:19 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-13 21:58:37
 */
#include <bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
long double a[maxn];
const int esp = 1000;
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin >> t;
    for (int ca = 1; ca <= t; ca++)
    {
        cout << "Case #" << ca << ": ";
        int n;
        cin >> n;
        for (int i = 1; i <= n; i++)
        {
            long double A, B;
            char c;
            cin >> A >> c >> B;
            int t1 = (int)(A * esp + 0.01);
            int t2 = (int)(B * esp + 0.01);
            a[i] = 1.0 * A / (long double)(A + B);
        }
        sort(a + 1, a + n + 1);
        long double sum = 0;
        int ans = 0;
        for (int i = 1; i <= n; i++)
        {
            if (sum + a[i] >= 1)
                break;
            ans++;
            sum += a[i];
        }
        cout << ans << endl;
    }
    cin.get(), cin.get();
    return 0;
}
```

