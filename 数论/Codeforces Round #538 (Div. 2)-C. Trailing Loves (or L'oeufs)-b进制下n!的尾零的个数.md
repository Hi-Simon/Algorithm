# [Codeforces Round #538 (Div. 2)-C. Trailing Loves (or L'oeufs?)-b进制下n!的尾零的个数](https://codeforces.com/contest/1114/problem/C)

## 【Description】

```cpp
Aki is fond of numbers, especially those with trailing zeros. For 
example, the number 9200 has two trailing zeros. Aki thinks the more 
trailing zero digits a number has, the prettier it is.

However, Aki believes, that the number of trailing zeros of a number 
is not static, but depends on the base (radix) it is represented in. 
Thus, he considers a few scenarios with some numbers and bases. And 
now, since the numbers he used become quite bizarre, he asks you to 
help him to calculate the beauty of these numbers.

Given two integers n and b (in decimal notation), your task is to 
calculate the number of trailing zero digits in the b-ary (in the 
base/radix of b) representation of n! (factorial of n).
```

## 【Input】

```cpp
The only line of the input contains two integers n and b (1≤n≤1018, 
2≤b≤1012).
```

## 【Output】

```cpp
Print an only integer — the number of trailing zero digits in the 
b-ary representation of n!
```

------

## 【Examples】

#### Sample Input

```cpp
6 9
```

#### Sample Output

```cpp
1
```

#### Sample Input

```cpp
38 11
```

#### Sample Output

```cpp
3
```

#### Sample Input

```cpp
5 2
```

#### Sample Output

```cpp
3
```

#### Sample Input

```cpp
5 10
```

#### Sample Output

```cpp
1
```

------

## 【Problem Description】

```cpp
b进制下n!的尾零的个数
```

## 【Solution】

```cpp
n和b都特别大，所以肯定需要一些技巧。
从简单的10进制看起，若求n!的尾零的个数，我们可以考虑对n!分解质因数，对0的个
数有贡献的只有2，5两个数(2*5=10)，分别统计其在n!的因子中的个数，那么尾零的
个数就是他们中较小的一个。

怎么对n!求呢？
考虑8!=8*7*6*5*4*3*2*1求其中包含2为其因子的个数：
对于1~8这8个数，有因子2的数为8，6，4，2，即8/2=4个。
那么分别将其除以2后为4，3，2，1，对于这四个数有因子2的为4，2，即4/2=2个
……
……
类似一直到n=0;

那么就可以得到10进制下n!中x因子的个数就可以通过以下函数得到：
int solve(int n,int x){
    int ans=0;
    while(n){
        n/=x;
        ans+=n;
    }
    return ans;
}

那么对于b进制，只要对b分解质因数，然后对b的每个质因数求其在n!中的个数，那么
最后答案就是个数最少的那个。

注意若对于b=9这种，质因数3出现两次，那么对于3求得其在n!中出现的次数后，需要
除以质因数出现的次数，即2。
```

------

## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-03-30 18:06:40 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-03-30 18:14:42
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int solve(int n,int x,int num){ //求因子x在n!中出现的次数，num为因子x在b中出现的次数。
    int ans=0;
    while(n){
        n/=x;ans+=n; 
    }
    return ans/num;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,b;cin>>n>>b;
    int ans=1e18;
    for(int i=2;i*i<=b;i++){ //对b分解质因数
        int cnt=0;
        if(b%i==0){
            while(b%i==0){
                b/=i;cnt++;
            }
            ans=min(ans,solve(n,i,cnt)); //b的所有质因数 在n!中出现个数最少的那个
        }
    }
    if(b>1) ans=min(ans,solve(n,b,1)); 
    cout<<ans<<endl;
    cin.get(),cin.get();
    return 0;
}
```
