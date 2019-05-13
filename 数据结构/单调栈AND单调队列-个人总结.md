## 单调栈AND单调队列-个人总结

### 单调栈

```cpp
int Stack[maxn],lft[maxn],top=0;
int ans=0;a[n+1]=-INF;top=0;
	for(int i=1;i<=n+1;i++){
            if(top==0||a[i]>=a[Stack[top]]){ //维护单调递增区间
                Stack[++top]=i;
                lft[i]=i;
                continue;
            }
            else{
                int t;
                while(top&&a[i]<a[Stack[top]]){
                    t=Stack[top--];
                    ans=max(ans,(i-lft[t])*a[t]);
                }
                Stack[++top]=i; 
                lft[i]=lft[t]; //向左拓展的最大距离，即[lft[t],i]之间的数都大于等于a[i]
            }
        }

/***************************************************************************************/

for(int i=1;i<=n+1;i++){
    int t=i;lft[i]=i;
    while(top&&a[i]<a[Stack[top]]){
        t=Stack[top--];
        ans=max(ans,(i-lft[t])*a[t]);
    }
    Stack[++top]=i;
    lft[i]=lft[t];
}
```

### 单调队列

```cpp
int q[maxn],l=1,r=0;
	for(int i=1;i<=n;i++){
   		while(l<=r&&a[i]<=a[q[r]]) r--; //维护单调递增区间
        q[++r]=i;
        while(l<=r&&i-q[l]>=k) l++; //维护不大于k的区间长度
        if(i-k>=0) cout<<a[q[l]]<<" ";
    }
```



### Others

- 维护区间最小值，需维护单调递增区间，反之，维护单调递减区间

  以维护单调递增区间为例，它表示栈顶的元素为当前的最小值，若新元素小于栈顶元素，则对当前栈顶元素进行处理并出栈，因为新元素加进来以后，栈顶元素肯定不是最小值，所以处理新元素之前的区间。

  