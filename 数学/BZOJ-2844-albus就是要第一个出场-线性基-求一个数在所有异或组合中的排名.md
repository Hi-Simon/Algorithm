#  [BZOJ-2844-albus就是要第一个出场-线性基-求一个数在所有异或组合中的排名](https://www.lydsy.com/JudgeOnline/problem.php?id=2844)



## 【Description】

```cpp
已知一个长度为n的正整数序列A（下标从1开始）， 令 S = { x | 1 <= x <= n }, S 的幂集2^S定义为S 所有子
集构成的集合。定义映射 f : 2^S -> Zf(空集) = 0f(T) = XOR A[t] , 对于一切t属于T现在albus把2^S中每个
集合的f值计算出来， 从小到大排成一行， 记为序列B（下标从1开始）。 给定一个数， 那么这个数在序列B中第1
次出现时的下标是多少呢？
```

## 【Input】

```cpp
第一行一个数n, 为序列A的长度。接下来一行n个数， 为序列A， 用空格隔开。最后一个数Q， 为给定的数.
```

## 【Output】

```cpp
共一行， 一个整数， 为Q在序列B中第一次出现时的下标模10086的值.
```

------



## 【Examples】 

#### Sample Input

```cpp
3
1 2 3
1
```

#### Sample Output

```cpp
3
```

------



## 【Problem Description】

```cpp
无
```

## 【Solution】

```cpp
线性基其实就是，几个基向量，可以类比线性代数中的基向量。
	n个数，子集有2^n次方个，所以异或值也有2^n次方个。如果将这些数去重后，就可以用类似线性基查询第k小的方
法求得q的排名。
	但是每个数可能不只一个。
结论：每个数的个数相同，都为2^(n-r)个。其中r为线性基的数量
原因：
	n个原始数就是原始向量，线性基也就是原始向量上三角化后的向量。将线性基的数纵向排列，并写成二进制的形式。
就形成了矩阵，线性基的数量就是矩阵的秩r，如果线性基的数量r==n,n个数的所有子集形成的异或值，都是独一无的。
如果线性基的数量r<n，则每个数的个数就为2^(n-r)个。相当于有n-r的零向量，这些零向量的子集个数有2^(n-r)个
任意一个数x与这些零向量所形成的子集异或,值x不变，所以x就有2^(n-r)个。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-02-28 21:10:00 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-02-28 22:16:30
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
#define mod 10086
int a[maxn],cnt=0,p[maxn],num=0;
void insert(int val){ //线性基的插入
    for(int i=60;i>=0;i--){
        if(val&(1LL<<i)){
            if(!a[i]){a[i]=val;num++;break;}
            else val^=a[i];
        }
    }
}
bool flag=0;
void rebuild(int n){ //线性基的重建，将上三角矩阵转化为对角矩阵
    for(int i=60;i>=0;i--){
        for(int j=i-1;j>=0;j--){
            if(a[i]&(1LL<<j)) a[i]^=a[j];
        }
    }cnt=0;
    for(int i=0;i<=60;i++){
        if(a[i]) p[cnt++]=i;
    }
}
int fpow(int a,int b){//快速幂
    int ans=1;a%=mod;
    while(b){
        if(b&1) ans=ans*a%mod;
        a=a*a%mod;
        b>>=1;
    }
    return ans;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;scanf("%lld",&n);
    for(int i=1;i<=n;i++){
        int x;scanf("%lld",&x);insert(x);
    }
    rebuild(n);
    int q;scanf("%lld",&q);
    int ans=0;
    for(int i=0;i<cnt;i++) if(q&(1<<p[i])) ans=(ans+(1<<i))%mod;
    printf("%lld\n",(fpow(2,n-num)%mod*ans%mod+1)%mod);
    cin.get(),cin.get();
    return 0;
}
```
