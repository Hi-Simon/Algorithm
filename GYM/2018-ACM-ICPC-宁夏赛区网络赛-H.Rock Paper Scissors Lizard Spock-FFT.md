# [2018-ACM-ICPC-宁夏赛区网络赛-H.Rock Paper Scissors Lizard Spock-FFT](https://nanti.jisuanke.com/t/A1676)

## 【Description】

```cpp
Didi is a curious baby. One day, she finds a curious game, which named Rock Paper 
Scissors Lizard Spock.

The game is an upgraded version of the game named Rock, Paper, Scissors. Each player 
chooses an option . And then those players show their choices that was previously hidden 
at the same time. If the winner defeats the others, she gets a point.

The rules are as follows. 

Scissors cuts Paper

Paper covers Rock

Rock crushes Lizard

Lizard poisons Spock

Spock smashes Scissors

Scissors decapitates Lizard

Lizard eats Paper

Paper disproves Spock

Spock vaporizes Rock

(and as it always has) Rock crushes Scissors.
```

![242dd42a2834349b5866a238c9ea15ce36d3be2f.jpg](https://res.jisuanke.com/img/upload/4fc53620badcae79745eff6fa4c5ee26a24058ce.jpg)

```cpp
But Didi is a little silly, she always loses the game. In order to keep her calm, her 
friend Tangtang writes down the order on a list and show it to her. Didi also writes down
her order on another list, like 
```

![p1.png](https://res.jisuanke.com/img/upload/d6dfc11b870079532b4566d4e45a8521f3bd1fd3.png)

```cpp
(Rock-R Paper-P Scissors-S Lizard-L Spock-K)

However, Didi may skip some her friends' choices to find the position to get the most 
winning points of the game, like 
```

![p2.png](https://res.jisuanke.com/img/upload/7c3143b259b476f261bc21301dfc55fa7b621e24.png)

```cpp
Can you help Didi find the max points she can get?
```



## 【Input】

```cpp
The first line contains the list of choices of Didi's friend, the second line contains 
the list of choices of Didi.

(1<=len(s2)<=len(s1)<=1e6)
```

## 【Output】

```cpp
One line contains an integer indicating the maximum number of wining point.

输出时每行末尾的多余空格，不影响答案正确性
```

------



## 【Examples】 

#### Sample Input

```cpp
RRRRRRRRRLLL
RRRS
```

#### Sample Output

```cpp
3
```
#### Sample Input

```cpp
RSSPKKLLRKPS
RSRS
```

#### Sample Output

```cpp
2
```

------



## 【Problem Description】

```cpp
给定字符间的强弱关系：S-{P,L},P-{R,K},R-{L,S},L-{K,P},K-{S,R}
再给一个模式串，和匹配串，问匹配串在模式串中，所能获得的最大点数，字符强的匹配到相对字符弱的则获得一分。
```

## 【Solution】

```cpp
类似字符串匹配问题，容易想到要用FFT来做，但是，此题要求判断强弱关系。
可以考虑分类讨论，最后累加求和。即分别对5类来跑FFT，例如对第一类，将匹配串中S出现的位置置1，模式串中P，L出
现的位置置1，然后两串作为系数做多项式乘法即可。
注意要先倒置匹配串。
```

------



## 【Code】

```cpp
/*
 * @Author: Simon 
 * @Date: 2019-07-12 15:11:21 
 * @Last Modified by: Simon
 * @Last Modified time: 2019-07-12 18:10:01
 */
#include<bits/stdc++.h>
using namespace std;
typedef int Int;
#define int long long
#define INF 0x3f3f3f3f
#define maxn 4000005
const double pi=acos(-1.0);
char t[maxn],s[maxn];
struct Complex{
    double x,y;
    Complex(double x=0,double y=0):x(x),y(y){}
    Complex operator +(const Complex &a){return Complex(x+a.x,y+a.y);}
    Complex operator -(const Complex &a){return Complex(x-a.x,y-a.y);}
    Complex operator *(const Complex &a){return Complex(x*a.x-y*a.y,x*a.y+y*a.x);}
}f[maxn],g[maxn];
void change(Complex y[],int len){
    for(int i=1,j=len>>1;i<len-1;i++){
        if(i<j) swap(y[i],y[j]);
        int k=len>>1;
        while(j>=k) j-=k,k>>=1;
        if(j<k) j+=k;
    }
}
void fft(Complex y[],int len,int on){
    change(y,len);
    for(int mid=1;mid<len;mid<<=1){
        Complex wn(cos(pi/mid),on*sin(pi/mid));
        for(int R=mid<<1,j=0;j<len;j+=R){
            Complex w(1,0);
            for(int k=0;k<mid;k++,w=w*wn){
                Complex u=y[j+k],v=w*y[j+mid+k];
                y[j+k]=u+v;
                y[j+mid+k]=u-v;
            }
        }
    }
    if(on==-1) for(int i=0;i<len;i++) y[i].x/=len;
}
int F[maxn],len=1;
void solve(char c1,char c2,char c3,int len1,int len2){
    for(int i=0;i<len;i++) f[i]=g[i]=Complex(0,0);
    for(int i=0;i<len2;i++) if(t[i]==c1) g[i].x=1;//预处理
    for(int i=0;i<len1;i++) if(s[i]==c2||s[i]==c3) f[i].x=1;
    fft(f,len,1);fft(g,len,1);
    for(int i=0;i<len;i++) f[i]=f[i]*g[i];
    fft(f,len,-1);
    for(int i=len2-1;i<len1;i++) F[i]+=f[i].x+0.5; //对全匹配的地方累加求和
}
Int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin>>s>>t;
    int len1=strlen(s),len2=strlen(t);
    reverse(t,t+len2); //倒置匹配串
    while(len<=len1+len2) len<<=1;
    solve('S','P','L',len1,len2); solve('P','R','K',len1,len2); //分类讨论
    solve('R','L','S',len1,len2); solve('L','K','P',len1,len2); 
    solve('K','S','R',len1,len2); int ans=0;
    for(int i=len2-1;i<len1;i++) ans=max(ans,F[i]);//
    cout<<ans<<endl;
    cin.get(),cin.get();
    return 0;
}
```
