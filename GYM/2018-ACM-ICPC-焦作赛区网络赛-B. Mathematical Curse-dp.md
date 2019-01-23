#  [2018-ACM-ICPC-焦作赛区网络赛-B. Mathematical Curse-dp](https://nanti.jisuanke.com/t/31711)



## 【Description】

> ```
> A prince of the Science Continent was imprisoned in a castle because of his 
> contempt for mathematics when he was young, and was entangled in some mathematical
> curses. He studied hard until he reached adulthood and decided to use his knowledge 
> to escape the castle.
> 
> There are NNN rooms from the place where he was imprisoned to the exit of the 
> castle. In the ithi^{th}ith room, there is a wizard who has a resentment value of
> a[i]a[i]a[i]. The prince has MMM curses, the jthj^{th}jth curse is f[j]f[j]f[j], 
> and f[j]f[j]f[j] represents one of the four arithmetic operations, namely 
> addition('+'), subtraction('-'), multiplication('*'), and integer division('/'). 
> The prince's initial resentment value is KKK. Entering a room and fighting with the
> wizard will eliminate a curse, but the prince's resentment value will become the 
> result of the arithmetic operation f[j]f[j]f[j] with the wizard's resentment value.
> That is, if the prince eliminates the jthj^{th}jth curse in the ithi^{th}ith room,
> then his resentment value will change from xxx to (x f[j] a[i]x\ f[j]\ a[i]x f[j] 
> a[i]), for example, when x=1,a[i]=2,f[j]=x=1, a[i]=2, f[j]=x=1,a[i]=2,f[j]='+', 
> then xxx will become 1+2=31+2=31+2=3.
> 
> Before the prince escapes from the castle, he must eliminate all the curses. He 
> must go from a[1]a[1]a[1] to a[N]a[N]a[N] in order and cannot turn back. He must 
> also eliminate the f[1]f[1]f[1] to f[M]f[M]f[M] curses in order(It is guaranteed 
> that N≥MN\ge MN≥M). What is the maximum resentment value that the prince may have
> when he leaves the castle?
> ```

## 【Input】

> ```
> The first line contains an integer T(1≤T≤1000)T(1 \le T \le 1000)T(1≤T≤1000), which
> is the number of test cases.
> 
> For each test case, the first line contains three non-zero integers: 
> N(1≤N≤1000),M(1≤M≤5)N(1 \le N \le 1000), M(1 \le M \le 5)N(1≤N≤1000),M(1≤M≤5) and 
> K(−1000≤K≤1000K(-1000 \le K \le 1000K(−1000≤K≤1000), the second line contains NNN 
> non-zero integers: a[1],a[2],...,a[N](−1000≤a[i]≤1000)a[1], a[2], ..., a[N](-1000 
> \le a[i] \le 1000)a[1],a[2],...,a[N](−1000≤a[i]≤1000), and the third line contains
> MMM characters: f[1],f[2],...,f[M](f[j]=f[1], f[2], ..., f[M](f[j] 
> =f[1],f[2],...,f[M](f[j]='+','-','*','/', with no spaces in between.
> ```

## 【Output】

> ```
>  For each test case, output one line containing a single integer.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 3
> 2 1 5
> 2 3
> /
> 3 2 1
> 1 2 3
> ++
> 4 4 5
> 1 2 3 4
> +-*/
> ```

> ###Sample Output

> ```
> 2
> 6
> 3
> ```

------



## 【Problem Description】

> ```
> 有n个数，m个操作符，你的初始值为k。
> 按顺序用当前值k对a[i]进行op[j]操作，操作结果为k=k op[j] a[i] （op[j]为四则运算的其中一种）
> 限制条件：
> 1、对于每个数a[i],只能从左到右选择，无论前面的数使用过或未使用过，都不能回头再选。
> 2、对于每个操作符op[j]，只能从左到右选择，并且每个操作符都必须使用且一次。
> 求最终的k的最大值
> ```

## 【Solution】

> ```
> dp
> 定义dp数组，
>     dp1[i][j];//前i个数，经过前j个操作后所获得的最大值
>     dp2[i][j];//前i个数，经过前j个操作后所获得的最小值
> dp1[i][j]只能有三个状态转移过来
> 1、前i-1个数，经过前j个操作后的最大值
> 2、前i-1个数，经过前j-1个操作后的最大值再对a[i]执行op[j]操作后的值
> 2、前i-1个数，经过前j-1个操作后的最小值再对a[i]执行op[j]操作后的值
> 三个状态中取最大的即可。
> dp2[i][j]类似，不累述了。
> 
> ```

------



## 【Code】

```c++
/*
 * @Author: Simon 
 * @Date: 2018-09-16 08:28:44 
 * @Last Modified by: Simon
 * @Last Modified time: 2018-09-16 09:22:42
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 1005
int a[maxn];
char op[7];
int dp1[maxn][7];//前i个数，经过前j个操作后所获得的最大值
int dp2[maxn][7];//前i个数，经过前j个操作后所获得的最小值
int cal(int a,char op,int k)//四种操作
{
	if(op=='+')
		return a+k;
	else if(op=='-')
		return k-a;
	else if(op=='*')
		return a*k;
	else if(op=='/')
		return k/a;
	return 0;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
	int t;
	cin>>t;
	while(t--)
	{
		int n,m,k;
		cin>>n>>m>>k;
		for(int i=1;i<=n;i++) cin>>a[i];
		cin>>op+1;
		int len=strlen(op+1);
		memset(dp1,-INF,sizeof(dp1));
		memset(dp2,INF,sizeof(dp2));
		for(int i=0;i<=n;i++)
		{
			dp2[i][0] = k;
			dp1[i][0] = k;
		}
		for(int i=1;i<=n;i++)
		{
			for(int j=1;j<=min(len,i);j++)
			{
				dp1[i][j] = max(dp1[i - 1][j], cal(a[i], op[j], dp1[i - 1][j - 1]));//第1个转移和第2个转移中最大的
				dp1[i][j] = max(dp1[i][j], cal(a[i], op[j], dp2[i - 1][j - 1]));//三个状态转移中最大的
				dp2[i][j] = min(dp2[i - 1][j], cal(a[i], op[j], dp2[i - 1][j - 1]));
				dp2[i][j] = min(dp2[i][j],cal(a[i],op[j],dp1[i-1][j-1]));
			}
		}
		cout<<dp1[n][len]<<endl;
	}
    cin.get(),cin.get();
    return 0;
}
```
