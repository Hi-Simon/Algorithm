#  [2018-ACM-ICPC-焦作赛区网络赛-J. Participate in E-sports-java大数二分判完全平方数](https://www.jisuanke.com/contest/1558?view=challenges)



## 【Description】

> ```
> Jessie and Justin want to participate in e-sports. E-sports contain many games, but
> they don't know which one to choose, so they use a way to make decisions.
> 
> They have several boxes of candies, and there are iii candies in the ithi^{th}ith 
> box, each candy is wrapped in a piece of candy paper. Jessie opens the candy boxes
> in turn from the first box. Every time a box is opened, Jessie will take out all 
> the candies inside, finish it, and hand all the candy papers to Justin.
> 
> When Jessie takes out the candies in the NthN^{th}Nth box and hasn't eaten yet, if 
> the amount of candies in Jessie's hand and the amount of candy papers in Justin's 
> hand are both perfect square numbers, they will choose Arena of Valor. If only the 
> amount of candies in Jessie's hand is a perfect square number, they will choose 
> Hearth Stone. If only the amount of candy papers in Justin's hand is a perfect 
> square number, they will choose Clash Royale. Otherwise they will choose League of
> Legends.
> 
> Now tell you the value of NNN, please judge which game they will choose.
> ```

## 【Input】

> ```
> The first line contains an integer T(1≤T≤800)T(1 \le T \le 800)T(1≤T≤800) , which 
> is the number of test cases.
> 
> Each test case contains one line with a single integer: N(1≤N≤10^200)N(1 \le N \le 
> 10^{200})N(1≤N≤10200) .
> ```

## 【Output】

> ```
>  For each test case, output one line containing the answer.
> ```

------



## 【Examples】 

> ### Sample Input

> ```
> 4
> 1
> 2
> 3
> 4
> ```

> ###Sample Output

> ```
> Arena of Valor
> Clash Royale
> League of Legends
> Hearth Stone
> ```

------



## 【Problem Description】

> ```
> 就判断n和sum(n-1)是否是完全平方数. sum(n-1)=1+2+3+..+n-1
> ```

## 【Solution】

> ```
> 用java大数二分判断两次即可。
> ```

------



## 【Code】

```c++
import java.util.*;

import java.math.BigInteger;

public class Main {
    static Scanner s=new Scanner(System.in);
    public static  BigInteger binary_find(BigInteger n){
        BigInteger l = new BigInteger("1");
        BigInteger two = new BigInteger("2");
        BigInteger r = new BigInteger("100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000");
        BigInteger mid;
        while (l.compareTo(r) != 1) {
            mid = l.add(r);
            mid = mid.shiftRight(1);
//                System.out.println(mid);
            BigInteger ss = mid.multiply(mid);
            if (ss.compareTo(n) == -1) {
                l = mid.add(BigInteger.ONE);
            } else {
                r = mid.subtract(BigInteger.ONE);
            }
        }
        return l;
    }
    public static void main(String[] args){
        int T=s.nextInt();
        while (T>0) {
            T--;
            BigInteger n = s.nextBigInteger();
            if(n.equals(BigInteger.ONE)){
                System.out.println("Arena of Valor");
                continue;
            }
//            System.out.println(l);
            boolean e=false,f=false;
            BigInteger i=binary_find(n);
            if(i.multiply(i).equals(n)) e=true;
            i=binary_find(n.multiply(n.subtract(BigInteger.ONE)).divide(BigInteger.valueOf(2)));
            if(i.multiply(i).equals(n.multiply(n.subtract(BigInteger.ONE)).divide(BigInteger.valueOf(2)))) f=true;
            if(e && f) {
                System.out.println("Arena of Valor");
            } else if (e) {
                System.out.println("Hearth Stone");
            } else if (f) {
                System.out.println("Clash Royale");
            } else {
                System.out.println("League of Legends");
            }
        }
    }
}
```
