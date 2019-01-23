#  [Balanced Numbers-数位dp-三进制状态压缩](https://vjudge.net/problem/SPOJ-BALNUM)

[TOC]



## ``Description ``

> ```
> Balanced numbers have been used by mathematicians for centuries. A positive integer is considered a balanced number if:
> 
> 1)      Every even digit appears an odd number of times in its decimal representation
> 
> 2)      Every odd digit appears an even number of times in its decimal representation
> 
> For example, 77, 211, 6222 and 112334445555677 are balanced numbers while 351, 21, and 662 are not.
> 
> Given an interval [A, B], your task is to find the amount of balanced numbers in [A, B] where both A and B are included.
> ```

## `Input`

> ```
> The first line contains an integer T representing the number of test cases.
> 
> A test case consists of two numbers A and B separated by a single space representing the interval. You may assume that 1 <= A <= B <= 10^19 
> ```

## `Output`

> ```
> or each test case, you need to write a number in a single line: the amount of balanced numbers in the corresponding interval
> ```

------



## `Examples` 

> ### `Input`

> ```
> 2
> 1 1000
> 1 9
> ```

> ###Ouput

> ```
> 147
> 4
> ```

------



## `Problem Description`

> ```
> 给你一个范围[A,B],让你判断范围内所有数是否为平衡数，是平衡数的条件为：
> 1、对于每一位上的数字，如果是偶数，则必须出现奇数次。
> 2、对于每一位上的数字，如果是奇数，则必须出现偶数次。
> ```

## `Solution`

> ```
> 数位dp
> 判断奇偶性这里，可以用三进制状态压缩。3进制有三个数0，1，2，则0表示未出现过，1表示出现奇数次，2表示出现偶数次。
> 0~9的每个数字用一位三进制来表示其出现的奇偶次数。所以总共需要10位三进制数，所以最大不超过6000。
> 
> 举个例子，2出现3次，5出现2次，9出现一次，其他未出现。则可以表示为：(001 002 000 1),转换为10进制即为1989。这个十进制数本身并没有什么意义，只是方便存储。
> 最终我们可以定义dp数组：dp[20][6000].
> 
> 解决完dp数组状态设计问题后，就需要解决状态转移问题。
> 每出现一个数字，我们就要将目前的十进制数转换为3进制数，然后将对应的数字的奇偶性做相应的改变。最后还要转换为10进制数，以方便存储。
> 当达到边界条件时，即pos=0时，只要判断当前0~9所有数字出现次数的奇偶性是否满足题目要求，如果满足则增加答案1次，否则不改变。
> 
> ```

------



## `Code`

```c++
/*
 * @Author: Simon 
 * @Date: 2018-08-14 21:36:20 
 * @Last Modified by:   Simon 
 * @Last Modified time: 2018-08-14 22:36:20 
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 105
int a[maxn];
int dp[maxn][60000];//0~9 每个数字的个数用一位三进制数来表示，所以总共需要10位
int digit[maxn];//3-10进制转换
void getT(int x)//转换为三进制
{
    int cnt=0;
    for(int i=0;i<=9;i++)
    {
        digit[i]=x%3;
        x/=3;
    }
}
int getD()//转换为十进制
{
    int s=0,base=1;
    for(int i=0;i<=9;i++)
    {
        s+=digit[i]*base;
        base*=3; 
    }
    return s;
}
bool check(int x)//判断是否是平衡数
{
    getT(x);//将当前十进制的数转换为10位的三进制数
    for(int i=0;i<=9;i++)//每一位对于一个数字出现的奇偶次数
    {
        if(!digit[i]) continue;
        if((i&1)&&digit[i]==1)return 0;//是奇数并且出现奇数次，false
        else if(!(i&1)&&digit[i]==2) return 0;//是偶数并且出现偶数次，false
    }
    return 1;//全部满足条件，true
}
int change(int x,int k)//改变k这个数字出现的奇偶次数
{
    getT(x);
        if(digit[k]==0) digit[k]=1;//未出现过，加1次
        else if(digit[k]==1) digit[k]=2;//已出现奇数次，改变为偶数次
        else if(digit[k]==2) digit[k]=1;//已出现偶数次，改变为奇数次
    return getD();//三进制转换为十进制
}
int dfs(int pos,int hash,bool limit)
{
    if(pos==0) return check(hash);
    if(!limit&&dp[pos][hash]!=-1) return dp[pos][hash];
    int up=limit?a[pos]:9;
    int tmp=0;
    for(int i=0;i<=up;i++)
    {
        tmp+=dfs(pos-1,((hash==0&&i==0)?0:change(hash,i)),limit&&i==up);//特判0
    }
    if(!limit) dp[pos][hash]=tmp;
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
    return dfs(pos,0,1);
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    memset(dp,-1,sizeof(dp));
    int t;
    cin>>t;
    while(t--)
    {
        int l,r;
        cin>>l>>r;
        cout<<solve(r)-solve(l-1)<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
