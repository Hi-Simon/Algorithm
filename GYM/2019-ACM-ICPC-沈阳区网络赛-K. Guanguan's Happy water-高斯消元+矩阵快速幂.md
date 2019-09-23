#[2019-ACM-ICPC-沈阳区网络赛-K. Guanguan's Happy water-高斯消元+矩阵快速幂](https://nanti.jisuanke.com/t/41411)

![](H:\GitHub\Algorithm\GYM\https___nanti.jisuanke.com_t_41411.png)

------



## 【Problem Description】

已知前$2k$个$f(i)$，且$f(n)=f(n-1)\cdot p(1)+f(n-2)\cdot p(2)+\dots+f(n-k)\cdot p(k)$。求$f(1)+f(2)+\dots+f(n)$。

## 【Solution】

根据题目条件可知
$$
f(k+1)=f(k)\cdot p(1)+f(k-2)\cdot p(2)+\dots+f(1)\cdot p(1)\\
f(k+2)=f(k+1)\cdot p(1)+f(k-1)\cdot p(2)+\dots+f(2)\cdot p(1)\\
\vdots\\
f(2k)=f(2k-1)\cdot p(1)+f(2k-2)\cdot p(2)+\dots+f(k)\cdot p(1)\\
$$
$k$个方程，$k$个未知数，所以可以用高斯消元求得矩阵$P$。则：
$$
\left(\begin{matrix}
f(n)\\
f(n-1)\\
f(n-2)\\
\vdots\\
f(n-k+1)
\end{matrix}\right)=\left(\begin{matrix}
p(1)&p(2)&\dots&p(k-1)&p(k)\\
1&0&\dots&0&0\\
0&1&\dots&0&0\\
\vdots\\
0&0&\dots&1&0
\end{matrix}\right)\cdot \left(\begin{matrix}
f(n-1)\\
f(n-2)\\
f(n-3)\\
\vdots\\
f(n-k)
\end{matrix}\right)
$$
则有$f(n)$的前缀和$sum(n)$：
$$
\left(\begin{matrix}
f(n)\\
f(n-1)\\
f(n-2)\\
\vdots\\
f(n-k+1)\\
sum(n)
\end{matrix}\right)=\left(\begin{matrix}
p(1)&p(2)&\dots&p(k-1)&p(k)&0\\
1&0&\dots&0&0&0\\
0&1&\dots&0&0&0\\
\vdots\\
0&0&\dots&1&0&0\\
p(1)&p(2)&\dots&p(k-1)&p(k)&1
\end{matrix}\right)\cdot \left(\begin{matrix}
f(n-1)\\
f(n-2)\\
f(n-3)\\
\vdots\\
f(n-k)\\
sum(n-1)
\end{matrix}\right)
$$
则通过矩阵快速幂即可求直接得$f(n)$的前缀和。复杂度为高斯消元的复杂度$O(k^3)$加上矩阵快速幂的复杂度$O(k^3\cdot log(n))$。所以总复杂度为$O(T\cdot k^3\cdot log(n))$。约为$4\times 10^8$。

**注意特判$n\le 2\times k$的情况**

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-09-19 20:19:56 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-09-21 20:14:34
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
const int mod=1e9+7;
#define maxn 145
#define maxm 145
/*
Author: Simon
返回自由变量个数，-1表示无解。
若矩阵的秩=增广矩阵的秩=变量个数，则有唯一解
若矩阵的秩=增广矩阵的秩<变量个数，则有无穷多解
若矩阵的秩<增广矩阵的秩,则无解

注：若用于开关问题，则使用注释部分。
复杂度: O(n^3)
*/
const int/*开关问题：int*/ eps = 1e-10;
// int n/*方程个数*/, m/*变量个数*/;
int/*开关问题：int*/ a[maxn][maxn]/*增广矩阵(n*(m+1)),开关问题：a[i][j]表示与j关联的开关为i*/, x[maxn]/*解*/; 

int fpow(int a,int b,int mod){
    int ans=1;a%=mod;
    while(b){
        if(b&1) ans=1LL*ans*a%mod;
        a=1LL*a*a%mod;
        b>>=1;
    }
    return ans;
}
inline int sgn(int x) { return x?1:0; } //若x不接近0，返回1，否则返回0。

