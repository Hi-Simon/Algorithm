#[Educational Codeforces Round 71 (Rated for Div. 2)-E. XOR Guessing-交互题](https://codeforces.com/contest/1207)

![](H:\GitHub\Algorithm\Codeforces\https___codeforces.com_contest_1207_problem_E.png)

------



## 【Problem Description】

​		总共两次询问，每次询问给出$100$个不同的数，评测系统对于每次询问，随机从$100$个数中选择一个数$a$，返回$x\oplus a$。让你通过两次返回的值猜出$x$值是多少。要求两次询问的$200$个数互不相同，且题目保证$x$值固定不变。

## 【Solution】

​		题目要求所有询问数据，即$x$的值在$[0,2^{14}-1]$范围内，且只能询问两次，根据异或的性质，$0$异或任何数都不改变。所以可以分两次得到答案，即第一次先确定$x$二进制中的高$7$位，也就是第一次询问时，所有询问的数的高$7$位全为$0$，这样保证评测系统选任何数异或后返回的值的高$7$位一定与$x$的高$7$位相同，同理，第二次只要保证所有询问的数的低$7$位全为$0$，最终将两次得到的值合并即可。

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-27 20:36:36 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-27 21:57:17
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 200005
int a[maxn];
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout<<"? ";
    for(int i=1;i<=100;i++) cout<<i<<" "; //第一次保证高7位为0
    cout<<endl;
    int ans;cin>>ans;
    ans|=127; //低7位全置1
    cout<<"? ";
    for(int i=1;i<=100;i++) cout<<(i<<7)<<' ';//第二次保证低7为0
    cout<<endl;
    int tmp;cin>>tmp;
    (ans&=(tmp|(127<<7))); //合并第二次的低7位
    cout<<"! "<<ans<<endl;
#ifndef ONLINE_JUDGE
    cout<<endl;system("pause");
#endif
    return 0;
}
```
