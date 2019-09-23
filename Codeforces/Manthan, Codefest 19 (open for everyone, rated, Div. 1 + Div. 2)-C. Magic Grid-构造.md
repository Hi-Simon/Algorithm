# [Manthan, Codefest 19 (open for everyone, rated, Div. 1 + Div. 2)-C. Magic Grid-构造](https://codeforces.com/contest/1208)

![](H:\GitHub\Algorithm\Codeforces\Problem - C - Codeforces.png)

------



## 【Problem Description】

​		给你一个$n$，构造一个$n\times n$的矩阵，使其满足任意一行，或一列的异或值相同。保证$n$能被$4$整除。

## 【Solution】

​		可以发现，从$0$开始，每$4$个连续数的异或值为$0$，所以可以很容易使得每一行的异或值等于$0$。

​		列向可以发现，从$0$开始，公差为$4$的，每$4$个数的异或值也为$0$。即$0\oplus4\oplus8\oplus12=0$。所以满足条件的矩阵形如：
$$
\left[\begin{matrix}
0&1&2&3&32&33&34&35&\\
4&5&6&7&36&37&38&39&\\
8&9&10&11&40&41&42&43&\\
12&13&14&15&44&45&46&47&\\
16&17&18&19&48&49&50&51&\\
20&21&22&23&52&53&54&55&\\
24&25&26&27&56&57&58&59&\\
28&29&30&31&60&61&62&63&\\
\end{matrix}\right]
$$


------

## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-26 18:38:05 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-26 19:02:53
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 2005
int a[maxn][maxn];
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;cin>>n;
    for(int i=0;i<n;i++) a[0][i]=i/4*n*4+i%4;
    for(int i=1;i<n;i++){
        for(int j=0;j<n;j++){
            a[i][j]=a[i-1][j]+4;
        }
    }
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            cout<<a[i][j]<<' ';
        }
        cout<<endl;
    }
#ifndef ONLINE_JUDGE
    system("pause");
#endif
    return 0;
}
```
