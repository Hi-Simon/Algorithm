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

- 单调栈应用

  - 最基础的应用就是给定一组数，针对每个数，寻找它和它右边第一个比它大的数之间有多少个数。

  - 给定一序列，寻找某一子序列，使得子序列中的最小值乘以子序列的长度最大。

  - 给定一序列，寻找某一子序列，使得子序列中的最小值乘以子序列所有元素和最大。
  - 给定一序列，在限定每个字母出现次数的情况下，求其字典序最小的$k$长子序列。可求后缀和，当一个字母出栈时，判断此后位置当前字母的个数是否满足限制条件，若满足出栈，否则不出栈。

```cpp
for(int i=0;i<s.size();i++){
    int t=i;
    while(Stack.size()&&s[i]<s[Stack.back()]/*当前值小于栈顶元素*/&&num[s[Stack.back()]-'a'][i]/*原串的每个字母的后缀和*/>=ans[s[Stack.back()]-'a'+1]+1/*后缀中此字母的出现次数能够满足限制条件*/&&ans[s[i]-'a'+1]/*每个字母限制出现的次数，要保证当前字母还有*/){
        t=Stack.back();
        Stack.pop_back();
        ans[s[t]-'a'+1]++;
    }
    if(ans[s[i]-'a'+1]) Stack.push_back(i),ans[s[i]-'a'+1]--;
}
```