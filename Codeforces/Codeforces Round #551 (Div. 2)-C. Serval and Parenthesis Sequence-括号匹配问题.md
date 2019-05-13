# [Codeforces Round #551 (Div. 2)-C. Serval and Parenthesis Sequence-括号匹配问题](https://codeforces.com/contest/1153/problem/C)

## 【Description】

```cpp
Serval soon said goodbye to Japari kindergarten, and began his life in Japari Primary 
School.

In his favorite math class, the teacher taught him the following interesting definitions.

A parenthesis sequence is a string, containing only characters "(" and ")".

A correct parenthesis sequence is a parenthesis sequence that can be transformed into a 
correct arithmetic expression by inserting characters "1" and "+" between the original 
characters of the sequence. For example, parenthesis sequences "()()", "(())" are correct 
(the resulting expressions are: "(1+1)+(1+1)", "((1+1)+1)"), while ")(" and ")" are not. 
Note that the empty string is a correct parenthesis sequence by definition.

We define that |s| as the length of string s. A strict prefix s[1…l] (1≤l<|s|) of a 
string s=s1s2…s|s| is string s1s2…sl. Note that the empty string and the whole string are 
not strict prefixes of any string by the definition.

Having learned these definitions, he comes up with a new problem. He writes down a string 
s containing only characters "(", ")" and "?". And what he is going to do, is to replace 
each of the "?" in s independently by one of "(" and ")" to make all strict prefixes of 
the new sequence not a correct parenthesis sequence, while the new sequence should be a 
correct parenthesis sequence.

After all, he is just a primary school student so this problem is too hard for him to 
solve. As his best friend, can you help him to replace the question marks? If there are 
many solutions, any of them is acceptable.
```

## 【Input】

```cpp
The first line contains a single integer |s| (1≤|s|≤3⋅105), the length of the string.

The second line contains a string s, containing only "(", ")" and "?".
```

## 【Output】

```cpp
A single line contains a string representing the answer.

If there are many solutions, any of them is acceptable.

If there is no answer, print a single line containing ":(" (without the quotes).
```

------



## 【Examples】 

#### Sample Input

```cpp
6
(?????
```

#### Sample Output

```cpp
(()())	
```
#### Sample Input

```cpp
10
(???(???(?
```

#### Sample Output

```cpp
:(
```
------



## 【Problem Description】

```cpp
给你一个只含有"(" ")" "?" 的序列，问能否找到一个括号序列，使得此括号序列为一个可插入序列，并且此序列的任
意一个前缀不为可插入序列
例:()(())，不满足题意，它的前缀()为可插入序列
((())())满足题意
```

## 【Solution】

```cpp
首先，整个序列长度n一定是偶数，然后对整个序列统计左括号数论left，右括号数量right，若任意括号数量大于n/2
个，则false。
对所有的"?"若左括号数未达到n/2个，则"?"变为"(",否则"?"变为")"

最后再对整个序列check一遍，判断是否存在某个右括号未配对。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-04-14 12:35:01 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-04-14 12:41:15
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 100005
int a[maxn];
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;cin>>n;
    string s;cin>>s;
    if(s.size()%2){//n必须为偶数
        cout<<":("<<endl;
        cin.get(),cin.get();
        return 0;
    }
    int left=0,right=0; //左括号数量，右括号数量
    for(auto v:s){
        if(v=='(') left++;
        if(v==')') right++;
    }
    if(left>n/2||right>n/2){//判断，是否大于总序列长度的一半
        cout<<":("<<endl;
        cin.get(),cin.get();
        return 0;
    }
    left=n/2-left;
    for(auto &v:s){//优先将左括号数量增加到总序列长度的一半
        if(v=='?'){
            if(left) v='(',left--;
            else v=')';
        }
    }
    left=0;
    for(int i=0;i<s.size()-1;i++){//check
        if(s[i]=='(') left++;
        else left--;
        if(left<=0){
            cout<<":("<<endl;
            cin.get(),cin.get();
            return 0;
        }
    }
    cout<<s<<endl;
    cin.get(),cin.get();
    return 0;
}
```
