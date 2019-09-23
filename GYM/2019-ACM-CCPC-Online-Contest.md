# 2019-ACM-CCPC-Online-Contest

## 1、[\^&\^](http://acm.hdu.edu.cn/showproblem.php?pid=6702)

### 题意：

​		求一个最小的正整数$C$，使得$(A\oplus C) \&(B\oplus C)$最小。

### 思路:

​		对于$A,B$来说，对于他们的二进制的第$i$位，如果其中一个是$0$，则$A_i\&B_i=0$，所以只要找所有满足$A_i=1,B_i=1$的$i$，将$C$的第$i$位置$1$就行了。所以答案就是$A\&B$。**注意题目要求正整数。**

### 代码：

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-23 19:09:47 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-23 19:10:39
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
    int T;cin>>T;
    while(T--){
        int a,b;cin>>a>>b;
        int ans=(a&b);
        cout<<(ans?ans:1)<<endl;
    }
#ifndef ONLINE_JUDGE
    system("pause");
#endif
    return 0;
}
```

---

## 2、[array](http://acm.hdu.edu.cn/showproblem.php?pid=6703)

### 题意：

### 思路：

​		

### 代码：

```cpp

```



---

## 3、[K-th occurrence](http://acm.hdu.edu.cn/showproblem.php?pid=6704)

### 题意：

### 思路：

​		后缀数组+$st$表+主席树+二分

### 代码：

```cpp

```

---



## 4、[path](http://acm.hdu.edu.cn/showproblem.php?pid=6705)

### 题意：

​		给你一个有向带权图，定义一条路径的值为所有你经过的边权的和，你可以经过任意一条边任意多的次数，问第$k$小的路径长度是多少？

### 思路：

​		优先级队列

​		听说是一个很套路的解法？那就记住吧，理解也只能感性的理解一下了。。

​		初始，将每个点为起点所连接的最短边放入优先级队列中，从队列顶端开始，第$i$次出队列，就是第$i$小的路径。根据第$i$小的路径转移出两种路径状态（假设第$i$小的路径最后走过的边为$u-v$）：

​		$1$、第$i$小的路径加上从$v$出发的最短路径

​		$2$、最后走过的边由原来的$u-v$，变为$u-v'$，即从$u$节点出发的第一个比$u-v$边权大的一条边。

### 代码：

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-29 13:13:45 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-29 14:42:14
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 50005
struct node{
    int u,v,w,rank;
    node(){}
    node(int u,int v,int w,int rank):u(u),v(v),w(w),rank(rank){}
    bool operator <(const node&a)const{
        return w>a.w;
    }
};
int a[maxn];
struct pi{
    int u,v,w;
    pi(){}
    pi(int u,int v,int w):u(u),v(v),w(w){}
    bool operator <(const pi&a) const{
        return w<a.w;
    }
};
vector<pi>g[maxn];
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;cin>>T;
    while(T--){
        int n,m,qq;cin>>n>>m>>qq;
        priority_queue<node>q;
        for(int i=1;i<=m;i++){
            int u,v,w;
            cin>>u>>v>>w;
            g[u].push_back({u,v,w});
        }
        for(int i=1;i<=n;i++) sort(g[i].begin(),g[i].end()); //按边权从小到大排序
        for(int i=1;i<=n;i++) if(g[i].size()) q.push({g[i][0].u,g[i][0].v,g[i][0].w,0}); //初始将所有点的最短出边入队列
        int Max=0; for(int i=1;i<=qq;i++) cin>>a[i],Max=max(Max,a[i]); //最大要算到第Max小的路径
        vector<int>ans;
        for(int i=1;i<=Max;i++){
            node now=q.top();q.pop();
            ans.push_back(now.w); //第i次出队列的边权长度，就是第i小的路径长度
            if(g[now.v].size()){ //1、从v点出发的最短边
                int u=now.v,v=g[now.v][0].v,w=g[now.v][0].w;
                q.push({u,v,w+now.w,0});
            }
            if(g[now.u].size()>now.rank+1){//2、由u-v转为u-v'
                int u=now.u,v=g[now.u][now.rank+1].v,w=g[now.u][now.rank+1].w;
                q.push({u,v,now.w+w-g[now.u][now.rank].w,now.rank+1});
            }
        }
        for(int i=1;i<=qq;i++) cout<<ans[a[i]-1]<<endl;
        for(int i=0;i<=n;i++) g[i].clear();
    }
#ifndef ONLINE_JUDGE
    system("pause");
#endif
    return 0;
}
```



