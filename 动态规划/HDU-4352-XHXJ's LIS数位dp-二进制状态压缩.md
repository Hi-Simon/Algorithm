#  [HDU-4352-XHXJ's LIS数位dp-二进制状态压缩](https://vjudge.net/problem/HDU-4352)

[TOC]



## Description 

> ```
> #define xhxj (Xin Hang senior sister(学姐))
> If you do not know xhxj, then carefully reading the entire description is very important.
> As the strongest fighting force in UESTC, xhxj grew up in Jintang, a border town of 
> Chengdu.
> Like many god cattles, xhxj has a legendary life
> 2010.04, had not yet begun to learn the algorithm, xhxj won the second prize in the 
> university contest. And in this fall, xhxj got one gold medal and one silver medal of 
> regional contest. In the next year's summer, xhxj was invited to Beijing to attend the 
> astar onsite. A few months later, xhxj got two gold medals and was also qualified for 
> world's final. However, xhxj was defeated by zhymaoiing in the competition that determined 
> who would go to the world's final(there is only one team for every university to send to 
> the world's final) .Now, xhxj is much more stronger than ever，and she will go to the 
> dreaming country to compete in TCO final.
> As you see, xhxj always keeps a short hair(reasons unknown), so she looks like a boy( I 
> will not tell you she is actually a lovely girl), wearing yellow T-shirt. When she is not
> talking, her round face feels very lovely, attracting others to touch her face gently。
> Unlike God Luo's, another UESTC god cattle who has cool and noble charm, xhxj is quite 
> approachable, lively, clever. On the other hand,xhxj is very sensitive to the beautiful
> properties, "this problem has a very good properties"，she always said that after ACing a
> very hard problem. She often helps in finding solutions, even though she is not good at the 
> problems of that type.
> Xhxj loves many games such as，Dota, ocg, mahjong, Starcraft 2, Diablo 3.etc，if you can 
> beat her in any game above, you will get her admire and become a god cattle. She is very 
> concerned with her younger schoolfellows, if she saw someone on a DOTA platform, she would
> say: "Why do not you go to improve your programming skill". When she receives sincere 
> compliments from others, she would say modestly: "Please don’t flatter at me.(Please don't
> black)."As she will graduate after no more than one year, xhxj also wants to fall in love.
> However, the man in her dreams has not yet appeared, so she now prefers girls.
> Another hobby of xhxj is yy(speculation) some magical problems to discover the special 
> properties. For example, when she see a number, she would think whether the digits of a 
> number are strictly increasing. If you consider the number as a string and can get a 
> longest strictly increasing subsequence the length of which is equal to k, the power of 
> this number is k.. It is very simple to determine a single number’s power, but is it also
> easy to solve this problem with the numbers within an interval? xhxj has a little tired，she 
> want a god cattle to help her solve this problem,the problem is: Determine how many numbers 
> have the power value k in [L，R] in O(1)time.
> For the first one to solve this problem，xhxj will upgrade 20 favorability rate。
> ```

## Input

> ```
> First a integer T(T<=10000),then T lines follow, every line has three
> positive integer L,R,K.(0<L<=R<2^63-1 and 1<=K<=10).
> ```

## Output

> ```
> For each query, print "Case #t: ans" in a line, in which t is the 
> number of the test case starting from 1 and ans is the answer.
> ```

------



## Examples 

> ### Input

> ```
> 1
> 123 321 2
> ```

> ###Ouput

> ```
> Case #1: 139 
> ```

------



## Problem Description

> ```
> 吐槽：一堆废话。
> 如果一个数的LIS的长度为k，它就有k的能量值，让你求[L,R]区间内，能量为k的数
> 的数目。
> 比如 15246 能量为4:1 2 4 6.
> ```

## Solution

> ```
> 数位dp，二进制状态压缩，LIS O(nlogn)算法思想
> 基本的数位dp，就不说了。
> 
> 先说LIS O(nlogn)算法思想：
> 定义f[len]数组，表示LIS的长度为len时，最后一位为f[len]。要求得数组为
> a[i]={4 2 6 3 1 5};初始len=0，i=1;
> if(a[i]>f[len]) ++len,f[len]=a[i];
> else 从1~len中找到一个位置k，满足f[k-1]<a[i]<f[k],令f[k]=a[i];
> ---总的来说就是找到第一个比自己大或相等的数，将它删除，把自己放上去。
> ---如果没有比自己大的数，就把自己放到最后
> f数组里保存的值是具有单调性的，所以查找k的过程我们就可以用二分的方法得到
> 所以复杂度就为O(nlogn).
> 整个过程就为：
> 4
> 2
> 2 6
> 2 3
> 1 3
> 1 3 5
> 最终答案为len=3，但要注意的是f数组里保存的数并不是LIS。
> ```
> ```
> 2进制状态压缩求LIS：
> 0~9只有十个数，LIS最大长度不过为10。
> 我们用10位二进制数来表示9~0的数字是否出现过。
> 比如上一个pos(数位dp中，枚举第pos个位置）的状态是0101000101表示8，6，2，0，出现过。
> 到当前pos时，枚举0~9。（在最初的状态的基础上改变）
> 0：状态不改变
> 1：0101000011，将2位置置0，1 位置置1
> 2: 状态不改变
> 3：0100001101，将6位置置0，3 位置置1
> .
> .
> .
> 9:1101000101, 将9位置置1
> ---总的来说就是找到第一个比当前枚举的数大或相等的位置，将它置零，自己置1.
> ---如果没有就把自己的位置置1.
> 二进制中1的个数就是LIS的长度。
> 定义dp[20][1024][10]数组，分别为，当前枚举的pos，LIS的二进制状态十进制表示，
> 题目要求的K。
> 结束。具体看代码吧。
> ```
>
> 

------



## Code

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-16 14:01:33 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-08-16 14:26:01
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 105
int a[maxn];
int dp[maxn][1<<10][12];//2进制状态压缩，9~0十位二进制数分别表示9~0的数字是否出现过
int l, r, k;
int gets(int s)//得到s中1的个数，即当前LIS的长度
{
    int cnt=0;
    while(s)
    {
        if(s&1) cnt++;
        s>>=1;
    }
    return cnt;
}
int news(int s,int x)//更新状态，LIS O(nlogn)算法思想
{
    for(int i=x;i<10;i++)
    {
        if(s&(1<<i)) return (s^(1<<i))|(1<<x);
    }
    return (s|(1<<x));
}
int dfs(int pos,int s,bool flag,bool limit)
{
    if(pos==0) return gets(s)==k;
    if(!limit&&dp[pos][s][k]!=-1) return dp[pos][s][k];
    int up=limit?a[pos]:9;
    int tmp=0;
    for(int i=0;i<=up;i++)
    {
        tmp+=dfs(pos-1,flag&&i==0?0:news(s,i),i==0&&flag,limit&&i==up);//特判前导0
    }
    if(!limit) dp[pos][s][k]=tmp;
    return tmp;
}
int solve(int x)
{
    memset(a,0,sizeof(a));
    int pos=0;
    while(x)
    {
        a[++pos]=x%10;
        x/=10;
    }
    return dfs(pos,0,1,1);
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    memset(dp,-1,sizeof(dp));
    int t;
    cin>>t;
    for(int ca=1;ca<=t;ca++)
    {
        cout<<"Case #"<<ca<<": ";
        cin>>l>>r>>k;
        cout<<solve(r)-solve(l-1)<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
