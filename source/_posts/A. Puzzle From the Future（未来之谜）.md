---
title: A. Puzzle From the Future（未来之谜）
tags: 字符串 算法 数据结构
categories: []
date: 2021-01-24 13:34:07
---

<!--more-->

每次测试的时限
1秒
每次测试的内存限制
256兆字节
输入
标准输入
输出
标准输出

## 原题：[原题链接](https://codeforces.com/contest/1474/problem/A)

In the 2022 year, Mike found two binary integers a and b of length n (both of them are written only by digits 0 and 1) that can have leading zeroes. In order not to forget them, he wanted to construct integer d

in the following way:

    he creates an integer c

as a result of bitwise summing of a and b without transferring carry, so c may have one or more 2-s. For example, the result of bitwise summing of 0110 and 1101 is 1211 or the sum of 011000 and 011000 is 022000
;
after that Mike replaces equal consecutive digits in c
by one digit, thus getting d. In the cases above after this operation, 1211 becomes 121 and 022000 becomes 020 (so, d

    won't have equal consecutive digits). 

Unfortunately, Mike lost integer a
before he could calculate d himself. Now, to cheer him up, you want to find any binary integer a of length n such that d

will be maximum possible as integer.

Maximum possible as integer means that 102>21
, 012<101, 021=21

and so on.
Input

The first line contains a single integer t
(1≤t≤1000

) — the number of test cases.

The first line of each test case contains the integer n
(1≤n≤105) — the length of a and b

.

The second line of each test case contains binary integer b
of length n. The integer b consists only of digits 0 and 1

.

It is guaranteed that the total sum of n
over all t test cases doesn't exceed 105

.
Output

For each test case output one binary integer a
of length n. Note, that a or b may have leading zeroes but must have the same length n

.
Example
Input
Copy

5
1
0
3
011
3
110
6
111000
6
001011

Output
Copy

1
110
100
101101
101110

Note

In the first test case, b=0
and choosing a=1 gives d=1

as a result.

In the second test case, b=011

so:

    if you choose a=000

, c will be equal to 011, so d=01
;
if you choose a=111
, c will be equal to 122, so d=12
;
if you choose a=010
, you'll get d=021
.
If you select a=110
, you'll get d=121

    . 

We can show that answer a=110 is optimal and d=121 is maximum possible.

In the third test case, b=110
. If you choose a=100, you'll get d=210 and it's the maximum possible d

.

In the fourth test case, b=111000
. If you choose a=101101, you'll get d=212101 and it's maximum possible d

.

In the fifth test case, b=001011
. If you choose a=101110, you'll get d=102121 and it's maximum possible d.


## 翻译题目：

在里面 2022
年，麦克发现了两个二进制整数 一种 和 b 长度 ñ （它们都只用数字写 0 和 1）可以包含前导零。为了不忘记他们，他想构造整数d

通过以下方式：
 他创建一个整数 C

作为按位求和的结果 一种 和 b 不转移进位，所以C 可能有一个或多个 2-s。例如，按位求和的结果0110 和 1101 是 1211 或 011000 和 011000 是 022000
;
之后，麦克替换了相等的连续数字 C
一位数 d。在上述情况下，执行此操作后，1211 变成 121 和 022000 变成 020 （所以， d不会有相等的连续数字）。 

不幸的是，迈克失去了整数 一种
在他计算之前 d他自己。现在，要振作起来，您想找到任何二进制整数一种 长度 ñ 这样 d

将最大可能为integer。

最大值（可能为整数）表示 102 > 21
, 012 < 101, 021=21

等等。
输入值

第一行包含一个整数 Ť
(1≤t≤1000

）-测试用例的数量。

每个测试用例的第一行包含整数 ñ
(1≤n≤105）—的长度 一种 和 b

.

每个测试用例的第二行包含二进制整数 b
长度 ñ。整数b 仅由数字组成 0 和 1

.

保证总和为 ñ
总体 Ť 测试用例不超过 105

.
输出量

对于每个测试用例，输出一个二进制整数一种
长度 ñ。注意一种 要么 b 可能有前导零，但长度必须相同 ñ

.
例
输入值
复制

5
1
0
3
011
3
110
6
111000
6
001011

输出量
复制

1
110
100
101101
101110

注意

在第一个测试案例中， b = 0
然后选择 a = 1 给 d=1

结果是。

在第二个测试用例中， b = 011

所以：如果你选择 a = 000

, C 将等于 011，所以 d=01
;
如果你选择 a = 111
, C 将等于 122，所以 d=12
;
如果你选择 a = 010
， 你会得到 d=021
.
如果选择 a = 110
， 你会得到 d=121


我们可以显示答案 a = 110 是最优的 d=121 是最大可能的。

在第三个测试用例中， b = 110
。如果你选择a = 100， 你会得到 d=210 这是最大可能的 d

.

在第四个测试用例中， b = 111000
。如果你选择a = 101101， 你会得到 d=212101 这是最大可能的 d

.

在第五个测试案例中， b = 001011
。如果你选择a = 101110， 你会得到 d=102121 这是最大可能的 d.

## 本题思路

：根据b来确定最大值d，d-b就是a的结果
d最大值确定思路：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210124132216653.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzI3MjI1,size_16,color_FFFFFF,t_70#pic_center)

第一个a[0],肯定是1,即 d[0]=b[0]+a[0];
经过手动模拟发现，d[j]的取值跟d[j-1]和b[j]有关，如果b[j]>=d[j-1],根据最大原则且max(b[j])==1，则a[j]=1;d[j]=b[j]+a[j];
如果b[j]<d[j-1],~~(1<2  0<1  0<2)~~ ,则根据最大原则，如果(b[j]+1)<d[j-1]  即d[j-1]和b[j]的差值大于1 ,则d[j]+=1；a[j]=1;

本题坑点：输入b的时候，题目没有说明取值范围，如果定义为int乃至long long等都是会超的（9999999....?），所以要定义为字符串类型，再存到b[j]数组中（也可以直接操作字符串，不一点需要存）
## ac代码：

```cpp
#include<iostream>
#include<string>
using namespace std;
int n;
int d[100005];
int a[100005];
int b[100005];
int main(){
	int t,nt;
	cin>>t;
	string str;
	for(int i=0;i<t;++i){
		cin>>n;
		nt=n;
		cin>>str;
		for(int j=0;j<n;++j){
			b[j]=(str[j]-'0');
		}
		d[0]=b[0]+1;
		a[0]=1;
		for(int j=1;j<n;++j){
			if(b[j]>=d[j-1]){   //1>0  1==1 0==0
				d[j]=b[j]+1;
				a[j]=1;
			}
			else{//  1<2  0<1  0<2
				a[j]=0;
				d[j]=b[j];
				if(d[j-1]-d[j]>1){
					d[j]+=1;
					a[j]=1;
				}
			}
		}
		for(int j=0;j<n;++j){
			cout<<a[j]; 
		}
		cout<<endl;
	}
	return 0;
} 
```

