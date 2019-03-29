#  [BZOJ-2728-与非-线性基-任意组合的与非值在[l,r]内的个数](https://www.lydsy.com/JudgeOnline/problem.php?id=2728)



## 【Description】

```cpp
给定n个非负整数A1,A2……An和约定位数k，利用NAND运算符与括号，每个数可以用任意次，请你求出范围[l,r]内可以
被计算的数有多少个。
```

## 【Input】

```cpp
输入文件第一行是用空格隔开的四个正整数N，K，L和R，接下来的一行是N个非负整数A1,A2……AN，其含义如上所述。
100%的数据满足K≤60且N≤1000,0<=Ai<=2^k-1,0<=L<=R<=10^18
```

## 【Output】

```cpp
仅包含一个整数，表示[L,R]内可以被计算出的数的个数
```

------



## 【Examples】 

#### Sample Input

```cpp
3 3 1 4                        
3 4 5
```

#### Sample Output

```cpp
4
```

------



## 【Problem Description】

```cpp
无
```

## 【Solution】

```cpp
首先，nand可以表示出所有逻辑运算
not a=a nand a
a and b=not (a nand b)
……
这是离散数学里的一个性质，忘了叫啥了……

既然nand可以表示出所有逻辑运算，将相当于能够对n个数，随意组合然后进行任意一种逻辑运算，这样就能表达出大部分
的数，但是有些数还是不能弄出来。
例如对与15(1111),8(1000),7(0111),1(0001),这四个数来说，在二进制位中，第二位和第三位在四个数中都是相同
的，那么我们对其进行任意的逻辑运算后的结果的第二位和第三位也一定相同。那么x01x，x10x，这种有8个数就不能弄
出来。

所以我们就可以对每一个二进制位，求出n个数中都与它相同的位置，作为一个基底。
最后统计个数时，将基向量从小到大‘或’起来，并判断是否大于指定值，若不大于，且当前基向量的二进制中0的个数为
r，则能产生的数的个数就为2^r。

```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-03-01 20:48:21 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-03-01 22:04:59
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int a[maxn],p[maxn],cnt=0;
bool vis[maxn];
int cal(int v){
    if(v==-1) return -1;
    int ret=0,ans=0;
    for(int i=1;i<=cnt;i++){
        if((ret|p[i])>v) continue; //当前能得到最大的数是否在指定范围内围内判断
        ret|=p[i];
        ans+=1LL<<(cnt-i); //对于当前基向量中，写成二进制后0的个数有cnt-i个，所以可以产生2^(cnt-i)个数
    }
    return ans;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,k,l,r;scanf("%lld%lld%lld%lld",&n,&k,&l,&r);
    for(int i=0;i<n;i++){
        scanf("%lld",a+i);
    }
    for(int i=k-1;i>=0;i--){ //对于前i位，对于n个数中的每个数都满足wj==wi则，第j位置1
        if(vis[i]) continue;
        int now=(1LL<<k)-1;
        for(int j=0;j<n;j++){
            if(a[j]&(1LL<<i)) now&=a[j]; 
            else now&=~a[j];
        }
        for(int j=0;j<k;j++) if(now&(1LL<<j)) vis[j]=1;
        p[++cnt]=now; //作为一个基向量 保存起来
    }
    printf("%lld\n",cal(r)-cal(l-1));
    cin.get(),cin.get();
    return 0;
}
```