---

## 5、[huntian oy](http://acm.hdu.edu.cn/showproblem.php?pid=6706)

### 题意：

​		求$f(n,a,b)=\sum_{i=1}^n\sum_{j=1}^igcd(i^a-j^a,i^b-j^b)[gcd(i,j)=1]\%(10^9+7)$。

### 思路：

​		$gcd(a^m-1,a^n-1)=a^{gcd(m,n)}-1$。 

​		推广：若$a>b,\ gcd(a,b)=1$，则有$gcd(a^m-b^m,a^n-b^n)=a^{gcd(n,m)}-b^{gcd(n,m)}$。

​		不知道上面等式的也可以打表看一下，直接能看出来$gcd(i^a-j^a,i^b-j^b)=i-j$。

然后可得：
$$
f(n,a,b)=\sum_{i=1}^n\sum_{j=1}^i(i-j)[gcd(i,j)=1]=\sum_{i=1}^n\sum_{j=1}^ii[gcd(i,j)=1]-\sum_{i=1}^{n}\sum_{j=1}^ij[gcd(i,j)=1]
\\\sum_{i=1}^ni\cdot\varphi(i)-\sum_{i=1}^n\frac{i\cdot\varphi(i)+[i=1]}{2}=\sum_{i=1}^ni\cdot\varphi(i)-\frac{1}{2}\sum_{i=1}^ni\cdot\varphi(i)-\frac{1}{2}
\\=\frac{1}{2}(\sum_{i=1}^ni\cdot \varphi(i)-1)
$$
令$\phi(n)=\sum_{i=1}^ni\cdot \varphi(i),\ g(n)=n\cdot \varphi(n),\ id(n)=n$，由$\sum_{d|n}\varphi(d)=n$可得：
$$
\sum_{d|n}g*id(n)=\sum_{d|n}d\cdot \varphi(d)\cdot\frac{n}{d}=n\cdot\sum_{d|n}\varphi(d)=n^2
\\
$$
所以有：
$$
\frac{n\cdot(n+1)\cdot(2n+1)}{6}=\sum_{i=1}^ni^2=\sum_{i=1}^n\sum_{d|i}d\cdot \varphi(d)\cdot\frac{i}{d}=\sum_{i=1}^{n}i\sum_{d=1}^{\frac{n}{i}}d\cdot\varphi(d)=\sum_{i=1}^ni\cdot\phi(\frac{n}{i})
$$
我们要求的是$\phi(n)$，也就是$i=1$时的值，所以就是：
$$
\phi(n)=\frac{n\cdot(n+1)\cdot(2n+1)}{6}-\sum_{i=2}^ni\cdot\phi(\frac{n}{i})
$$
带回原式中得：
$$
f(n,a,b)=\frac{1}{2}(\phi(n)-1)。
$$

### 代码：

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-05-02 19:14:05 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-23 18:13:42
 */
