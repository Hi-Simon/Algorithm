#  [LightOJ-1336-Sigma Function-数论-思维](https://vjudge.net/problem/LightOJ-1336)



## 【Description】

> ```
> Sigma function is an interesting function in Number Theory. It is denoted by the 
> Greek letter Sigma (σ). This function actually denotes the sum of all divisors of a
> number. For example σ(24) = 1+2+3+4+6+8+12+24=60. Sigma of small numbers is easy to 
> find but for large numbers it is very difficult to find in a straight forward way. 
> But mathematicians have discovered a formula to find sigma. If the prime power 
> decomposition of an integer is
> ```
>
> $$
> n=p_1^{e_1}*p_2^{e_2}*\dots*p_k^{e_k}\\
> $$
>
> ```
> Then we can write,
> ```
>
> $$
> \sigma(n)=\frac{p_1^{e_1+1}-1}{p_1-1}*\frac{p_2^{e_2+1}-1}{p_2-1}*\dots*\frac{p_k^{e_k+1}-1}{p_k-1}
> $$
>
> ```
> For some n the value of σ(n) is odd and for others it is even. Given a value n, you 
> will have to find how many integers from 1 to n have even value of σ.
> ```
>
>

## 【Input】

> ```
> Input starts with an integer T (≤ 100), denoting the number of test cases.
>     
> Each case starts with a line containing an integer n (1 ≤ n ≤ 10^12).
> ```

## 【Output】

> ```
>  For each case, print the case number and the result.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 4
> 3
> 10
> 100
> 1000
> ```

> ###Sample Output

> ```
> Case 1: 1
> Case 2: 5
> Case 3: 83
> Case 4: 947
> ```

------



## 【Problem Description】

> ```
> σ(n)为n的所有因子的和，给你一个n，问[1,n]中，σ(i)为偶数的个数有多少个。
> ```

## 【Solution】

> ```
> 数论
> 将题目给的公式看懂就比较简单了吧。
> 题目给的公式的每一项起始就是一个等比数列的和。即：
> ```
>
> $$
> \sigma(n)=(1+p_1+p_1^2+\dots+p_{1}^{e_1})*(1+p_2+p_2^2+\dots+p_2^{e2})*\dots*(1+p_k+p_k^2+\dots+p_k^{ek})
> $$
>
> ```
> 则偶数的个数=n-奇数的个数
> 奇数的个数=小于n的平方数的个数。
> 
> 1、要想σ(n)为奇数，则必须保证乘法的每一项都为奇数。
> 2、要保证每一项都为奇数，则必须保证(pi+pi^2+...+pi^ei)有偶数项，即ei为偶数。
> 3、特殊情况：pi为2时，ei无论何值，此项之和都为奇数。
> 
> 对于每个ei都为偶数，则n必为平方数。
> 平方数*2<n,则个数加1。
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-06 13:37:22 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-06 13:43:24
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int a[maxn];
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin>>t;
    for(int ca=1;ca<=t;ca++)
    {
        cout<<"Case "<<ca<<": ";
        int n;
        cin>>n;
        int sum=0;
        int i=1;
        while(i*i<=n)
        {
            sum++;
            if(i*i*2<=n) sum++;
            i++;
        }
        cout<<n-sum<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
