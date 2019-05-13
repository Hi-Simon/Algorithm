## 图论-LCA个人总结

### Tarjan算法求LCA

```cpp
int Set[maxn],dis[maxn],qhead[maxn],qcnt=0;
node qg[maxm];
pair<int,int>ans[maxn];
int Find(int x){
    return x==Set[x]?x:Find(Set[x]);
}
void Tarjan(int u){
    for(int i=head[u];~i;i=g[i].next){
        int &v=g[i].v,&w=g[i].w;
        if(vis[v]) continue;
        dis[v]=dis[u]+w; //v到根节点的长度
        Tarjan(v);
        Set[v]=u; //
    }
    for(int i=qhead[u];~i;i=qg[i].next){
        int &v=qg[i].v,&w=qg[i].w;
        if(!vis[v]) continue;
        ans[w].first=Find(v); //lca
        ans[w].second=dis[u]+dis[v]-2*dis[ans[w].first]; //两点之间的距离
    }
    vis[u]=1; //放最后，使得每个查询都只查询一次
}
```

### 倍增法求LCA

```cpp
int dep[maxn],fa[maxn][30],
void dfs(int u,int p,int d=1){
    dep[u]=d; //每个节点所在的深度
    for(int i=head[u];~i;i=g[i].next){
        int &v=g[i].v,&w=g[i].w;
        if(v==p) continue;
        dfs(v,u,d+1);
        fa[v][0]=u; //v的父节点
    } 
}
int lca(int u,int v){
    if(dep[u]<dep[v]) swap(u,v);
    while(dep[u]>dep[v]){ 
        for(int i=20;i>=0;i--){
            if(dep[fa[u][i]]<dep[v]) continue;
            u=fa[u][i];
        }
    }
    if(u==v) return u;
    for(int i=20;i>=0;i--){
        if(fa[u][i]==fa[v][i]) continue;
        u=fa[u][i],v=fa[v][i];
    }
    return fa[u][0];
}

for(int i=1;(1<<i)<n;i++){
	for(int j=1;j<=n;j++){
    	fa[j][i]=fa[fa[j][i-1]][i-1];
    }
}
```