#include<bits/stdc++.h>
using namespace std;
#define INF 0x3f3f3f3f
#define maxn 1000000
#define Mod 2500005
#define inv2 500000004
const int mod=1e9+7;
int inv6;
struct HashMap//手写Hash
{
    int head[Mod+5],key[Mod],value[Mod],nxt[Mod],tol;
    inline void clear() { tol=0;memset(head,-1,sizeof(head)); }
    HashMap(){clear();}
    inline void insert(int k,int v)
    {
        int idx=k%Mod;
        for(int i=head[idx];~i;i=nxt[i])
        {
            if(key[i]==k)
            {
                value[i]=min(value[i],v);
                return ;
            }
        }
        key[tol]=k;value[tol]=v;nxt[tol]=head[idx];head[idx]=tol++;
    }
    inline int operator [](const int &k) const
    {
        int idx=k%Mod;
        for(int i=head[idx];~i;i=nxt[i])
        {
            if(key[i]==k) return value[i];
        }
        return -1;
    }
}mp;
int prime[maxn],cnt=0;
long long Phi[maxn];
int sum[maxn]; //预处理i*phi(i)前缀和
bool vis[maxn]={1,1};
void Euler(){
    Phi[1]=1;
    for(int i=2;i<maxn;i++){
        if(!vis[i]){
            prime[++cnt]=i;
            Phi[i]=i-1;
        }
        for(int j=1;j<=cnt&&i*prime[j]<maxn;j++){
            vis[i*prime[j]]=1;
            if(i%prime[j]==0){
                Phi[i*prime[j]]=Phi[i]*prime[j];
                break;
            }
            Phi[i*prime[j]]=Phi[i]*(prime[j]-1);
        }
    }
    for(int i=1;i<maxn;i++) sum[i]=(sum[i-1]+i*1LL*Phi[i]%mod)%mod;
}
int fpow(int a,int b){
    int ans=1;
    while(b){
        if(b&1) ans=ans*1LL*a%mod;
        a=a*1LL*a%mod;
        b>>=1;
    }
    return ans;
}
int sum_1(int n){ //sum(1,2,3,……,n)
    n%=mod;
    return 1LL*n*(n+1)%mod*inv2%mod;
}
int sum_2(int n){ //sum(1,4,9,……,n^2)
    n%=mod;
    return 1LL*n*(n+1)%mod*(2*n+1)%mod*inv6%mod;
}
int dfs(int n){ 
    if(n<maxn) return sum[n];
    if(mp[n]!=-1) return mp[n];
    long long sum=0;
    for(int i=2,j;i<=n;i=j+1){// 分块
        j=n/(n/i);
        (sum+=(sum_1(j)-sum_1(i-1))%mod*1LL*dfs(n/i)%mod)%=mod;
    }
    sum=(sum_2(n)-sum)%mod;
    mp.insert(n,sum);
    return sum;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);Euler();inv6=fpow(6,mod-2);
    int T;scanf("%d",&T);
    while(T--){
        int n,a,b;scanf("%d%d%d",&n,&a,&b);
        printf("%lld\n", ((dfs(n) - 1)*1LL * inv2 % mod + mod) % mod);
    }
    cin.get(),cin.get();
    return 0;
}
```



---

## 6、[Shuffle Card](http://acm.hdu.edu.cn/showproblem.php?pid=6707)

### 题意：

​		初始给你一个按序的$[1,n]$的排列。$m$次操作，每次将数$x$移到最前端，问最后这个排列是什么。

### 思路：

​		倒置整个初始排列，每次操作将$x\ push\_back$到数组末端。最后倒序输出，标记一下有没有输出过即可。

### 代码：

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-23 20:18:17 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-23 20:20:44
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 200005
int a[maxn];
bool vis[maxn];
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m;cin>>n>>m;
    for(int i=n;i>=1;i--) cin>>a[i];
    for(int i=1;i<=m;i++){
        int x;cin>>x;
        a[++n]=x;
    }
    for(int i=n;i>=1;i--){
        if(!vis[a[i]]) cout<<a[i]<<' ',vis[a[i]]=1;
    }
#ifndef ONLINE_JUDGE
    system("pause");
#endif
    return 0;
}
```



---

## 7、[Windows Of CCPC](http://acm.hdu.edu.cn/showproblem.php?pid=6708)

### 题意：

​		找规律。

### 思路：

​		找规律。

