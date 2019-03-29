#  [Codeforces Round #532 (Div. 2)-F. Ivan and Burgers-线性基](http://codeforces.com/contest/1100/problem/F)



## 【Description】

```cpp
Ivan loves burgers and spending money. There are   burger joints on the street where Ivan
lives. Ivan has   friends, and the  -th friend suggested to meet at the joint li and walk
to the joint ri(li<ri). While strolling with the  i-th friend Ivan can visit all joints 
which satisfy li<xi<ri.
    
For each joint Ivan knows the cost of the most expensive burger in it, it costs     
burles. Ivan wants to visit some subset of joints on his way, in each of them he will buy
the most expensive burger and spend the most money. But there is a small issue: his card 
broke and instead of charging him for purchases, the amount of money on it changes as 
follows.
    
If Ivan had   burles before the purchase and he spent   burles at the joint, then after 
the purchase he would have   burles, where   denotes the bitwise XOR operation.
    
Currently Ivan has       burles and he wants to go out for a walk. Help him to determine 
the maximal amount of burles he can spend if he goes for a walk with the friend  . The 
amount of burles he spends is defined as the difference between the initial amount on his
account and the final account.
```

## 【Input】

```cpp
The first line contains one integer n  (1<=n<=500000) — the number of burger shops.
    
The next line contains n integers c1,c2,……,cn(0<=ci<=1e6), where ci  — the cost of the 
most expensive burger in the burger joint  .
    
The third line contains one integer q (1<=q<=500000) — the number of Ivan's friends.

Each of the next   lines contain two integers li and ri (1<=li<=ri<=n) — pairs of numbers
of burger shops between which Ivan will walk. 
```

## 【Output】

```cpp
Output q  lines,i-th of which containing the maximum amount of money Ivan can spend with 
the friend i .
```

------



## 【Examples】 

#### Sample Input

```cpp
4
7 2 3 4
3
1 4
2 3
1 3
```

#### Sample Output

```cpp
7
3
7
```
#### Sample Input

```cpp
5
12 14 23 13 7
15
1 1
1 2
1 3
1 4
1 5
2 2
2 3
2 4
2 5
3 3
3 4
3 5
4 4
4 5
5 5
```

#### Sample Output

```cpp
12
14
27
27
31
14
25
26
30
23
26
29
13
13
7
```
------



## 【Problem Description】

```cpp
给你n个数a1,a2,……,an，然后有q次询问，每次询问给你一个区间[l,r]，在区间内选一些数使得异或值最大，输出这个
最大值。
```

## 【Solution】

```cpp
线性基
将所有区间按右端点从小到大排序，插入时，将右段点之前的所有数插入线性基，并对a[i]来说，i越大，优先级越高，即
替换掉原来的基向量。插入时同时记录每个二进制位插入的i是多少。
查询最大值时 增加一个判断，判断当前二进制位上插入的数的位置i是否大于等于l。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-03-06 20:00:45 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-03-06 20:26:44
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 500005
int a[25],v[maxn],pos[25]; 
struct node{
    int u,v,ord;
    bool operator <(const node a)const {
        if(v==a.v) return u<a.u;
        return v<a.v;
    }
}c[maxn];
void insert(int val,int p){//线性基的插入
    for(int i=20;~i;i--){
        if(val>>i&1LL){
            if(!a[i]){
                a[i]=val;pos[i]=p;break;//记录位置
            }
            if(p>pos[i]) swap(pos[i],p),swap(a[i],val); //p越大，优先插入
            val^=a[i];
        }
    }
}
int query(int l){ //查询最大值
    int ans=0;
    for(int i=20;~i;i--){
        if(pos[i]>=l&&(ans^a[i])>ans) ans^=a[i]; //保证插入的数的范围在[l,r]内
    }
    return ans;
}
int ans[maxn];
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;scanf("%lld",&n);
    for(int i=1;i<=n;i++){
        scanf("%lld",v+i);
    }
    int q;scanf("%lld",&q);
    for(int i=1;i<=q;i++) scanf("%lld%lld",&c[i].u,&c[i].v),c[i].ord=i;
    sort(c+1,c+q+1);int r=1;
    for(int i=1;i<=q;i++){
        while(r<=c[i].v){
            insert(v[r],r);r++;
        }
        ans[c[i].ord]=query(c[i].u);
    }
    for(int i=1;i<=q;i++) printf("%lld\n",ans[i]);
    cin.get(),cin.get();
    return 0;
}
```