int Gauss(int a[maxn][maxn]/*增广矩阵*/, int n/*方程个数*/, int m/*变量个数*/) {
    memset(x, 0, sizeof(x));
    int r = 0/*第r行*/, c = 0/*第c列*/;
    while (r < n && c < m) {/*化为上三角矩阵*/
        int m_r = r;
		for(int i=r+1;i<n;i++) if (abs(a[i][c]) > abs(a[m_r][c])) m_r = i; /*从第r行开始，找出第c列绝对值最大的 */
        if (m_r != r){
			for(int j=c;j<m+1;j++) swap(a[r][j], a[m_r][j]); /*将值最大的放到第r行*/
		}
        if (!sgn(a[r][c])) { /*判断a[r][c]是否为零*/
            a[r][c] = 0; ++c;
            continue;
        }
		for(int i=r+1;i<n;i++){ /*将第c列化为上三角*/
            if (a[i][c]) {
                int/*开关问题：int*/ t = 1LL*a[i][c]%mod*fpow(a[r][c]%mod,mod-2,mod)%mod;/*开关问题：删除*/
                for(int j=c;j<m+1;j++) a[i][j] = (a[i][j]-1LL*a[r][j]%mod * t%mod)%mod/*开关问题：a[i][j]^=a[r][j]*/;
            }
		}
		++r; ++c;
    }
    for(int i=r;i<n;i++) if(sgn(a[i][m])) return -1;/*若xi=0,b!=0则无解*/
	for(int i=m-1;i>=0;i--){/*回代解方程组*/
        int/*开关问题：int*/ s = a[i][m];
		for(int j=i+1;j<m;j++) s = (s-1LL*a[i][j]%mod * x[j] % mod)%mod/*开关问题：s^=(a[i][j]&x[j])*/;
        x[i] = 1LL*s%mod * fpow(a[i][i],mod-2,mod)%mod/*开关问题：x[i]=s*/;
    }
    return 0;/*有唯一解*/
}

//Author: Simon
struct Matrix{ //矩阵类
    int m[maxn][maxn];
    void clear(){
        for(int i=0;i<maxn;i++) for(int j=0;j<maxn;j++) m[i][j]=0;
    }
    Matrix(){
        clear();
    }
    void init(){
        for(int i=0;i<maxn;i++) m[i][i]=1;
    }
    void set(int len){ //构造矩阵，根据题目变化
        for(int i=0;i<len;i++){
            for(int j=i-1;j<=i+1;j++){
                if(j<0||j>=len) continue;
                m[i][j]=1;
            }
        }
    }
    int *operator [](int x){
        return m[x];
    }
};
Matrix mul(Matrix a,Matrix b,int n){
    Matrix c;
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            for(int k=0;k<n;k++){
                c[i][j]=(c[i][j]+1LL*a[i][k]*b[k][j]%mod)%mod;
            }
        }
    }
    return c;
}
Matrix fpow(Matrix a,long long b,int k){
    // printf("b=%lld\n",b);
    Matrix c;c.init();
    while(b){
        if(b&1) c=mul(c,a,k);
        a=mul(a,a,k);
        b>>=1;
    }
    return c;
}

int t[maxn];
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;scanf("%lld",&T);
    while(T--){
		memset(a,0,sizeof(a));
        int k;long long n;scanf("%lld%lld",&k,&n);
        for(int i=1;i<=2*k;i++){
            scanf("%lld",t+i);
        }
        if(n<=2LL*k){ //特判
            long long _ans=0;
			for(int i=0;i<n;i++) _ans=(_ans+t[i])%mod;
			printf("%lld\n",_ans);
			continue;
		}
        for(int i=0;i<k;i++){
            for(int j=0;j<k;j++){
                a[i][j]=t[k+i-j];
            }
        }
        for(int i=0;i<k;i++) a[i][k]=t[i+1+k];
        Gauss(a,k,k); Matrix A;
		for(int i=0;i<k;i++) A[0][i]=x[i];
		for(int i=1;i<k;i++) A[i][i-1]=1;
		A[k][0]=1;A[k][k]=1;
        A=fpow(A,n-k,k+1);
		Matrix B;
		for(int i=0;i<k;i++){
			B[i][0]=t[k-i];
			B[k][0]+=t[i+1];
		}
		A=mul(A,B,k+1);
		printf("%lld\n",A[k][0]);
    }
#ifndef ONLINE_JUDGE
    cout<<endl;system("pause");
#endif
    return 0;
}
```
