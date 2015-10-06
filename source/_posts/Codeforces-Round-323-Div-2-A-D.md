title: 'Codeforces Round #323 (Div. 2) A~D'
date: 2015-10-06 21:00:35
tags:
- Codeforces

categories:
- acm_比赛

---

自从CF改时间后,很久没有做CF了...

### 传送门: [Codeforces Round #323 (Div. 2)](http://codeforces.com/contest/583)

<!-- more -->

* ##### [A. Asphalting Roads]()

水题..

```java
/**
 * Created by ckboss on 15-10-5.
 */

import java.util.*;

public class CF583A {

    CF583A(){

        Scanner in = new Scanner(System.in);

        int  n = in.nextInt();
        boolean[] hang = new boolean[n+1];
        boolean[] lie = new boolean[n+1];
        Vector<Integer> vi = new Vector<Integer>();

        for(int i=1,x,y;i<=n*n;i++) {

            x=in.nextInt();
            y=in.nextInt();

            if(hang[x]==false && lie[y]==false) {
                hang[x]=true;
                lie[y]=true;

                vi.add(i);
            }
        }

        for(Integer x : vi){
            System.out.printf(x+" ");
        }
    }

    public static void main(String[] args) {
        new CF583A();
    }
}
```


* #### [B. Robot's Task](http://codeforces.com/contest/583/problem/B)

水 模拟

```java
/**
 * Created by ckboss on 15-10-5.
 */
import java.util.*;

public class CF583B {

    int n;
    int[] a = new int[1111];
    boolean[] vis = new boolean[1111];

    void gogogo(int[] args) {

        int pos=args[0];
        int dir=args[1];
         pos+=dir;
         if(pos==n) {
             pos=n-1;
             dir=-1;
         }
         else if(pos==-1) {
             pos=1;
             dir=1;
         }

        args[0]=pos;
        args[1]=dir;
    }

    CF583B(){

        Scanner in = new Scanner(System.in);

        n=in.nextInt();
        for(int i=0;i<n;i++) {
            a[i]=in.nextInt();
            vis[i]=false;
        }

        int eng=0;
        int pos=0;
        int dir=1;
        int change=0;

        while(true) {


            if(vis[pos]==false) {

                if (a[pos] <= eng) {
                    vis[pos] = true;
                    eng++;
                }
            }

            if(eng>=n) break;

            int[] args = {pos,dir};
            gogogo(args);
            pos=args[0];

            if(dir!=args[1]) change++;
            dir=args[1];
        }

        System.out.println(change);
    }

    public static void main(String[] args) {
        new CF583B();
    }
}
```

* #### [C. GCD Table](http://codeforces.com/contest/583/problem/C)

有点意思,从大到小的推,两个数的GCD肯定不会比这两个数大.

```java
/**
 * Created by ckboss on 15-10-5.
 */

import java.util.*;

public class CF583C {

    final int maxn=550;

    int n;
    int[] a = new int[maxn*maxn];
    Map<Integer,Integer> mp = new HashMap<Integer, Integer>();
    Vector<Integer> vi = new Vector<Integer>();

    int gcd(int a,int b) {
        if(b==0) return a;
        return gcd(b,a%b);
    }

    CF583C() {

        Scanner in = new Scanner(System.in);
        n=in.nextInt();

        for(int i=0;i<n*n;i++) {
            a[i]=in.nextInt();
            mp.put(a[i],mp.containsKey(a[i])?mp.get(a[i])+1:1);
        }

        Arrays.sort(a,0,n*n);

        for(int i=n*n-1;i>=0;i--) {
            if(mp.get(a[i])<=0)
                continue;
            mp.put(a[i],mp.get(a[i])-1);
            for(Integer x : vi) {
                int u = gcd(x,a[i]);
                mp.put(u,mp.get(u)-2);
            }
            vi.add(a[i]);
        }

        for(Integer x : vi) {
            System.out.printf("%d ",x);
        }
    }

    public static void main(String[] args) {
        new CF583C();
    }
}
```

