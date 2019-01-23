#  [LightOJ-1282-Leading and Trailing-快速幂-深入理解整数](https://vjudge.net/problem/LightOJ-1282)



## 【Description】

> ```
> You are given two integers: n and k, your task is to find the most significant 
> three digits, and least significant three digits of n^k.
> ```

## 【Input】

> ```
> Input starts with an integer T (≤ 1000), denoting the number of test cases.
> 
> Each case starts with a line containing two integers: n (2 ≤ n < 2^31) and k (1 ≤ k 
> ≤ 10^7).
> 
> ```

## 【Output】

> ```
> For each case, print the case number and the three leading digits (most significant
> ) and three trailing digits (least significant). You can assume that the input is 
> given such that n^k contains at least six digits.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 5
> 123456 1
> 123456 2
> 2 31
> 2 32
> 29 8751919
> ```

> ###Sample Output

> ```
> Case 1: 123 456
> Case 2: 152 936
> Case 3: 214 648
> Case 4: 429 296
> Case 5: 665 669
> ```

------



## 【Problem Description】

> ```
> 求n^k的前三位数字，和最后三位数字。
> ```

## 【Solution】

> ```
> 最后三位数字简单，快速幂对1000取模就好。
> 前三位：先将n^k转换为科学计数法的方式，再取前三位即可。
> ```
>
> $$
> 任意一个整数m都可以表示为0.p_1p_2\dots p_n\times10^n\\
> 所以，任意一个整数m，对其取对数x=log_{10}(m),则x的整数部分就等于n.\\
> 而10^{x-n}=0.p_1p_2\dots p_n,所以答案就是p_1p_2p_3
> $$
>
> ```
> 注意特判末尾三位小于100的情况，要补0.
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-06 14:45:36 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-06 15:02:36
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
#define mod 1000
int a[maxn];
int fpow(int a,int b)//快速幂
{
    int ans=1,x=a%mod;
    while(b)
    {
        if(b&1) ans*=x,ans%=mod;
        x*=x;
        x%=mod;
        b>>=1;
    }
    return ans;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin>>t;
    for(int ca=1;ca<=t;ca++)
    {
        cout<<"Case "<<ca<<": ";
        int n,k;
        cin>>n>>k;
        long double t=log10(n)*k;//log10(n^k)
        long double x=fmod(t,1);//t的小数部分
        x=pow(10,x);
        while(x<100) x*=10;//取前三位
        cout<<(int)x<<' '<<setw(3)<<setfill('0')<<fpow(n,k)<<endl;//注意补0
    }
    cin.get(),cin.get();
    return 0;
}
```
