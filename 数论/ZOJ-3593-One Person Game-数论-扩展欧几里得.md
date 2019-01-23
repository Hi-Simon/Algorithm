#  [ZOJ-3593-One Person Game-数论-扩展欧几里得](https://vjudge.net/problem/ZOJ-3593)



## 【Description】

> ```
> There is an interesting and simple one person game. Suppose there is a number axis 
> under your feet. You are at point A at first and your aim is point B. There are 6 
> kinds of operations you can perform in one step. That is to go left or right by a,b 
> and c, here c always equals to a+b.
> 
> You must arrive B as soon as possible. Please calculate the minimum number of 
> steps. 
> ```

## 【Input】

> ```
> There are multiple test cases. The first line of input is an integer T(0 < T ≤ 
> 1000) indicates the number of test cases. Then T test cases follow. Each test case
> is represented by a line containing four integers 4 integers A, B, a and b, 
> separated by spaces. (-2^31 ≤ A, B < 2^31, 0 < a, b < 2^31) 
> ```

## 【Output】

> ```
> For each test case, output the minimum number of steps. If it's impossible to reach 
> point B, output "-1" instead. 
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 2
> 0 1 1 2
> 0 1 2 4
> ```

> ###Sample Output

> ```
> 1
> -1
> ```

------



## 【Problem Description】

> ```
> 在一维坐标系内，从A走到B所需要的最少步数，每次可以向任意方向走a,b,c步，其中c=a+b.
> ```

## 【Solution】

> ```
> 因为c=a+b,所以，c可以分解到a，b中，所以可以先不考虑c，最后再处理。
> 即此题就是求ax+by=B-A的解。
> 由扩展欧几里得:
> ```
>
> $$
> ax+by=gcd(a,b)\\
> 则两边都乘以\frac {c}{gcd(a,b)}得：ax*(\frac c{gcd(a,b)})+by*(\frac c{gcd(a,b)})=c
> $$
>
> ```
> 可求得一对特解：x0=xc/gcd(a,b),y0=yc/gcd(a,b)(这里不懂可以先去学扩展欧几里得)
> 则通解x=x0+b/gcd*t,y=y0-a/gcd*t,其中t为未知数。
> 因为题目要求最小步数，即如果x==y，那么答案就是x。
> 若x!=y，且xy>0,则答案就是max(x,y); 例：2x+3y=7,解得x=2,y=1,那么走的步数就是c+a=5+2=7,2步
> 否则，xy<0,则答案就是abs(x)+abs(y);例：3x+5y=7,解得x=-1,y=2,那么走的步数就是b+b+(-a)=7,3步
> 所以最小步数取在x==y时，对上面通解进行求解，可解的t的值，但由于t可能不为整数解，所以对其两边求最小
> 值。
> ```
>
> $$
> 求证：“二元一次不定式ax+by=c，(a,b)=1,且有一对特解x=n,y=m,则它的通解为\\x=n-bt,y=m+at.\\
> 证明：
> 	设(n,m)(x,y)是两组解，则an+bm=c,ax+by=c,两式相减得：\\
> 	a(n-x)+b(m-y)=0,因为a,b互素，所以a=|m-y|,b=|n-x|,所以b能整除n-x\\
> 	则n-x=tb,x=n-bt,带入原式得y=m+at\\得证。
> $$
>
>

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-14 16:11:04 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-14 16:32:55
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int exgcd(int a,int b,int &x,int &y)//扩展欧几里得
{
    if(b==0)
    {
        x=1,y=0;
        return a;
    }
    int ans=exgcd(b,a%b,y,x);
    y-=(a/b)*x;
    return ans;
}
void solve(int a,int b,int c)
{
    int x,y;
    int gcd=exgcd(a,b,x,y);
    if(c%gcd!=0)
    {
        cout<<-1<<endl;
        return ;
    }
    x*=c/gcd,y*=c/gcd;//ax+by=gcd(a,b)---->ax*(c/gcd(a,b))+by*(c/gcd(a,b))=c,x,y为一个特解
    a/=gcd,b/=gcd;//为求通解x=x0+b/gcd*t,y=y0-a/gcd*t,做准备
    int mid=(y-x)/(a+b),ans=1e18;//当x==y时，求得t的值
    for(int i=mid-1;i<=mid+1;i++)//查找最终答案
    {
        int tmp=0;
        if(abs(x+b*i)+abs(y-a*i)==abs(x+b*i+y-a*i)) tmp=max(abs(x+b*i),abs(y-a*i)); //同方向
        else tmp=abs(x+b*i)+abs(y-a*i);//不同方向
        ans=min(ans,tmp);
    }
    cout<<ans<<endl;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin>>t;
    while(t--)
    {
        int A,B,a,b;
        cin>>A>>B>>a>>b;
        int c=abs(B-A);
        solve(a,b,c);
    }
    cin.get(),cin.get();
    return 0;
}
```