* #### [D. Once Again...](http://codeforces.com/contest/583/problem/D)


* #### [题解: http://codeforces.com/blog/entry/20692](http://codeforces.com/blog/entry/20692)



will give the same matrix but for length p + q. As soon as such multiplication is associative, next we will use fast matrix exponentiation algorithm to calculate M[i][j] (the answer for a1, a2, ..., anT) — matrix mt[i][j] raised in power T. The answer is the maximum in matrix M. Such solution has complexity .

time limit per test1 second
memory limit per test256 megabytes
inputstandard input
outputstandard output
You are given an array of positive integers a1, a2, ..., an × T of length n × T. We know that for any i > n it is true that ai = ai - n. Find the length of the longest non-decreasing sequence of the given array.

Input
The first line contains two space-separated integers: n, T (1 ≤ n ≤ 100, 1 ≤ T ≤ 107). The second line contains n space-separated integers a1, a2, ..., an (1 ≤ ai ≤ 300).

Output
Print a single number — the length of a sought sequence.

Sample test(s)
input
4 3
3 1 4 2
output
5
Note
The array given in the sample looks like that: 3, 1, 4, 2, 3, 1, 4, 2, 3, 1, 4, 2. The elements in bold form the largest non-decreasing subsequence.


```java
import java.util.Scanner;

/**
 * Created by ckboss on 15-10-5.
 */
public class CF583D {

    final long INF=1<<62;

    class Matrix {
        Matrix(int _nx,int _ny) {
            nx=_nx; ny=_ny;
            m = new long[nx+1][ny+1];
        }
        public int nx,ny;
        public long[][] m;

        Matrix Add(Matrix mt) {

            Matrix ret = new Matrix(nx,ny);

            for(int i=1;i<=nx;i++) {
                for(int j=1;j<=ny;j++) {
                    ret.m[i][j]=-INF;
                    for(int k=1;k<=nx;k++) {
                        ret.m[i][j]=Math.max(ret.m[i][j],m[i][k]+mt.m[k][j]);
                    }
                }
            }

            return ret;
        }

        void Showit() {
            System.out.printf("nx: %d ny: %d\n",nx,ny);
            for(int i=1;i<=nx;i++) {
                for(int j=1;j<=ny;j++) {
                    System.out.printf("%d%c",m[i][j],(j==ny)?'\n':' ');
                }
            }
        }
    }

    Matrix QuickAdd(Matrix m ,int t) {

        Matrix ret = new Matrix(n,n);

        while(t!=0) {
            if(t%2==1) {
                ret=ret.Add(m);
            }
            m=m.Add(m);
            t/=2;
        }

        return ret;
    }

    int n,T;
    int[] a;

    CF583D() {

        Scanner in = new Scanner(System.in);

        n=in.nextInt();
        T=in.nextInt();
        a=new int[n+10];

        for(int i=1;i<=n;i++) {
            a[i]=in.nextInt();
        }

        /// get matrix
        Matrix M = new Matrix(n,n);

        for(int i=1;i<=n;i++) {
            for(int j=1;j<=n;j++) {
                if(a[j]<a[i])  {
                    M.m[i][j]=-INF;
                    continue;
                }
                M.m[i][j]=1;
                for(int k=1;k<j;k++) {
                   if(a[j]>=a[k]) {
                        M.m[i][j]=Math.max(M.m[i][j],M.m[i][k]+1);
                   }
                }
            }
        }

        Matrix mm = QuickAdd(M,T);

        long ans=0;
        for(int i=1;i<=n;i++) {
            for(int j=1;j<=n;j++) {
                ans=Math.max(ans,mm.m[i][j]);
            }
        }

        System.out.println(ans);
    }

    public static void main(String[] args) {
        new CF583D();
    }
}
```