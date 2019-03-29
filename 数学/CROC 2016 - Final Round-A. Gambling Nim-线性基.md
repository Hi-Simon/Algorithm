#  [CROC 2016 - Final Round-A. Gambling Nim-线性基](http://codeforces.com/contest/662/problem/A)



## 【Description】

```cpp
As you know, the game of "Nim" is played with n piles of stones, where the i-th pile 
initially contains ai stones. Two players alternate the turns. During a turn a player 
picks any non-empty pile and removes any positive number of stones from it. The one who 
is not able to make a move loses the game.
    
Petya and Vasya are tired of playing Nim, so they invented their own version of the game
and named it the "Gambling Nim". They have n two-sided cards, one side of the i-th card 
has number ai written on it, while the other side has number bi. At the beginning of the
game the players put all the cards on the table, each card only one of its sides up, and
this side is chosen independently and uniformly. Thus they obtain a sequence c1, c2, ..., 
cn, where ci is equal to ai or bi. Then they take n piles of stones, with i-th pile 
containing exactly ci stones and play Nim. Petya takes the first turn.
    
Given that both players play optimally, find the probability of Petya's victory. Output
the answer as an irreducible fraction.
```

## 【Input】

```cpp
The first line of the input contains a single integer n (1 ≤ n ≤ 500 000) — the number of
cards in the deck.
    
Each of the following n lines contains the description of one card, consisting of two 
integers ai and bi (0 ≤ ai, bi ≤ 10^18).
```

## 【Output】

```cpp
Output the answer as an irreducible fraction p / q. If the probability of Petya's victory
is 0, print 0/1.
```

------



## 【Examples】 

#### Sample Input

```cpp
2
1 1
1 1
```

#### Sample Output

```cpp
0/1
```
#### Sample Input

```cpp
2
1 2
1 2
```

#### Sample Output

```cpp
1/2
```
#### Sample Input

```cpp
3
0 4
1 5
2 3
```

#### Sample Output

```cpp
1/1
```
------



## 【Problem Description】

```cpp
给你两组数，ai-an,bi-bn,从这两组数中任意选择n个数，求使得这n个数异或和不为0的 的方案数占总方案的多少。
选择的时候，对于同一个i，要么选ai，要么选bi，不能同时选择ai和bi。
```

## 【Solution】

```cpp
转换一下，我们求选择的n个数异或和为0的方案数。
考虑到对于同一个i，只能选择ai或bi中的一个，所以，可以想到异或的特性，（a^b）^a=b,（a^b）^b=a。
所以将ai^bi建立线性基。令s=ai^……^an。对于s的所有二进制位，判断它是否为1，若为1，则异或相应位置的基向量。

最后判断s是否为0，若不为0，则不可能出现。（前面做过一道题，n个数，它们得到的线性基中基向量的个数为r个，则
这n个数任意组合异或所得的所有数出现的次数相同，都为2^(n-r)个。）

否则，方案数为1-[2^(n-r)]/(2^n)=(2^r)-1/(2^r)
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-03-04 22:20:08 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-03-04 22:25:24
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 500005
int a[maxn];
void insert(int val){//线性基的插入
    for(int i=60;~i;i--){
        if(val>>i&1LL){
            if(!a[i]){a[i]=val;break;}
            val^=a[i];
        }
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,s=0;scanf("%lld",&n);
    for(int i=1;i<=n;i++){
        int x,y;scanf("%lld%lld",&x,&y);
        s^=x;insert(x^y);
    }
    int cnt=0;
    for(int i=60;~i;i--){
        if(s>>i&1LL) s^=a[i];//若第i个二进制为1，则异或相应的基向量，使其为0
        if(a[i]) cnt++; //统计基向量的个数
    }
    if(s) return puts("1/1"),0;
    printf("%lld/%lld\n",(1LL<<cnt)-1,1LL<<cnt);//若出现0，则0出现的次数一定是2^(n-r)次。
    cin.get(),cin.get();
    return 0;
}
```
