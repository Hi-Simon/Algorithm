#  [HDU-3501-Calculation 2-数论-小于n且与n互质的数的和](https://vjudge.net/problem/HDU-3501)



## 【Description】

> ```
> Given a positive integer N, your task is to calculate the sum of the positive 
> integers less than N which are not coprime to N. A is said to be coprime to B if A,
> B share no common positive divisors except 1. 
> ```

## 【Input】

> ```
> For each test case, there is a line containing a positive integer N(1 ≤ N ≤ 
> 1000000000). A line containing a single 0 follows the last test case.
> ```

## 【Output】

> ```
>  For each test case, you should print the sum module 1000000007 in a line.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 3
> 4
> 0
> ```

> ###Sample Output

> ```
> 0
> 2
> ```

------



## 【Problem Description】

> ```
> 求小于n且与n不互质的数的和
> ```

## 【Solution】

> ```
> 公式Ph[n]*n/2为小于n且与n互质的数的和，用前n个数的和减去它即可。
> 有gcd(n,i)==1，则gcd(n,n-i)==1,所以在小于n且与n互质的所有数中，必能两两配对，且两数之和为n
> 则可得其公式为Ph[n]*n/2.
> 
> 证：gcd(n,i)==1,则gcd(n,n-i)==1
> 反证法：若gcd(n,n-i)==k>1,则(n-i)%k==0且n%k==0,则n%k-i%k==0-->i%k==0,又因为gcd(n,i)==1
> 与n%k==0,i%k==0,矛盾，所以得证k==1.
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-08 18:49:48 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-08 19:07:07
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
const int mod=1e9+7;
int Ph(int n)//求单个欧拉函数
{
    int ans=n;
    for(int i=2;i*i<=n;i++)
    {
        if(n%i==0)
        {
            ans-=ans/i;
            while(n%i==0) n/=i;
        }
    }
    if(n>1) ans-=ans/n;
    return ans;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    while(cin>>n&&n)
    {
        int ans=n*(1+n)/2-n;//等差数列求和公式
        cout<<(ans%mod-(Ph(n)*n/2)%mod+mod)%mod<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
