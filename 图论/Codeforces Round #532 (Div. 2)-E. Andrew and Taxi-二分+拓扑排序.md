#  [Codeforces Round #532 (Div. 2)-E. Andrew and Taxi-二分+拓扑排序](http://codeforces.com/contest/1100/problem/E)



## 【Description】

```cpp
Andrew prefers taxi to other means of transport, but recently most taxi drivers have 
been acting inappropriately. In order to earn more money, taxi drivers started to drive 
in circles. Roads in Andrew's city are one-way, and people are not necessary able to 
travel from one part to another, but it pales in comparison to insidious taxi drivers.

The mayor of the city decided to change the direction of certain roads so that the taxi 
drivers wouldn't be able to increase the cost of the trip endlessly. More formally, if 
the taxi driver is on a certain crossroads, they wouldn't be able to reach it again if 
he performs a nonzero trip.

Traffic controllers are needed in order to change the direction the road goes. For 
every road it is known how many traffic controllers are needed to change the direction 
of the road to the opposite one. It is allowed to change the directions of roads one by 
one, meaning that each traffic controller can participate in reversing two or more 
roads.

You need to calculate the minimum number of traffic controllers that you need to hire 
to perform the task and the list of the roads that need to be reversed.
```

## 【Input】

```cpp
The first line contains two integers  n and  m (2<=n<=100000, 1<=m<=100000) — the 
number of crossroads and the number of roads in the city, respectively.

Each of the following  lines contain three integers  ui  ,   vi  and  ci   
(1<=ui,vi<=n, 1<=ci<=1e9, ui!=vi) — the crossroads the road starts at, the crossroads 
the road ends at and the number of traffic controllers required to reverse this road.
```

## 【Output】

```cpp
In the first line output two integers the minimal amount of traffic controllers 
required to complete the task and amount of roads  k which should be reversed. k should 
not be minimized.

In the next line output  integers separated by spaces — numbers of roads, the 
directions of which should be reversed. The roads are numerated from   in the order 
they are written in the input. If there are many solutions, print any of them.
```

------



## 【Examples】 

#### Sample Input

```cpp
5 6
2 1 1
5 2 6
2 3 2
3 4 3
4 5 5
1 5 4
```

#### Sample Output

```cpp
2 2
1 3 
```
#### Sample Input

```cpp
5 7
2 1 5
3 2 3
1 3 3
2 4 1
4 3 5
5 4 1
1 5 3
```

#### Sample Output

```cpp
3 3
3 4 7 
```
------



## 【Problem Description】

```cpp
给你一个有向图，每条边都有个权值，权值代表将这条边反向所要的人力，求一个最小的人力k，使得将权值小于等于k
的某些边反向后形成的有向图没有环。
```

## 【Solution】

```cpp
二分+拓扑排序
二分答案k，将边权值小于等于k的边当作无向图处理，不做出入度统计，对边权值大于k的边建立邻接表，并统计各个
点的入度。然后通过bfs进行拓扑排序来判断是否有环。若无环，则减小k的值。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-02-27 14:31:53 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-02-27 19:48:07
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int In[maxn],top[maxn]; //每个点的入度，每个点的拓扑排序排名
struct node{
    int u,v,w;
}a[maxn];
vector<int>g[maxn]; //邻接表
bool check(int mid,int n,int m){
    for(int i=1;i<=n;i++) g[i].clear();
    memset(In,0,sizeof(In));
    for(int i=1;i<=m;i++){
        if(a[i].w>mid) g[a[i].u].push_back(a[i].v),In[a[i].v]++; //将大于mid的边存储起来，并统计入度
    }
    queue<int>q; int cnt=0; //cnt计数做拓扑排序的排名
    for(int i=1;i<=n;i++){ //为拓扑排序做准备，将入度为0的点入队列,并排名
        if(!In[i]) q.push(i),top[i]=++cnt;
    }
    while(!q.empty()){ //拓扑排序
        int now=q.front();q.pop();
        for(auto v:g[now]){ //将当前点所连的所有边都去掉，相当于入度减一
            In[v]--;
            if(!In[v]) q.push(v),top[v]=++cnt; 
        }
    }
    for(int i=1;i<=n;i++){ //判断是否所有边都已删除
        if(In[i]) return 0;
    }
    return 1;
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n,m;cin>>n>>m;int cnt=0;
    for(int i=1;i<=m;i++){
        cin>>a[i].u>>a[i].v>>a[i].w;
        cnt=max(cnt,a[i].w);
    }
    int left=0,right=cnt,mid;
    while(left<=right){ //二分答案
        mid=(left+right)>>1;
        if(check(mid,n,m)){
            right=mid-1;
        }
        else left=mid+1;
    }
    cout<<left<<' ';
    check(left,n,m);vector<int>ans;//求当前答案的拓扑排序后的排名
    for(int i=1;i<=m;i++){
        if(a[i].w<=left&&top[a[i].u]>top[a[i].v]) ans.push_back(i);
    } //若当前边的连接顺序与排名顺序不相同，则需要改变这条边的方向
    cout<<ans.size()<<endl;
    for(auto v:ans) cout<<v<<' ';
    cout<<endl;
    cin.get(),cin.get();
    return 0;
}
```
