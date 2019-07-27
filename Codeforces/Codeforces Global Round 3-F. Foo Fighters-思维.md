# [Codeforces Global Round 3-F. Foo Fighters-思维](https://codeforces.com/contest/1148/problem/F)

## 【Description】

```cpp
You are given n objects. Each object has two integer properties: vali — its price — and 
maski. It is guaranteed that the sum of all prices is initially non-zero.

You want to select a positive integer s. All objects will be modified after that. The i-
th object will be modified using the following procedure:

Consider maski and s in binary notation,
Compute the bitwise AND of s and maski (s&maski),
If (s&maski) contains an odd number of ones, replace the vali with −vali. Otherwise do
nothing with the i-th object.
You need to find such an integer s that when the modification above is done the sum of 
all prices changes sign (if it was negative, it should become positive, and vice-versa; 
it is not allowed for it to become zero). The absolute value of the sum can be arbitrary.
```

## 【Input】

```cpp
The first line contains a single integer n (1≤n≤3⋅105) — the number of objects.

The i-th of next n lines contains integers vali and maski (−109≤vali≤109, 1≤maski≤262−1)
— the price of the object and its mask.

It is guaranteed that the sum of vali is initially non-zero.
```

## 【Output】

```cpp
Print an integer s (1≤s≤262−1), such that if we modify the objects as described above,
the sign of the sum of vali changes its sign.

If there are multiple such s, print any of them. One can show that there is always at 
least one valid s.
```

------



## 【Examples】 

#### Sample Input

```cpp
5
17 206
-6 117
-2 151
9 93
6 117
```

#### Sample Output

```cpp
64
```
#### Sample Input

```cpp
1
1 1
```

#### Sample Output

```cpp
1
```

------



## 【Problem Description】

```cpp
给n对数，每对有一个权值val和一个mask，让你选择一个合理的数s，使得它与所有的mask[i]做与位运算，若
s&mask[i]的结果的二进制中1的个数为奇数个，则val[i]=-val[i]，并且对所有数操作完之后的权值和与原来的权值
和 符号相反。值可以任意，但不能为0。
```

## 【Solution】

```cpp
将所有mask按最高位为1的位置分类，最多分为62类。按位从小到大枚举，对62类分别处理。对于每一类，将此类中的所
有权值相加，若与总权值同符号，则此位 置1，且此位为1的所有权值全取反。
此时可能出现一个问题，即此位为1的所有权值取反后之和 可能与总权值同符号，但是没关系，我们只要保证，当前处理
的类的权值与总权值符号相反即可，其他不在此类，但此位为1的其他数交给其他类处理即可。

合理性在于，62类包含的所有的数，若每一类中都满足原权值与变换后的权值符号相反，则所有数的权值必定满足。

假设最高位是第i位则称其为第i类，从小到大枚举的原因在于 对第i+1类的处理一定不会影响第i类甚至之前的类的结
果。因为对于第i类来说，第，i+1,i+2,i+3...等位上一定都是0。

结合代码看讲解，食用更佳。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-07-07 20:26:15 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-07-08 16:04:37
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
vector<pair<int,pair<int,int> > >a;
set<int>tmp;
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,sum=0;cin>>n;
    for(int i=1;i<=n;i++){
        int p,mask;cin>>p>>mask; 
        a.push_back({p,{mask,0LL}});
        sum+=p;
        for(int j=62;j>=0;j--){
            if(a[i-1].second.first>>j&1){
                a[i-1].second.second=j; //按最高位分类
                break;
            }
        }
    }
    int ans=0;
    if(sum<0) for(int i=0;i<n;i++) a[i].first*=-1; //统一处理正数。
    for(int i=0;i<=62;i++){//从小到大枚举类别，保证处理过的类不被影响
        int t=0;
        for(int j=0;j<n;j++){
            if(a[j].second.second!=i) continue;
            t+=a[j].first; //将第i类的所有权值相加
        }
        if(t>0){ //第i类权值和与总权值符号相同
            ans|=1LL<<i; //第i位 置1
            for(int j=0;j<n;j++){ //将所以第i位为1的数全部取反
                if(a[j].second.first>>i&1) a[j].first*=-1;
            }
        }
    }
    cout<<ans<<endl;
    cin.get(),cin.get();
    return 0;
}
```
