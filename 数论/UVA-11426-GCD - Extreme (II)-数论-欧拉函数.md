#  [UVA-11426-GCD - Extreme (II)-数论-欧拉函数](https://vjudge.net/problem/UVA-11426)



## 【Description】

> ```
> Given the value of N, you will have to ﬁnd the value of G. The deﬁnition of G is
> given below:
> ```
>
> $$
> G=\sum_{i=1}^{n-1}\sum_{j=i+1}^NGCD(i,j)
> $$
>
> ```
> Here GCD(i, j) means the greatest common divisor of integer i and integer j.
> For those who have trouble understanding summation notation, the meaning of G is
> given in the following code:
> G=0;
> for(i=1;i<N;i++)
>     for(j=i+1;j<=N;j++)
>     {
>         G+=gcd(i,j);
>     }
> /*Here gcd() is a function that findsthe greatest common divisor of the two input
> numbers*/
> ```

## 【Input】

> ```
> The input ﬁle contains at most 100 lines of inputs. Each line contains an integer N
> (1 < N < 4000001).
> The meaning of N is given in the problem statement. Input is terminated by a line 
> containing a single zero.
> ```

## 【Output】

> ```
> For each line of input produce one line of output. This line contains the value of
> G for the corresponding
> N. The value of G will ﬁt in a 64-bit signed integer.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 10
> 100
> 200000
> 0
> ```

> ###Sample Output

> ```
> 67
> 13015
> 143295493160
> ```

------



## 【Problem Description】

> ```
> 题意就一个式子。不用多说。
> ```

## 【Solution】

> ```
> 数论。欧拉函数。
> 对于题目给的式子不太好求，我们将其转换一下。转换为以下形式：
> ```
>
> $$
> G=\sum_{i=2}^{N}\sum_{j=1}^{i-1}GCD(j,i)
> $$
>
> ```
> 可以将两个式子分解开来对比，两个是等价的。
> G=sum[2]+sum[3]+...+sum[N].
> sum[i]=gcd(1,i)+gcd(2,i)+...+gcd(i-1,i).
> 我们设gcd(j,i)=x,我们假设所有满足条件的j中(1<=j<i),gcd(j,i)=x的个数有y个(1<=x<=i)。
> 则sum[i]=x1*y1+x2*y2+...+xn*yn.
> 对于每个x，x*y=x*Phi[i/x],
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-03 15:27:18 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-03 21:00:51
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 4000005
int prime[maxn];
int Ph[maxn];
int sum[maxn];
void getprime()//欧拉筛
{
    for(int i=2;i<=maxn;i++)
    {
        if(!prime[i])
        {
            prime[++prime[0]]=i;
            Ph[i]=i-1;
        }
        for(int j=1;j<=prime[0]&&i*prime[j]<=maxn;j++)
        {
            prime[i*prime[j]]=1;
            if(i%prime[j]==0)
            {
                Ph[i*prime[j]]=Ph[i]*prime[j];
                break;
            }
            else Ph[i*prime[j]]=(prime[j]-1)*Ph[i];
        }
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    getprime();
    for(int i=1;i<=maxn;i++)
    {
        for(int j=i+i;j<=maxn;j+=i)
        {
            sum[j]+=i*Ph[j/i];//求sum[i]
        }
    }
    for(int i=1;i<=maxn;i++)
    {
        sum[i]+=sum[i-1];
    }
    int n;
    while(cin>>n&&n)
    {
        cout<<sum[n]<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
