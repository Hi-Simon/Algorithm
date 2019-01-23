#  [2018-ACM-ICPC-沈阳赛区网络赛-G. Spare Tire-容斥-[1,n]与m互质的数的平方和](https://nanti.jisuanke.com/t/31448)



## 【Description】

> ```
> A sequence of integer {an}\lbrace a_n \rbrace{an} can be expressed as:
> ```
>
> $$
> a_n=\Bigg \{ \begin{array}{ll}
> 0,&n=0\\
> 2,&n=1\\
> \frac {3*a_{n-1}-a_{n-2}}2+n+1,&n>1
> 
> \end{array} 
> $$
>
> ```
> Now there are two integers nnn and mmm. I'm a pretty girl. I want to find all 
> b1,b2,b3⋯bpb_1,b_2,b_3\cdots b_pb1,b2,b3⋯bp that 1≤bi≤n1\leq b_i \leq n1≤bi≤n
> and bib_ibi is relatively-prime with the integer mmm. And then calculate:
> ```
>
> $$
> \sum_{i=1}^pa_{b_i}
> $$
>
> ```
> But I have no time to solve this problem because I am going to date my boyfriend
> soon. So can you help me?
> ```

## 【Input】

> ```
> Input contains multiple test cases ( about 150001500015000 ). Each case contains 
> two integers nnn and mmm.1≤n,m≤10^8.
> ```

## 【Output】

> ```
>  For each test case, print the answer of my question(after mod 1,000,000,007).
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 4 4
> ```

> ###Sample Output

> ```
> 14
> ```

------



## 【Problem Description】

> ```
> 给你an的递推式。设[1,n]内与m互质的数为p1,p2,...,pk,求
> ```
>
> $$
> a_{p_1}+a_{p_2}+\dots+a_{p_k}
> $$
>
>

## 【Solution】

> ```
> 根据递推式求an通项公式：
> ```
>

> $$
> 令b_n=a_n-a_{n-1}\\
> 则2b_n=b_{n-1}+2n+2\\
> 两边同时减4n得：2(b_n-2n)=b_{n-1}-(2n-2)\\
> \frac{b_n-2n}{b_{n-1}-2(n-1)}=\frac{1}2\: (n\ge2) \: b_1-2=a_1-a_0-2=0\\
> 发现b_n-2n=0,b_n=2n,即a_n-a_{n-1}=2n\\
> a_2-a_1=2\times2\\
> a_3-a_2=2\times3\\
> a_4-a_3=2\times4\\
> \dots\
> a_n-a{n-1}=2\times n\\
> 上式全部相加得：a_n-a_1=2\times(2+3+\dots+n)\\
> a_n=2\times(1+2+3+\dots+n)=n\times(n+1)
> $$

> ```
> 则题目要 求的为:
> ```
>
> $$
> p_1^2+p_2^2+\dots+p_k^2+p_1+p_2+\dots+p_k\\
> $$
>
> ```
> 就是[1,n]内与m互质的数的平方和+其和。
> 转换一下，转换为[1,n]的和+平方和-[1,n]中与m不互质的数的平方和+其和。
> 然后再利用容斥原理即可求得答案，不会的先做一下下面这道题。题解：
> ```
>
> [HDU-4135-容斥求[a,b]内与n互质的数的个数](https://blog.csdn.net/Kente_K/article/details/82594149)
>
> ```
> 如果懂了，下面就简单了。
> 对于m的每个质因子q1,q2,q3,...,qt，假设n=12,m=12，则质因子为2，3.
> 则n以内能被2整除的数为：2，4，6，8，10，12。
> 其平方和:
> ```
>
> $$
> 2^2+4^2+6^2+8^2+10^2+12^2\\
> =2(1^2+2^2+3^2+4^2+5^2+6^2)\\
> 就等于 2\times 前12/2项的平方和。 利用前n项平方和公式即可求出来。
> $$
>
> ```
> 对于每一个的m的质因数，都可以利用上面的方法将n以内能被qi整除的数的平方和求出来，最后利用容斥原理的
> 奇加偶减求出[1,n]内不与m互质的数的平方和。
> 他们的和不用多说，在求平方和的时候顺便求一下就好。
> 
> 最后别忘了取模，以及除法取模的特殊性。
> ```
>

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-10 19:22:24 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-10 21:11:01
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
const int mod=1e9+7;
vector<int>fac;
void get_fac(int n)//求质因数
{
    for(int i=2;i*i<=n;i++)
    {
        if(n%i==0)
        {
            fac.push_back(i);
            while(n%i==0) n/=i;
        }
    }
    if(n>1) fac.push_back(n);
}
int cal(int n,int ans)//求前n项平方和 和 前n项和，  ans为质因数。
{
    ans%=mod;
    int t1=n,t2=n+1,t3=2*n+1;
    if(t1%2==0) t1/=2;
    else t2/=2;
    if(t1%3==0) t1/=3;
    else if(t2%3==0) t2/=3;
    else t3/=3;

    int ans1=(ans*ans)%mod;
    ans1=(ans1*t1)%mod;ans1=(((ans1*t2)%mod)*t3)%mod;
    t1=n,t2=n+1;
    if(t1%2==0) t1/=2;
    else t2/=2;
    int ans2=(ans*t2)%mod;ans2=(ans2*t1)%mod;
    return (ans1+ans2)%mod;
}
int solve(int r)//容斥
{
    int sum=0;
    for(int i=1;i<(1<<fac.size());i++)
    {
        int ans=1,num=0;
        for(int j=0;j<fac.size();j++)
        {
            if(i&(1<<j))
            {
                ans=(ans*fac[j])%mod;
                num++;
            }
        }
        if(num&1) sum=(sum+cal(r/ans,ans))%mod;
        else sum=(sum-cal(r/ans,ans)+mod)%mod;
    }
    return sum%mod;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m;
    while(cin>>n>>m)
    {
        fac.clear();
        get_fac(m);
        int sum=cal(n,1);
        cout<<(sum-solve(n)+mod)%mod<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
