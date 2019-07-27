# [2014-ACM-ICPC-西安赛区区域赛-H. The Problem to Make You Happy-图上博弈论](https://codeforces.com/gym/100548/attachments)

## 【Description】

```cpp
Alice and Bob are good friends, as in every other storyline. One day Alice and Bob are
playing an interesting game. The game is played on a directed graph with n vertices and m
edges, Alice and Bob have exactly one chess piece each. Initially, Bob’s chess piece is 
placed on vertex x, while Alice’s chess piece is placed at vertex y. Bob plays first, 
then Alice, then Bob, then Alice and so on.
During each move, the player must move his/her chess piece from the vertex his/her chess
piece currently at to an adjacent vertex, by traveling through exactly one directed edge.
(Remember that the game is played on a directed graph.) If someone can’t make such a
move, he/she will be considered to lose the game.
There’s one additional rule: at any time, if Bob and Alice’s chess pieces are at the same
vertex, then Alice is consider to be the winner and the game ends up immediately.
Now you are given the initial situation, could you determine who is the winner? Please
note that neither Bob nor Alice will make any mistakes during the game, i.e. they both 
play optimally. In case that the game never ends up, Bob is considered to be the winner.
```

## 【Input】

```cpp
The first line of the input gives the number of test cases, T. T test cases follow.
For each test case, the first line contains two integers n and m (2  n  100, 1  m  n 
× (n − 1)). Next m lines, each line contains two integers b and e, indicating there
is one directed edge from vertex b to vertex e. Last line contains two integers x and y
(1  x, y  n, x ̸= y), which are Bob and Alice’s initial position. The graph contains no
self-loops or duplicate edges.
```

## 【Output】

```cpp
For each test case output one line “Case #x: y”, where x is the case number (starting 
from 1) and y is “Yes” (without quotes) if Bob can win or the game never ends up, 
otherwise “No”(without quotes).
```

------



## 【Examples】 

#### Sample Input

```cpp
35
3
1 2
3 4
4 5
3 1
4 3
1 2
2 3
3 4
1 2
3 3
1 2
2 3
3 1
2 1
```

#### Sample Output

```cpp
Case #1: Yes
Case #2: No
Case #3: Yes
```

------



## 【Problem Description】

```cpp
给你一个有向图，Bob，Alice的初始位置，两人轮流移动，Bob先手，每次移动一个单位。若某一方无法移动则输，或者
当Bob与Alice在同一个点上时，游戏结束，Bob输。问Bob是否能赢。
```

## 【Solution】

```cpp
动态规划
定义dp数组，dp[i][j][0]=0表示Bob在i位置，Alice在j位置，且该Bob走时Bob处于必输态。

首先预处理出所有明显的Bob必输的位置，然后根据博弈论基本原理转移求出所有Bob必输的状态即可。

即
1、假设Bob在位置i,若此位置为必输位置，则需满足i无论走到任意位置k，k是必输位置。
2、假设Alice在位置j，若此位置为必输位置，则需满足存在一个位置k，k是必输位置。

注意:非常重要的一点，上述所说的必输位置都是对Bob来说的状态。

```

------



## 【Code】

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <cstring>
#include <algorithm>
using namespace std;
#define maxn 105
struct node {
    int x, y, turn;
    node() {}
    node(int x, int y, int turn) : x(x), y(y), turn(turn) {}
};
int mp[maxn][maxn];//有向图存储
int dp[maxn][maxn][2]; //dp[i][j][0]表示Bob在i位置，Alice在j位置，该Bob走时先手 是否是必胜态
int du[maxn],/*各个点的出度*/ num[maxn][maxn];//num[i][now.y]表示Bob在i位置，Alice在now.y位置时的now.x个数
void bfs(int x, int y, int n) {
    memset(dp, -1, sizeof(dp));
    memset(num, 0, sizeof(num));
    queue<node> q;
    for (int i = 1; i <= n; i++) { //预处理所有先手必输态
        dp[i][i][0] = 0; //根据题目两个人走到同一位置，Bob必输
        dp[i][i][1] = 0;
        q.push({i, i, 0});
        q.push({i, i, 1});
    }
    for (int i = 1; i <= n; i++) { //预处理所有先手必输态
        if (du[i] == 0) { //根据题目若一方无路可走，则输
            for (int j = 1; j <= n; j++) {
                if (i == j) continue;
                dp[i][j][0] = 0;
                q.push({i, j, 0});
            }
        }
    }
    while (!q.empty()) {
        node now = q.front(); q.pop();
        if (now.turn == 0) {//若Alice走完之后，当前状态对Bob来说是必输态，则Alice未走的时的状态也是必输态
            for (int i = 1; i <= n; i++) {
                if (!mp[i][now.y]) continue;
                if (!dp[now.x][i][1]) continue;
                dp[now.x][i][1] = 0;
                q.push({now.x, i, 1});
            }
        }
        else {
            for (int i = 1; i <= n; i++) { //若Bob无论怎么走，对Alice来说都是必赢态，则Bob未走时的状态是必输态
                if (!mp[i][now.x]) continue;
                num[i][now.y]++;
                if (num[i][now.y] == du[i]) {
                    if (!dp[i][now.y][0]) continue;
                    dp[i][now.y][0] = 0;
                    q.push({i, now.y, 0});
                }
            }
        }
    }
    if (dp[x][y][0] == 0) cout << "No" << endl;
    else cout << "Yes" << endl;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t, ca = 0;
    cin >> t;
    while (t--) {
        memset(du, 0, sizeof(du));
        memset(mp, 0, sizeof(mp));
        int n, m; cin >> n >> m;
        cout << "Case #" << ++ca << ": ";
        for (int i = 1; i <= m; i++) {
            int u, v; cin >> u >> v;
            mp[u][v] = 1; du[u]++;
        }
        int x, y; cin >> x >> y;
        bfs(x, y, n);
    }
    return 0;
}

```