### 代码：

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-23 20:38:59 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-23 21:26:25
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 2005
char a[maxn][maxn];
Int main(){
#ifndef ONLINE_JUDGE
    //freopen("input.in","r",stdin);
    //freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    a[0][0]=a[0][1]=a[1][1]='C';a[1][0]='P';
    for(int i=1;i<10;i++){
        for(int p=(1<<i),k=0;k<(1<<(i));p++,k++){
            for(int kk=0;kk<(1<<(i));kk++){
                if(a[k][kk]=='C') a[p][kk]='P';
                else a[p][kk]='C';
            }
        }
        for(int p=0;p<(1<<(i));p++){
            for(int q=(1<<i),k=0;k<(1<<(i));k++,q++){
                a[p][q]=a[p][k];
            }
        }
        for(int p=(1<<i),k=0;k<(1<<(i));k++,p++){
            for(int q=(1<<i),kk=0;kk<(1<<(i));kk++,q++){
                a[p][q]=a[k][kk];
            }
        }
    }
    int T;cin>>T;
    while(T--){
        int n;cin>>n;
        for(int i=0;i<(1<<n);i++){
            for(int j=0;j<(1<<n);j++){
                cout<<a[i][j];
            }
            cout<<endl;
        }
    }
#ifndef ONLINE_JUDGE
    system("pause");
#endif
    return 0;
}
```



---

## 8、[Fishing Master](http://acm.hdu.edu.cn/showproblem.php?pid=6709)

### 题意：

​		有$n$条鱼，煮熟每条鱼所花费的时间为$a_i$，抓一条鱼所花费的时间为$k$，问在一次只能煮一条鱼的条件下，煮熟所有的鱼，所花费的最少时间为多少？

### 思路：

​		假设所有煮鱼的时间总和为$sum$，则总时间肯定不小于$sum+k$。即最理想的情况就是，第$1$条鱼需要花费$k$的时间来抓，以后抓的每一条鱼，都在前一条煮熟之前抓到。因此不花费额外的时间。但实际会出现，已经没有鱼可煮了，因此需要花费额外的时间来抓鱼，所以其实就是让这额外抓鱼的时间最少即可。

### 代码：

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-25 14:00:56 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-25 14:11:55
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
    int T;cin>>T;
    while(T--){
        int n,k,ans=0;cin>>n>>k;
        for(int i=1;i<=n;i++) cin>>a[i],ans+=a[i];
        sort(a+1,a+n+1,greater<int>()); //按煮鱼时间从大到小排序，这样可以使得再煮第一条鱼的时候多抓几条鱼。
        int tot=1/*目前的存货*/,num=1/*总共抓了多少条鱼*/;priority_queue<int>q;
        for(int i=1;i<=n;i++){
            tot--;
            if(tot<0){ //无鱼可煮时，需要用最少的时间来抓一条鱼来煮。
                ans+=k-q.top();
                q.pop();tot++,num++;
            } 
            if(a[i]%k!=0) q.push(a[i]%k);
            tot+=a[i]/k;num+=a[i]/k;
            if(num>=n) break; //若抓鱼总数大于等于n则，不可能再花费额外的时间
        }
        cout<<ans+k<<endl;
    }
#ifndef ONLINE_JUDGE
    system("pause");
#endif
    return 0;
}
```



---

## 9、[Kaguya](http://acm.hdu.edu.cn/showproblem.php?pid=6710)

### 题意：

### 思路：

​		概率动态规划+二分图

### 代码：

```cpp

```



---

## 10、[Touma Kazusa's function](http://acm.hdu.edu.cn/showproblem.php?pid=6711)

### 题意：

### 思路：

​		莫比乌斯反演+莫队

### 代码：

```cpp

```

## 11、[sakura](http://acm.hdu.edu.cn/showproblem.php?pid=6712)

### 题意：

### 思路：

​		中国剩余定理+卢卡斯定理

### 代码：

```cpp

```

