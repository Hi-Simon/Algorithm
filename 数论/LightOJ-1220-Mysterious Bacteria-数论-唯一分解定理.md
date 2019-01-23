#  [LightOJ-1220-Mysterious Bacteria-数论-唯一分解定理](https://vjudge.net/problem/LightOJ-1220)



## 【Description】

> ```
> Dr. Mob has just discovered a Deathly Bacteria. He named it RC-01. RC-01 has a very 
> strange reproduction system. RC-01 lives exactly x days. Now RC-01 produces exactly 
> p new deadly Bacteria where x = bp (where b, p are integers). More generally, x is 
> a perfect pth power. Given the lifetime x of a mother RC-01 you are to determine 
> the maximum number of new RC-01 which can be produced by the mother RC-01.
> ```

## 【Input】

> ```
> Input starts with an integer T (≤ 50), denoting the number of test cases.
> 
> Each case starts with a line containing an integer x. You can assume that x will 
> have magnitude at least 2 and be within the range of a 32 bit signed integer.
> 
> ```

## 【Output】

> ```
> For each case, print the case number and the largest integer p such that x is a 
> perfect pth power.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 3
> 17
> 1073741824
> 25
> ```

> ###Sample Output

> ```
> Case 1: 1
> Case 2: 30
> Case 3: 2
> ```

------



## 【Problem Description】

> ```
> 给你一个数n，求最大的p使得n满足n=b^p;
> 例64=2^6=4^3=8^2,但最大的为6，所以ans=6.
> 注意n可以为负数w(ﾟДﾟ)w
> ```

## 【Solution】

> ```
> 数论，唯一分解定理。
> 对于每一个n都可以分解为：
> ```
>
> $$
> n=p_1^{e_1}p_2^{e_2}\dots p_m^{e_m}
> $$
>
> ```
> 我们只关心e1,e2,...,em.
> 将ei去重后的个数为k。
> 如果n大于0：
> 	答案就是所有ei的最大公约数
> 如果n小于0：
> 	如果k==1，就是将e1一直除2，直到其为奇数。
> 	否则答案就是1.
> （数据好像特别水？4ms过
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-11 18:37:46 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-11 19:39:19
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
set<int>s;
void get_fac(int n)
{
    for(int i=2;i*i<=n;i++)
    {
        int ans=0;
        if(n%i==0) 
        {
            while(n%i==0) n/=i,ans++;
            s.insert(ans);
        }
    }
    if(n>1) s.insert(1);
}
int gcd(int a,int b)
{
    return b==0?a:gcd(b,a%b);
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
        int n;
        cin>>n;
        s.clear();
        get_fac(abs(n));
        if(s.size()==1)
        {
            int ans=*s.begin();
            if(n<0)
            {
                while(ans%2==0) ans/=2;
                cout<<ans<<endl;
            }
            else cout<<ans<<endl;
        }
        else
        {
            int ans=*s.begin();
            set<int>::iterator it;
            for(it=s.begin();it!=s.end();it++) ans=gcd(ans,*it);
            if(n<0) cout<<1<<endl;
            else cout<<ans<<endl;
        }
    }
    cin.get(),cin.get();
    return 0;
}
```
