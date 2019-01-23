#  [2018-ACM-ICPC-徐州赛区网络赛-G. Trace](https://nanti.jisuanke.com/t/31459)



## 【Description】

> ```
> There's a beach in the first quadrant. And from time to time, there are sea waves. 
> A wave ( x , y ) means the wave is a rectangle whose vertexes are ( 0 , 0 ), ( x , 
> 0 ), ( 0 , y ), ( x , y ). Every time the wave will wash out the trace of former 
> wave in its range and remain its own trace of ( x , 0 ) -> ( x , y ) and ( 0 , y ) 
> -> ( x , y ). Now the toad on the coast wants to know the total length of trace on 
> the coast after n waves. It's guaranteed that a wave will not cover the other 
> completely.
> ```
>
> ![img](https://res.jisuanke.com/img/upload/36b67f52cd1e0e6ac91ed22078f0c564f9c8a8aa.png)

## 【Input】

> ```
> The first line is the number of waves n(n≤50000)n(n < 50000)n(n≤50000).
> The next nnn lines，each contains two numbers x y ,( 0<x0 < x0<x , y≤10000000y < 
> 10000000y≤10000000 )，the i-th line means the i-th second there comes a wave of ( x 
> , y ), it's guaranteed that when 1≤i1 < i1≤i , j≤nj < nj≤n ，xi≤xjx_i < x_jxi≤xj 
> and yi≤yjy_i < y_jyi≤yj don't set up at the same time.
> ```

## 【Output】

> ```
> An Integer stands for the answer.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 3
> 1 4
> 4 1
> 3 3
> ```

> ###Sample Output

> ```
> 10
> ```

------



## 【Problem Description】

> ```
> 每一个海浪能在海滩上形成矩形的轨迹，矩形都是在第一象限，且以(0,0)为左下角，(x,y)为右上角。
> 后面的海浪能把前面的海浪部分轨迹覆盖掉，求n个海浪后的轨迹长度。
> 
> 题目数据保证每一次的海浪形成的轨迹都有部分保留（好像是这个意思，以原描述为准）
> ```

## 【Solution】

> ```
> 对横坐标，纵坐标分别考虑，从最后一个海浪向前考虑，找到第一个比当前值小的值，当前海浪对长度的贡献为
> 其差值，如果没有比它小的，贡献就是其本身。
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-09 13:46:18 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-09 14:10:59
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
vector<int>s1,s2;
set<int>s;
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        int x,y;
        cin>>x>>y;
        s1.push_back(x);s2.push_back(y);
    }
    int ans=0,Max=0;
    for(int i=s1.size()-1;i>=0;i--)
    {
        auto it=s.lower_bound(s1[i]);//it-1就是第一个比当前值小的值
        if(it==s.begin()) ans+=s1[i];//没有比当前值小的
        else
        {
            it--;
            ans+=s1[i]-*it;//有比当前值小的，加上其差值
        }
        s.insert(s1[i]);
    }
    s.clear();
    for(int i=s2.size()-1;i>=0;i--)
    {
        auto it=s.lower_bound(s2[i]);
        if(it==s.begin()) ans+=s2[i];
        else
        {
            it--;
            ans+=s2[i]-*it;
        }
        s.insert(s2[i]);
    }
    cout<<ans<<endl;
    cin.get(),cin.get();
    return 0;
}
```
