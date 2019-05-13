# [Codeforces Round #551 (Div. 2)-D. Serval and Rooted Tree-树上dp-逆思维](https://codeforces.com/contest/1153/problem/D)

## 【Description】

```cpp
Now Serval is a junior high school student in Japari Middle School, and he is still 
thrilled on math as before.

As a talented boy in mathematics, he likes to play with numbers. This time, he wants to 
play with numbers on a rooted tree.

A tree is a connected graph without cycles. A rooted tree has a special vertex called the 
root. A parent of a node v is the last different from v vertex on the path from the root 
to the vertex v. Children of vertex v are all nodes for which v is the parent. A vertex 
is a leaf if it has no children.

The rooted tree Serval owns has n nodes, node 1 is the root. Serval will write some 
numbers into all nodes of the tree. However, there are some restrictions. Each of the 
nodes except leaves has an operation max or min written in it, indicating that the number 
in this node should be equal to the maximum or minimum of all the numbers in its sons, 
respectively.

Assume that there are k leaves in the tree. Serval wants to put integers 1,2,…,k to the k 
leaves (each number should be used exactly once). He loves large numbers, so he wants to 
maximize the number in the root. As his best friend, can you help him?
```

## 【Input】

```cpp
The first line contains an integer n (2≤n≤3⋅105), the size of the tree.

The second line contains n integers, the i-th of them represents the operation in the 
node i. 0 represents min and 1 represents max. If the node is a leaf, there is still a 
number of 0 or 1, but you can ignore it.

The third line contains n−1 integers f2,f3,…,fn (1≤fi≤i−1), where fi represents the 
parent of the node i.
```

## 【Output】

```cpp
Output one integer — the maximum possible number in the root of the tree.
```

------



## 【Examples】 

#### Sample Input

```cpp
6
1 0 1 1 0 1
1 2 2 2 2
```

#### Sample Output

```cpp
1
```
#### Sample Input

```cpp
5
1 0 1 0 1
1 1 1 1
```

#### Sample Output

```cpp
4
```
#### Sample Input

```cpp
8
1 0 0 1 0 1 1 0
1 1 2 2 3 3 3
```

#### Sample Output

```cpp
4
```
#### Sample Input

```cpp
9
1 1 0 0 1 0 1 0 1
1 1 2 2 3 3 4 4
```

#### Sample Output

```cpp
5
```
------



## 【Problem Description】

```cpp
给你一棵树，树上每个节点属性0或1，0表示，当前节点的权值为其所有儿子节点的最小值，1则相反，表示当前节点的权
值为其所有儿子节点的最大值。
初始只有叶子节点有权值，假设叶子节点有k个，则他们的权值范围在[1,k]之间。

问如何安排叶子节点的权值使得根节点1的权值最大。
```

## 【Solution】

```cpp
如下图：
	对于2号节点来说，它的属性是min
	它能得到的最大值为 总的叶子节点个数-2号节点的儿子个数+1=4
    同理对于3号节点来说
    它能得到的最大值为 3
    所以根节点=max(4,3)=4
    
即对于2，3这种叶子节点的直接父节点来说，如果属性值是min，则按排其叶子节点的权值的时候，不用考虑其他叶子节
点，对其叶子节点从大到小安排权值。
例如2号节点的两个叶子节点安排为5，4
例如3号节点的三个叶子节点安排为5，4，3

所以定义dp[i]表示i号节点中的权值的排名： 叶子节点数为5，5排名第一，4排名第二，etc..

dp[2]=2,dp[3]=3;

若节点属性为min则dp[i]加上所有儿子节点v的dp[v]
否则就等于所有儿子节点的最大值
```

------

![](https://codeforces.com/predownloaded/a9/d3/a9d38129b0d0f7c52487553b4c4f180878a5d920.png)

## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-04-14 21:21:23 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-04-14 21:54:18
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 300005
int a[maxn],du[maxn],dp[maxn],leaves=0;
vector<int>g[maxn];
void dfs(int u,int p){
    if(u>1&&du[u]==1){
        dp[u]=1;
        return ;
    }
    if(a[u]) dp[u]=INF;
    else dp[u]=0;
    for(auto v:g[u]){
        if(v==p) continue;
        dfs(v,u);
        if(a[u]) dp[u]=min(dp[u],dp[v]);//求排名的最大值
        else dp[u]+=dp[v]; //儿子节点个数，即权值的排名
    }
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;scanf("%lld",&n);
    for(int i=1;i<=n;i++) scanf("%lld",a+i);
    for(int i=2;i<=n;i++){
        int x;scanf("%lld",&x);
        g[x].push_back(i);
        du[x]++;du[i]++;
    }
    for(int i=2;i<=n;i++) if(du[i]==1) leaves++;//总叶子节点个数
    dfs(1,-1);
    printf("%lld\n",leaves-dp[1]+1);//排名转化为 实际值
    cin.get(),cin.get();
    return 0;
}
```
