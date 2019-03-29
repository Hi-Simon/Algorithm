# [BZOJ-3811-玛里苟斯-线性基-任意组合的异或值为x,求x^k的期望](https://www.lydsy.com/JudgeOnline/problem.php?id=3811) 



## 【Description】

```cpp
魔法之龙玛里苟斯最近在为加基森拍卖师的削弱而感到伤心，于是他想了一道数学题。
S 是一个可重集合，S={a1,a2,…,an}。等概率随机取 S 的一个子集 A={ai1,…,aim}。计算出 A 中所有元素的异或 
x, 求 x^k 的期望。
```

## 【Input】

```cpp
第一行两个正整数 n, k。
以下 n 行每行一个整数，表示 ai。
```

## 【Output】

```cpp
如果结果是整数，直接输出。如果结果是小数（显然这个小数是有限的），输出精确值（末尾不加多余的 0）。
```

------



## 【Examples】 

#### Sample Input

```cpp
4 2
0
1
2
3
```

#### Sample Output

```cpp
3.5
```

------



## 【Problem Description】

```cpp
无
```

## 【Solution】

```cpp
求解时，我们考虑每一个二进制位对答案的贡献。
因为k=1，2，3，4，5，所以我们可以分类讨论。
	k==1时，我们将n个数所求得的线性基全部‘或’起来得ans，那么答案就是ans*0.5
为什么呢？
```

假设我们得到$ans=5$,二进制就是$101$，那么$5\times 0.5=(1\times 2^2+0\times2^1+1\times2^0)\times 0.5=0.5\times 2^2+0.5\times 2^0$
```cpp
会发现，‘或’ 之后，二进制位中为0的地方对答案没有贡献，意义就是，对于线性基的每一个基向量，写成二进制后，
如果对于某一位，所有的基向量这一位都是0，则在最终的答案中，这一位也一定是0。(因为无论怎么异或这一位上的值都
不会变。)

如果对于某一位，存在一个基向量这一位是1，则在最终的答案中，这一位是1的概率是0.5。(因为这一位上有奇数个1的
概)率是0.5。所以上式相当于对于每一个二进制位求它 为1的期望是多少，那么答案就是将所有位的期望相加。
```

```cpp
	k==2时，思路上差不多。ans意义与上面相同，wi表示二进制中第i位的值为wi
```

$$
ans=w_2\cdot2^2+w_1\cdot2^1+w_0\cdot2^0,则ans^2=(w_2\cdot2^2+w_1\cdot 2^1+w_0\cdot 2^0)\cdot(w_2\cdot 2^2+w_1\cdot 2^1+w_0\cdot 2^0)
=\\w_0\cdot w_0\cdot 2^{0+0}+w_0\cdot w_1\cdot 2^{0+1}+w_0\cdot w_2\cdot 2^{0+2}+w_1\cdot w_0\cdot 2^{1+0}+w_1\cdot w_1\cdot 2^{1+1}+w_1\cdot w_2\cdot 2^{1+2}+\\
w_2\cdot w_0\cdot 2^{2+0}+w_2\cdot w_1\cdot 2^{2+1}+w_2\cdot w_2\cdot 2^{2+2}\\
由上面的式子可以得出，对于w_i\cdot w_j\cdot 2^{i+j},若要第i+j位为1，则w_i,w_j都必须为1\\
$$

```cpp
第i+j位为1的概率可以分为两种情况：
1、对于每一个基向量，wi==wj,则最终答案中第i+j位为1的概率为0.5(因为若异或之后对于第i位，它是1的概率是
0.5，而对于每一个基向量，wj==wi,所以第j位的值一定与第i位相同，相当于它是0或1的概率是1，最终第i+j位的概率
就是0.5,可以理解为它们是相互关联的)
2、若存在一个基向量，wi!=wj，则最终答案中第i+j位为1的概率为0.25(因为对于第i,j位，异或之后它们是1的概率
都是0.5，所以最终第i+j位是1的概率就是0.5*0.5=0.25，可以理解为它们是相互独立的)
所以最终就是O(m^2)枚举每一位(m是二进制的位数)，对于第i+j位求它的值是1的期望，最终答案就是所有的期望之和。
```

```cpp
	k>=3时，因为答案小于2^63次方，所以x^k<2^63,所以x<2^21，那么就可以直接dfs暴力求解了。因为求解过程
中数据可能溢出，所以做一些特殊处理。具体看代码。
```



------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-03-03 12:40:14 
 * @Last Modified by:   Simon 
 * @Last Modified time: 2019-03-03 12:40:14 
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
unsigned int a[maxn],cnt=0;
void insert(int val){//线性基的插入
    for(int i=63;i>=0;i--){
        if(val&(1ULL<<i)){
            if(!a[i]){a[i]=val;cnt=max(cnt,(unsigned int)i);break;} //得到每个基向量
            val^=a[i];
        }
    }
}
void solve1(int n){//k==1
    unsigned int ans=0;
    for(int i=0;i<=cnt;i++) ans|=a[i];
    printf("%llu",ans>>1ULL);
    puts((ans&1)?".5":"");
}
void solve2(int n){//k==2
    unsigned int ret=0,tot=0,ans=0;
    for(int i=0;i<=cnt;i++) ret|=a[i];
    for(int i=0;i<32;i++){ //O(m^2)枚举
        for(int j=0;j<32;j++){
            if((ret & (1ULL << i)) && (ret & (1ULL << j))){ //保证两个位置上都是1,否则最终第i+j位上不可能出现1
                bool flag = 0;
                for(int k=0;k<=cnt;k++) if((a[k]>>i&1)^(a[k]>>j&1)) {flag=1;break;}//存在一个基向量，wi!=wj
                if(i+j-1-flag<0) tot++; else ans+=1ULL<<(i+j-1ULL-flag);//最终答案就是所有位置上的期望之和
            }
        }
    }
    ans+=tot>>1ULL;printf("%llu",ans);
    puts((tot&1ULL)?".5":"");
}
unsigned int fpow(int a,int b){//快速幂
    unsigned int ans=1;
    while(b){
        if(b&1ULL) ans*=a;
        a*=a;b>>=1ULL;
    }
    return ans;
}
unsigned int ans=0,tot=0,rhl,sz;
void dfs(unsigned int x,unsigned int s,unsigned int k){ //dfs暴力迭代
    if (x > cnt) {
        unsigned int res=fpow(s,k-1);
        if (res < rhl) { //这部分都是细节处理，防止中间数据溢出
            res *= s, tot += res;
            if (tot >= rhl) ans += tot / rhl, tot %= rhl;
        }
        else {
            ans += res / rhl * s, res %= rhl, res *= s, tot += res;
            if (tot >= rhl) ans += tot / rhl, tot %= rhl;
        }
        return;
    }
    dfs(x + 1, s,k), dfs(x + 1, s ^ a[x],k);
    return;
}
void solve3(int k){ //k>=3
    rhl=1<<(cnt+1); dfs(0,0,k);
    printf("%llu",ans); puts(tot?".5":"");
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    unsigned int n,k;scanf("%llu%llu",&n,&k);
    for(int i=0;i<n;i++){
        unsigned int x;scanf("%llu",&x);insert(x);
    }
    if(k==1) solve1(n);
    else if(k==2) solve2(n);
    else solve3(k);
    cin.get(),cin.get();
    return 0;
}
```
