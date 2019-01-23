# [Codeforces Round #495 (Div. 2)](http://codeforces.com/contest/1004) - D. Sonya and Matrix 

[TOC]



## ``Description ``

> Since Sonya has just learned the basics of matrices, she decided to play with them a little bit.
>
> Sonya imagined a new type of matrices that she called rhombic matrices.  These matrices have exactly one zero, while all other cells have the  Manhattan distance to the cell containing the zero. The cells with equal  numbers have the form of a rhombus, that is why Sonya called this type  so.
>
> **The Manhattan distance between two cells (x1，y1）and (x2, y2) is defined as |x1−x2|+|y1−y2|.** **For example, the Manhattan distance between the cells (5,2) and (7,1) equals to |5−7|+|2−1|=3**
>
> ![img](http://codeforces.com/predownloaded/02/8b/028b8a58b3fdf8341f001ac18671b12f10afdbeb.png) 
>
> Note that rhombic matrices are uniquely defined by n, m, and the coordinates of the cell containing the zero. 
>
> She drew a n×m rhombic matrix. She believes that you can not recreate the matrix if she gives you only the elements of this matrix in some arbitrary order (i.e., the sequence of n⋅m numbers). **Note that Sonya will not give you n and m**, so only the sequence of numbers in this matrix will be at your disposal. 
>
> **Write a program that finds such an n×m rhombic matrix whose elements are the same as the elements in the sequence in some order.** 

## `Input`

> The first line contains a single integer t (1≤t≤10^6) — the number of cells in the matrix. 
>
> **The second line contains t integers a1,a2,…,at (0≤ai<t) — the values in the cells in arbitrary order.** 

## `Output`

> In the first line, print two positive integers n and m (n×m=t) — the size of the matrix. 
>
> In the second line, print two integers x and y (1≤x≤n, 1≤y≤m) — the row number and the column number where the cell with 0 is located. 
>
> **If there are multiple possible answers, print any of them. If there is no solution, print the single integer −1.** 

## `Examples` 

> ### `Input`

> ```
> 20
> 1 0 2 3 5 3 2 1 3 2 3 1 4 2 1 4 2 3 2 4
> ```

> ###Ouput

> ```
> 4 5
> 2 2
> ```

> ###Input

> ```
> 18
> 2 2 3 2 4 3 3 3 0 2 4 2 1 3 2 1 1 1
> ```

> ###Output

> ```
> 3 6
> 2 3
> ```

> ###Input

> ```
> 6
> 2 1 0 2 1 2
> ```

> ### Output

> ```
> -1
> ```

## `题意`

> 定义一种新的矩阵——菱形矩阵，矩阵内各元素的值为：与“0“元素的”距离“。
>
> 比如两点 (x1，y1）and (x2, y2) 的距离定义为 |x1−x2|+|y1−y2|.  (3,4) and (1,1) 等于 |3−1|+|4−1|=5。（3，4）为”0“元素位置。
>
> **题意是给你一串长度为 t (t=n*m) 的数串，让你求出一种满足菱形矩阵性质的行数n，列数m，以及此时0元素的位置x，y。**

## `题解`

> 令t=n*m. (x, y)为0元素所在位置。
>
> **令b为与0元素相距最远的距离值。**
>
> **x可以总结为所给元素中，从小到大排序，第一个其个数不为4的倍数的数。**
>
> 我们假设![img](https://codeforces.com/predownloaded/37/13/3713f4fa992dbb9f0038ce991c1c404ec991c093.png) ,那么b=(n-x)+(m-y)。(题目说只要输出一种符合条件的即可，所以我们可以假设x,y的范围)
>
> 那么y=n+m−b−x。
>
> **又根据b=(n-x)+(m-y)，我们可以知道b等于所给数串中最大的数。**
>
> **此时，b，x已知，再枚举所有可能的n，m值，求出相应的y。**
>
> 最后判断是否存在一组n,m,x,y符合条件。

## `代码`

```
#include <iostream>
#include <cmath>
using namespace std;
int a[1000005];   //存储给数串
int num[1000005]; //计数，统计所给各个数字的个数
int t;
int nu[1000005];                   //备用计数
bool c(int n, int m, int x, int y) //check函数，判断当前，n,m,x,y是否满足条件。这里用各个数的个数来判断是否满足条件。
{
    for (int i = 0; i <= t; i++)
        nu[i] = num[i];
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            int c = abs(i - x) + abs(j - y);

            nu[c]--;

            if (nu[c] == -1) //若不满足条件，则此n,m,x,y形成的菱形矩阵中数字个数不与所给数串中各个数字个数完全相等
            {
                return 0;
            }
        }
    }
    return 1;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);

    cin >> t;
    int b = 0;
    for (int i = 1; i <= t; i++)
    {
        cin >> a[i];
        num[a[i]]++;      //计数
        b = max(b, a[i]); //找到最大的数，即b的值
    }

    int x = 1;
    for (int i = 1; i <= t; i++)
    {
        if (num[i] && num[i] % 4) //找到第一个个数不是4的倍数的数，即x的值
        {
            x = i;
            break;
        }
    }
    int n = 2 * x - 2; //做了小优化，n也可以直接从1开始枚举
    int y = 0, m;
    for (int i = n; i <= t; i++)
    {
        if (!i)
            continue;
        if (t % i == 0)
        {
            m = t / i;         //枚举n的值，求出相应的m
            y = i + m - b - x; //由，n,m,b,x可求出y的值
            if (c(i, m, x, y)) //判断当前n,m,x,y是否满足条件
            {
                cout << i << ' ' << m << endl;
                cout << x << ' ' << y << endl;
                return 0;
            }
        }
    }
    cout << -1 << endl; //若找不到满足条件的一组数，则不存在。
    return 0;
}
```

