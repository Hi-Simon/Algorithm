## Min25筛-个人总结

###作用：

可以在$O(\frac{n^{\frac{3}{4}}}{logn})​$的时间内求积性函数前缀和。适用条件为“在质数处表达式为多项式，在质数的高次幂处可以快速求值”的积形函数。

### 符号说明

$f(i)$：要求的积形函数。

$P​$：质数集。

$P_k$：第$k$个质数，例如$P_1=2​$。

$Fac_n​$：$n​$的最小质因子。

$m​$：$i\in[1,n],\lfloor\frac{n}{i}\rfloor 的个数​$。

$x_k​$：第$k​$个$\lfloor\frac{n}{i}\rfloor​$。

$g(n,j)​$：满足$i​$是质数**或者**$Fac_i>p_j​$的$f(i)​$的前缀和。即：
$$
g(n,j)=\sum_{i=1}^nf(i)[i\in P\ or \ Fac_i>P_j ]
$$
$S(n,j)$：满足$Fac_i\ge P_j$的$f(i)​$的前缀和。即：
$$
S(n,j)=\sum_{i=1}^nF(i)[Fac_i\ge P_j ]
$$

### 简要步骤：

1、对给定的函数$f(i)$分析，当$i=p$即质数情况下，是否容易求得答案，以及$i=p^c$时是否容易求得答案。若是则可用$Min25$筛。若不是考虑转化为积形函数，一般通过减少限制条件来转化，最后将缺少或增加的答案减去即可。

2、对于每个$x_k=\lfloor\frac{n}{i}\rfloor$，求出$g(k,0)=\sum_{j=1}^{x_k}f(j)$。

3、根据转移方程$g(n,j)=\begin{cases}    g(n,j-1)&P_j^2\gt n\\    g(n,j-1)-f(P_j)[g(\frac{n}{P_j},j-1)-\sum_{i=1}^{j-1}f(P_i)]&P_j^2\le n\end{cases}$，对于每个$x_k=\lfloor\frac{n}{i}\rfloor$求得$g(k,|P|)​$的值。

4、根据转移方程$S(n,j)=g(n,|P|)-\sum_{i=1}^{j-1}f(P_i)+\sum_{k=j}^{P_k^2\le n}\sum_{e=1}^{P_k^{e+1}\le n}S(\frac{n}{P_k^e},k+1)\times f(P_k^e)+f(P_k^{e+1})$求得$S(n,1)$的值。

5、$\sum_{i=1}^nf(i)=S(n,1)+f(1)$。

### 原理：

#### $g(n,j)$的转移方程推导：

​	对于第一种情况，$P_j^2>n​$，一定有$Fac_n< P_j​$。因为满足$Fac_x=P_j​$的最小合数$x=P_j^2​$。所以一定有$g(n,j)=g(n,j-1)​$。

​	对于第二种情况，$P_j^2\le n$，从$P_j$到$P_{j-1}$能产生贡献的值变多，且增加贡献的值的最小质因子一定等于$P_j$。所以将$P_j$从中提取出来，得到$g(n,j)=g(n,j-1)-f(P_j)\cdot X$。考虑$X$的构成。它包含除去$P_j$后的最小质因子仍大于等于$P_{j}$的数$g(\frac{n}{P_j},j-1)$。但多减去了小于$P_j$的质数，所以容斥一下，也就是加上$\sum_{i=1}^{j-1}f(P_i)​$。

假设$n=25，P_j=P_2=3$。

| $i$      | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   | 13   | 14   | 15   | 16   | $\dots$ | 25   |
| -------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ------- | ---- |
| $P_2=3$  |      | 2    | 3    |      | 5    |      | 7    |      |      |      | 11   |      | 13   |      |      |      | $\dots$ | 25   |
| $P_1= 2$ |      | 2    | 3    |      | 5    |      | 7    |      | 9    |      | 11   |      | 13   |      | 15   |      | $\dots$ | 25   |

所以总结起来就是：
$$
g(n,j)=\begin{cases}    g(n,j-1)&P_j^2\gt n\\    g(n,j-1)-f(P_j)[g(\frac{n}{P_j},j-1)-\sum_{i=1}^{j-1}f(P_i)]&P_j^2\le n\end{cases}
$$
最后求到$g(n,|P|)​$即可，可以用一维数组转移。

即最后求得的就是对于所有$x_k=\lfloor\frac{n}{i}\rfloor ，g(x_k,|P|)=\sum_{i=1}^{x_k}f(i)[i\in P]$。

**另：**观察$g(x_k,|P|)$，若$i=1$，则$x_k=n$，则求得$g(n,|P|)$，即$i\in[1,n],i\in P$的$f(i)$的前缀和。类似的，若$i=2$，则$x_k=\lfloor\frac{n}{2}\rfloor$，即$i\in[1,\lfloor\frac{n}{2}\rfloor]，i\in P$的$f(i)$的前缀和，所以可以发现$g$函数中求得的是将$n$分块后的素数的前缀和，因此可以用于求类似$\sum_{d=1}^nd[d\in P]\sum_{i=1}^{\frac{n}{d}}f(i)$的积形函数前缀和中。$g(n,|P|)-g(\lfloor\frac{n}{2}\rfloor,|P|)​$就是一段分块区间和。

### $S(n,j)$的转移方程推导：

将$S(n,j)​$分为质数和部分和合数和部分。

质数部分就是：$$g(n,j)-\sum_{i=1}^{j-1}f(P_i)$$，即减去最小质因子小于$P_j​$的部分即可。

合数部分就是：枚举这个合数的最小质因子及其出现次数，然后直接乘即可。

所以：
$$
S(n,j)=g(n,|P|)-\sum_{i=1}^{j-1}f(P_i)+\sum_{k=j}^{P_k^2\le n}\sum_{e=1}^{P_k^{e+1}\le n}S(\frac{n}{P_k^e},k+1)\times f(P_k^e)+f(P_k^{e+1})
$$

### 从入门到放弃题单：

1、[UESTC-2204](https://acm.uestc.edu.cn/problem/min25/description/)

2、[LOJ-6053](https://loj.ac/problem/6053)

3、[SPOJ-20173](https://www.luogu.org/problem/SP20173)

4、[SPOJ-20174](https://www.luogu.org/problem/SP20174)

5、[SPOJ-34096-](https://www.luogu.org/problem/SP34096)

6、[UESTC-2213](https://acm.uestc.edu.cn/problem/min25-min25/description/)

7、[HDU-6066](http://acm.hdu.edu.cn/showproblem.php?pid=6606)

8、[HDU-6537](<http://acm.hdu.edu.cn/showproblem.php?pid=6537>)