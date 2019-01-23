#  [UVA-11754-Code Feat-数论-中国剩余定理+枚举](https://vjudge.net/problem/UVA-11754)



## 【Description】

```c++
Hooray! Agent Bauer has shot the terrorists, blown up the bad guy base, saved the 
hostages, exposedthe moles in the government, prevented an environmental catastrophe,
and found homes for threeorphaned kittens, all in the span of 19 consecutive hours. But
now, he only has 5 hours remaining todeal with his  nal challenge: an activated nuclear 
bomb protected by a security code. Can you helphim  gure out the code and deactivate 
it? Events occur in real time.
    he government hackers at CTU (Counter-Terrorist Unit) have learned some things 
about the code,but they still haven't quite solved it. They know it's a single, 
strictly positive, integer. They also know several clues of the form \when divided by
X,the remainder is one offY1;Y2;Y3;..;Yk"There aremultiple solutions to these clues,but 
the code is likely to be one of the smallest ones. So they'd likeyou to print out 
the  rst few solutions, in increasing order.
    The world is counting on you
```

## 【Input】

```c++
Input consists of several test cases.  Each test case starts with a line containing C,
the number of clues (1 <= C <= 9), and S, the number of desired solutions (1 <= S <= 
10).  The next C lines each start with two integers X (2 <= X) and k (1 <= k <= 100),
 followed by the k distinct integers Y1, Y2, ..., Yk (0 <= Y1, Y2, ..., Yk < X).
You may assume that the Xs in each test case are pairwise relatively prime (ie, they 
have no common factor except 1).  Also, the product of the Xs will fit into a 32-bit 
integer.
The last test case is followed by a line containing two zeros.
```

## 【Output】

```c++
For each test case, output S lines containing the S smallest positive solutions to the 
clues, in increasing order.
Print a blank line after the output for each test case.
```

------



## 【Examples】 

#### Sample Input

```c++
3 2
2 1 1
5 2 0 3
3 2 1 2
0 0
```

#### Sample Output

```c++
5
13
```

------



## 【Problem Description】

```c++
有C个互质的数，对于每个数，分别有k个余数，求最小的S个数，满足这S个数对Ci取模的余数等于k个余数中的其中一
个
```

## 【Solution】

```c++
第一想法就是枚举所有余数组合，分别进行中国剩余定理求解出对应的数，最后输出最小的S个数即可。
事实证明超时，原因是当余数个数过大时，枚举会很慢，不如直接暴力来的快。
所以将其分为两部分，当所有余数个数的乘积小于10000时，用中国剩余定理求解，否则，直接暴力求解。

中国剩余定理：
基础定理：
定理1：几个数相加，如果存在一个加数，不能被整数a整除，那么它们的和，就不能被整数a整除。
定理2：两数不能整除，若除数扩大（或缩小）了几倍，而被除数不变，则其商和余数也同时扩大（或\\缩小）相同的倍数（余数必小于除数）。

例：求一个最小的数，他分别除以3，5，7,余数分别为2，3，2。
找出所有除数的最小公倍数lcm（3，5，7）=105。
对于3，lcm/3=35,35%3=2,此时找到基础数35，它满足能整除5和7，且除以三3余2.
对于5，lcm/5=21,21%5=1,由定理2可得，3*21%5=3，此时找到基础数63，他能被3，7整除，且除以5余3.
对于7，lcm/7=15,15%7=1,同理得，2*15%7=2,此时找到基础数30，他能被3，5整除，且除以7余2.
最后由定理1将所有找的的数加起来35+63+30=128,因为是求最小的数,所以在对他们的最小公倍数取模即23.

编码用扩展欧几里得来做，原理如下：
对于a[i]，lcm/a[i]=m, 假设m*x%a[i]==1,则m*x=a[i]*y+1，则m*x+(-a[i]*y)=1,即我们用扩展欧几里得
求出x即可，那么基础数就等于m*x*r[i](其中a[i],为除数，r[i]为余数)
```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-12 21:35:19 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-23 20:28:22
 */
#include <bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
map<int, int> p;
int a[maxn],r[maxn];//除数，余数的一个组合
set<int> g[maxn],ans;//g[i][j],为所有a[i]的余数
set<int> s[maxn];
int exgcd(int a,int b,int &x,int &y)//扩展欧几里得
{
    if(b==0)
    {
        x=1,y=0;
        return a;
    }
    int ans=exgcd(b,a%b,y,x);
    y-=a/b*x;
    return ans;
}
int China(int C)//中国剩余定理
{
    int M=1,x,y,ans=0;
    for(int i=1;i<=C;i++) M*=a[i];//最小公倍数
    for(int i=1;i<=C;i++)
    {
        int w=M/a[i];
        int d=exgcd(a[i],w,x,y);
        ans=(ans+y*w*r[i])%M;//所有基础数的和
    }
    return (ans+M)%M;//答案
}
void dfs(int c,int C)//dfs枚举所有余数的组合
{
    if(c==C+1)
    {
        ans.insert(China(C));
        return ;
    }
    for(auto v:g[c])
    {
        r[c]=v;
        dfs(c+1,C);
    }
}
void solve_enum(int t,int C,int S)//>10000时，直接暴力
{
    for(int i=0;S!=0;i++)
    {
        for (auto v : g[t])
        {
            int n = i * a[t] + v;
            if(!n) continue;
            int flag=0;
            for(int j=1;j<=C;j++)
            {
                if(j==t) continue;
                if(!g[j].count(n%a[j])){flag=1;break;}
            }
            if(!flag) {cout<<n<<endl;if(--S==0) return ;}
        }
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int C, S;
    while (cin >> C >> S && C && S)
    {
        ans.clear();int t=1,tol=1;p.clear();int M=1;
        for (int i = 1; i <= C; i++)
        {
            cin >> a[i];M*=a[i];
            int n;
            cin >> n;
            for (int j = 1; j <= n; j++)
            {
                int x;
                cin >> x;
                g[i].insert(x);
            }
            tol*=g[i].size();//所有余数个数的乘积
            if(g[i].size()*a[t]<g[t].size()*a[i]) t=i;//找到最小的i，满足num[i]/a[i]最小，其中num[i]为余数的个数，a[i]为除数
        }
        if(tol<10000)
        {
            dfs(1, C);
            for(int i=0;S!=0;i++)//按顺序输出最小的数，且要保证不为0
            {
                for(auto v:ans)
                {
                    int n=i*M+v;
                    if(n>0)
                    {
                        cout<<n<<endl;
                        if(--S==0) break;
                    }
                }
            }
        }
        else solve_enum(t,C,S);
        for (int i = 0; i <= C; i++)
            g[i].clear();
        cout<<endl;//题目要求输出一个空行
    }
    cin.get(), cin.get();
    return 0;
}
```
