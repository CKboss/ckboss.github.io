title: 'Codeforces Round #324 (Div. 2) A~E'
date: 2015-10-08 14:14:55
tags:
- Codeforces
- 思维

categories:
- acm_比赛

---

**[Codeforces Round #324 (Div. 2)](http://codeforces.com/contest/584)**

**整体难度不大=两个水题+一个模拟+数论贪心+一个有点难度的贪心.**

[题目传送门](http://codeforces.com/contest/584/problems)


<!-- more -->

### A. Olesya and Rodion

水:
```python
n,t=map(int,input().split())

if t==10:
    if n==1:
        print(-1)
    else:
        print('1'+'0'*(n-1))
else:
    print(str(t)+'0'*(n-1))
```


### B. Kolya and Tanya

表意不清的英文+乱七八糟的配图:

python的代码就是短...

```python
n=int(input())
print((27**n-7**n)%1000000007)
```


### C. Marina and Vasya

很多坑的模拟.

```java
/**
 * Created by ckboss on 15-10-7.
 */
import java.util.*;

public class CF584C {

    int n,t;
    int t1,t2;
    String s1,s2;
    StringBuffer s3;

    char getChar(char c1,char c2) {
        char c;
        for(c='a';c<='z';c++) {
            if(c!=c1&&c!=c2) {
                break;
            }
        }
        return c;
    }

    CF584C() {

        Scanner in = new Scanner(System.in);

        n=in.nextInt(); t=in.nextInt();
        s1=in.next(); s2=in.next();
        s3=new StringBuffer();
        t1=0; t2=0;

        for(int i=0;i<n;i++) {
            char c1=s1.charAt(i),c2=s2.charAt(i);
            if(c1!=c2) {
                if (t1 <= t2) {
                    s3.append(c2);
                    t1++;
                } else {
                     s3.append(c1);
                    t2++;
                }
            }
            else s3.append(c1);
        }

        //System.out.println("---> "+s3+" t1: "+t1+" t2: "+t2);

        if(t1>t||t2>t) {
            System.out.println("-1");
            return ;
        }

        boolean flag=false;
        for(int i=0;i<n;i++) {

            char c1=s1.charAt(i),c2=s2.charAt(i);

            if(c1!=c2) {
                char oc = s3.charAt(i);
                if(oc==c1&&t1<t) {
                    s3.setCharAt(i, getChar(c1, c2));
                    t1++;
                }
                if(oc==c2&&t2<t) {
                    s3.setCharAt(i, getChar(c1, c2));
                    t2++;
                }
            }

            if (t1 == t && t2 == t) {
                flag = true;
                break;
            }
        }

        for(int i=0;i<n&&flag==false;i++) {

            char c1=s1.charAt(i),c2=s2.charAt(i);

            if(c1==c2) {

                s3.setCharAt(i, getChar(c1, c2));
                t1++; t2++;
            }

            if (t1 == t && t2 == t) {
                flag = true;
                break;
            }
        }

        if(flag==false) {
           System.out.println("-1");
        }
        else {
            System.out.println(s3);
        }
    }

    public static void main(String[] args) {
        new CF584C();
    }
}
```


### D. Dima and Lisa

每个数都可以拆分成不大于三个的质数的和(偶数除2外只要两个,奇数最多3个).

如何拆分呢?直接贪心取最大的就可以了.
因为质数还不算太稀少,暴力的一个一个往下判断就好了

```java
import java.util.Scanner;
import java.util.Vector;

/**
 * Created by ckboss on 15-10-7.
 */
public class CF584D {

    int n;
    Vector<Integer> vi;

    boolean isPrime(int x) {
        if(x<2) return false;
        if(x==2) return true;
        if(x%2==0) return false;
        for(int i=3;i*i<=x;i+=2) {
            if(x%i==0) return false;
        }
        return true;
    }

    CF584D() {

        Scanner in = new Scanner(System.in);
        n=in.nextInt();
        vi = new Vector<Integer>();
        //System.out.println("---> "+isPrime(n));

        int x=n;
        boolean first=false;
        while(n!=0) {
            if(isPrime(x)) {
                if(first==false) {
                    vi.add(x);
                    n -= x;
                    x = n;
                    first=true;
                }
                else if(first==true) {

                    if(isPrime(n-x)==true||n-x==0) {
                        vi.add(x);
                        if(n-x!=0) vi.add(n-x);
                        break;
                    }
                    else {
                        x--;
                    }
                }
            }
            else x--;
        }

        int sz=vi.size();
        System.out.println(sz);
        for(int i=0;i<sz;i++) {
            System.out.printf("%d ",vi.get(i));
        }
    }

    public static void main(String[] args) {
        new CF584D();
    }
}
```

### E. Anton and Ira


Anton loves transforming one permutation into another one by swapping elements for money, and Ira doesn't like paying for stupid games. Help them obtain the required permutation by paying as little money as possible.

More formally, we have two permutations, p and s of numbers from 1 to n. We can swap pi and pj, by paying |i - j| coins for it. Find and print the smallest number of coins required to obtain permutation s from permutation p. Also print the sequence of swap operations at which we obtain a solution.

##### Input
The first line contains a single number n (1 ≤ n ≤ 2000) — the length of the permutations.

The second line contains a sequence of n numbers from 1 to n — permutation p. Each number from 1 to n occurs exactly once in this line.

The third line contains a sequence of n numbers from 1 to n — permutation s. Each number from 1 to n occurs once in this line.

##### Output
In the first line print the minimum number of coins that you need to spend to transform permutation p into permutation s.

In the second line print number k (0 ≤ k ≤ 2·106) — the number of operations needed to get the solution.

In the next k lines print the operations. Each line must contain two numbers i and j (1 ≤ i, j ≤ n, i ≠ j), which means that you need to swap pi and pj.

It is guaranteed that the solution exists.

```cpp
Sample test(s)
input
4
4 2 1 3
3 2 4 1
output
3
2
4 3
3 1
```

##### Note
In the first sample test we swap numbers on positions 3 and 4 and permutation p becomes 4 2 3 1. We pay |3 - 4| = 1 coins for that. On second turn we swap numbers on positions 1 and 3 and get permutation 3241 equal to s. We pay |3 - 1| = 2 coins for that. In total we pay three coins.


**比较难想的贪心**

假如有这么两个数列:

$p: 7,4,6,2,5,1,3$ 和 $s: 7,5,6,1,3,2,4$

首先预处理出每个数字在每个数列中的位置,然后从s中的最后一位开始考虑.

s中的最后一位4出现在p中的第2个位置上,我们要通过交换把4换到n这个位置上来.为了不让交换的距离变的大,我们选择的与4交换的数在s中的位置应该<=4在p中的位置.

比如: 如果选择4和6交换位置,4的位置会往前挪一位,但是6的位置会远离6应该在的位置.
但是如果选择4和5交换位置,5在s中应该在的位置是2,4在p中暂时的位置也是2,选择5和4交换就可以同时让两个数字都更靠近应该在的位置.

如此贪心就可以了.


ps: 大量输出,java很容易超时...

```cpp
/* ***********************************************
Author        :CKboss
Created Time  :2015年10月08日 星期四 13时35分35秒
File Name     :CF584E.cpp
************************************************ */

#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <string>
#include <cmath>
#include <cstdlib>
#include <vector>
#include <queue>
#include <set>
#include <map>

using namespace std;

const int maxn=2222;

typedef pair<int,int> pII;

int n;
int p[maxn],s[maxn];
int pos_p[maxn],pos_s[maxn];

vector<pII> vp;

int main()
{
    //freopen("in.txt","r",stdin);
    //freopen("out.txt","w",stdout);

	scanf("%d",&n);
	for(int i=0;i<n;i++)
	{
		scanf("%d",p+i); pos_p[p[i]]=i;
	}
	for(int i=0;i<n;i++)
	{
		scanf("%d",s+i); pos_s[s[i]]=i;
	}

	int ans=0; vp.clear();

	for(int i=n-1;i>=0;i--)
	{
		if(s[i]==p[i]) continue;

		int idx=pos_p[s[i]];

		for(int j=idx+1;j<=i;j++)
		{
			int tp=pos_s[p[j]];

			if(tp<=idx)
			{
				ans+=j-idx;
				swap(p[j],p[idx]);
				swap(pos_p[p[j]],pos_p[p[idx]]);
				vp.push_back(make_pair(j+1,idx+1));
				idx=j;
			}
		}
	}

	int sz=vp.size();

	printf("%d\n%d\n",ans,sz);
	for(int i=0;i<sz;i++)
	{
		printf("%d %d\n",vp[i].first,vp[i].second);
	}

    return 0;
}
```


