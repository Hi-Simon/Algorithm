#  [HDU-2588-GCD-数论-欧拉函数+思维](https://vjudge.net/problem/HDU-2588)



## 【Description】

```c++
The greatest common divisor GCD(a,b) of two positive integers a and b,sometimes written 
(a,b),is the largest divisor common to a and b,For example,(1,2)=1,(12,18)=6.
(a,b) can be easily found by the Euclidean algorithm. Now Carp is considering a little 
more difficult problem:
Given integers N and M, how many integer X satisfies 1<=X<=N and (X,N)>=M. 
```

## 【Input】

```c++
The first line of input is an integer T(T<=100) representing the number of test cases. 
The following T lines each contains two numbers N and M (2<=N<=1000000000, 1<=M<=N), 
representing a test case.
```

## 【Output】

```c++
For each test case,output the answer on a single line.
```

------



## 【Examples】 

#### Sample Input

```c++
3
1 1
10 2
10000 72
```

#### Sample Output

```c++
1
6
260
```

------



## 【Problem Description】

```c++
给你n,m。求gcd(x,n)>=m的x的个数。其中1<=x<=n
```

## 【Solution】

```c++
找到所有x>=m，且n%x==0.
那么答案就是所有满足条件的phi(n/x)的和。
原因:
令y=n/x;phi(n/x)=phi(y),就是小于y且与y互质的数的个数。
假设小于y，且与y互质的数为p1,p2,,,pk
因为对于x，gcd(x,n)==x>=m,所以gcd(x*pk,n)==x>=m.
```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-02 17:07:22 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-02 20:32:10
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
const int maxn=1e9+5;
int Phi(int n) //欧拉函数
{
    long long ans=n;
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
    int t; cin>>t;
    while(t--)
    {
        int n,m,ans=0;
        cin>>n>>m;
        for(int i=1;i*i<=n;i++)
        {
            if(n%i==0)
            {
                if(i>=m) ans+=Phi(n/i); //gcd(i,n)=i>=m
                if(n/i>=m&&n/i!=i) ans+=Phi(i);//gcd(n/i,n)=n/i>=m
            }
        }
        cout<<ans<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
