#  [LightOJ-1341-Aladdin and the Flying Carpet-数论-唯一分解定理](https://vjudge.net/problem/LightOJ-1341)



## 【Description】

> ```
> It's said that Aladdin had to solve seven mysteries before getting the Magical Lamp
> which summons a powerful Genie. Here we are concerned about the first mystery.
> 
> Aladdin was about to enter to a magical cave, led by the evil sorcerer who 
> disguised himself as Aladdin's uncle, found a strange magical flying carpet at the 
> entrance. There were some strange creatures guarding the entrance of the cave. 
> Aladdin could run, but he knew that there was a high chance of getting caught. So, 
> he decided to use the magical flying carpet. The carpet was rectangular shaped, but 
> not square shaped. Aladdin took the carpet and with the help of it he passed the 
> entrance.
> 
> Now you are given the area of the carpet and the length of the minimum possible 
> side of the carpet, your task is to find how many types of carpets are possible. 
> For example, the area of the carpet 12, and the minimum possible side of the carpet
> is 2, then there can be two types of carpets and their sides are: {2, 6} and {3, 
> 4}.
> ```

## 【Input】

> ```
> Input starts with an integer T (≤ 4000), denoting the number of test cases.
> 
> Each case starts with a line containing two integers: a b (1 ≤ b ≤ a ≤ 10^12) where
> a denotes the area of the carpet and b denotes the minimum possible side of the 
> carpet.
> 
> ```

## 【Output】

> ```
>  For each case, print the case number and the number of possible carpets.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 2
> 10 2
> 12 2
> ```

> ###Sample Output

> ```
> Case 1: 1
> Case 2: 2
> ```

------



## 【Problem Description】

> ```
> 给你矩形的面积a，和一个b，b为矩形的边的最小长度。求xy的对数，满足x*y=a,且x>b,y>b.
> ```

## 【Solution】

> ```
> (唯一分解定理）对于每一个数a，都可以分解为以下形式，且唯一：
> ```
>
> $$
> a=a_1^{p1}\times a_2^{p2}\dots a_m^{pm}
> $$
>
> ```
> 其中，ai为素数，pi为正整数。
> 则a的正因数的个数为num =（1+p1)*(1+p2)*...*(1+pm)
> 所以满足x*y=a,x,y的对数为cnt=num/2;
> 但题目还要求x>b,y>b所以暴力枚举一下，将小于b的边长减去即可。
> 
> 最后还要注意特判a<b*b的情况，
> ```
>
>

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-05 20:09:09 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-05 21:41:11
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
const int maxn=1e6+10;
Int prime[maxn];
void get_prime()//线性筛素数
{
    for(Int i=2;i<=maxn;i++)
    {
        if(!prime[i])
        {
            prime[++prime[0]]=i;
        }
        for(Int j=1;j<=prime[0]&&i*prime[j]<=maxn;j++)
        {
            prime[i*prime[j]]=1;
            if(i%prime[j]==0) break;
        }
    }
}
int getfactor(int x)
{
    int ans=1;
    for(int i=1;prime[i]*prime[i]<=x&&i<=prime[0];i++)
    {
        if(x%prime[i]==0)
        {
            int sum = 0;
            while(x%prime[i]==0)
            {
                sum++;//pi
                x/=prime[i];
            }
            ans*=(1+sum);//正因数的个数
        }
    }
    if(x>1) ans*=2;
    return ans;
}
Int main()
{
    get_prime();
    Int t;
    int a, b;
    scanf("%d",&t);
    for(Int ca=1;ca<=t;ca++)
    {
        
        scanf("%lld%lld",&a,&b);
        if(a<b*b){ printf("Case %d: 0\n",ca); continue;}//特判
        int cnt=getfactor(a)/2;
        for(int i=1;i<b;i++)//减去小于b的边长
        {
            if(a%i==0)
            {
                cnt--;
            }
        }
        printf("Case %d: %lld\n",ca,cnt);
    }
    cin.get(),cin.get();
    return 0;
}
```
