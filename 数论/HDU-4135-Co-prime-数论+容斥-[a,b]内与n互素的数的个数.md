#  [HDU-4135-Co-prime-数论+容斥-[a,b]内与n互素的数的个数](https://vjudge.net/problem/HDU-4135)



## 【Description】

> ```
> Given a number N, you are asked to count the number of integers between A and B 
> inclusive which are relatively prime to N.
> Two integers are said to be co-prime or relatively prime if they have no common 
> positive divisors other than 1 or, equivalently, if their greatest common divisor 
> is 1. The number 1 is relatively prime to every integer. 
> ```

## 【Input】

> ```
> The first line on input contains T (0 < T <= 100) the number of test cases, each of 
> the next T lines contains three integers A, B, N where (1 <= A <= B <= 10 15) and 
> (1 <=N <= 10 9).
> ```

## 【Output】

> ```
> For each test case, print the number of integers between A and B inclusive which 
> are relatively prime to N. Follow the output format below.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 2
> 1 10 2
> 3 15 5
> ```

> ###Sample Output

> ```
> Case #1: 5
> Case #2: 10
> ```

------



## 【Problem Description】

> ```
> 求[a,b]内与n互质的数的个数
> ```

## 【Solution】

> ```
> 数论，容斥
> 容斥原理：
>     要计算几个集合并集的大小，我们要先将所有单个集合的大小计算出来，然后减去所有两个集合相交的部
> 分，再加回所有三个集合相交的部分，再减去所有四个集合相交的部分，依此类推，一直计算到所有集合相交的部
> 分。
> 
> 记f(a,b)表示[a,b]内与n不互质的数的个数
> 则求[a,b]内与n互质的个数可以转化为：[a,b]的总个数-f(a,b)
> f(a,b)=f(1,b)-f(1,a-1)
> 最终问题就转化为求[1,x]内与n不互质的数的个数
> 
> 将n的质因数分解出来，即p1,p2,p3,...,pm
> 那么可以知道[1,x]内与pi不互质的数的集合为qi(集合内元素个数|qi|=x/pi),我们要求的就q1,q2,...,qm
> 集合的并集，那么根据容斥原理即可得到最终答案。
> 
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-10 16:55:18 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-10 17:52:35
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
vector<int>fac;//质因数
void get_fac(int n)//得到n的质因数
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
int solve(int r)//容斥,求r以内与n不互质的数的个数
{
    int sum=0;
    for(int i=1;i<(1<<fac.size());i++)//枚举质因数的每一种组合
    {
        int ans=1,num=0;
        for(int j=0;j<fac.size();j++)//求当前组和的积
        {
            if(i&(1<<j))
            {
                ans *= fac[j];
                num++;
            }
        }
        if(num&1) sum+=r/ans;//如果当前组合个数为奇数个，加上r以内能被ans整除的数的个数
        else sum-=r/ans;//否则减去r以内能被ans整除的数的个数
    }
    return sum;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin>>t;
    for(int ca=1;ca<=t;ca++)
    {
        cout<<"Case #"<<ca<<": ";
        int A,B,N;
        cin>>A>>B>>N;
        fac.clear();
        get_fac(N);
        cout<<(B-A+1)-(solve(B)-solve(A-1))<<endl;//[A,B]的总个数，减去[A,B]内不与N互质的数的个数即答案
    }
    cin.get(),cin.get();
    return 0;
}
```
