# [Codeforces Round #578 (Div. 2)-D. White Lines-差分前缀和](https://codeforces.com/contest/1200)

------

![](H:\GitHub\Algorithm\Codeforces\Problem - D - Codeforces - codeforces.com.png)

## 【Problem Description】

```cpp
给你一个字符矩阵，其中B表示黑色，W表示白色。你可以任意选择一个格子（i，j),将（i，j)~(i+k-1,j+k-1)的矩形
区域内的格子全部刷为白色。然后问最多可以有多少个行列满足一整行是白色或者一整列是白色。
```

## 【Solution】

```cpp
思路是，对于某一行（列）来说，若要使这一行（列）对答案的贡献加1，有哪些格子可以选择。若能找到这些格子，将这
些格子的值加1。对所有行和列都做完之后，某个值最大的格子就是要选的格子，其中的值即最终的答案。

以下对行来举例：
	若要求对第i行来说，有哪些格子可以使其对答案贡献加1。可以求出此行中最左端黑格子的位置l，最右端黑格子的
	位置r。
	1、若第i行没有黑格子，答案直接加1即可。
	2、若第i行中，r-l+1>k,则此行不可能对答案有贡献。跳过此行。
	3、否则，可选则格子一定在（i-k+1,r-k+1)~(i,l)矩形之内。那么将这些格子的数目全加1就行了。
	若直接遍历需要O(n^2)的复杂度。所以可以考虑差分来做。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-08-15 17:06:01 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-08-15 18:53:11
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
    freopen("input.in","r",stdin);
    freopen("output.out","w",stdout);
#endif
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,k;
    while(cin>>n>>k){
        char a[n+5][n+5];
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                cin>>a[i][j];
            }
        }
        int sum[n+5][n+5],ans=0;
        memset(sum,0,sizeof(sum));
        for(int i=1;i<=n;i++){ //考虑行的贡献
            int l=-1,r=-1;
            for(int j=1;j<=n;j++){
                if(a[i][j]=='B'){
                    if(l==-1) l=j;r=j; //记录此行最左端，和最右端的黑格子的位置
                }
            }
            if(l==-1){ans++;continue;} //若不存在黑格子，答案直接加1
            if(r-l+1>k) continue; //若黑格子间隔大于k，则不可能对答案有贡献
            int rs=max(i-k+1,1LL),re=i; //找到起点形成的矩形区域
            int cs=max(r-k+1,1LL),ce=l;
            sum[rs][cs]++,sum[re+1][cs]--; //差分标记
            sum[rs][ce+1]--,sum[re+1][ce+1]++;
        }
        for(int j=1;j<=n;j++){ //考虑列对答案的贡献,其他同上
            int l=-1,r=-1;
            for(int i=1;i<=n;i++){
                if(a[i][j]=='B'){
                    if(l==-1) l=i;r=i;
                }
            }
            if(l==-1) {ans++;continue;}
            if(r-l+1>k) continue;
            int cs=max(j-k+1,1LL),ce=j;
            int rs=max(r-k+1,1LL),re=l;
            sum[rs][cs]++,sum[re+1][cs]--;
            sum[rs][ce+1]--,sum[re+1][ce+1]++;
        }
        int tmp=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                sum[i][j]+=sum[i-1][j]+sum[i][j-1]-sum[i-1][j-1]; //差分求和
                tmp=max(tmp,sum[i][j]);
            }
        }
        cout<<ans+tmp<<endl;
    }
    cin.get(),cin.get();
    return 0;
}
```
