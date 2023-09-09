---
title: Yet Another Array Restoration(又一个阵列恢复)
tags: 数据结构 算法
categories: []
date: 2020-12-12 17:36:35
---

<!--more-->

We have a secret array. You don't know this array and you have to restore it. However, you know some facts about this array:

The array consists of n distinct positive (greater than 0) integers.
The array contains two elements x and y (these elements are known for you) such that x<y.
If you sort the array in increasing order (such that a1<a2<…<an), differences between all adjacent (consecutive) elements are equal (i.e. a2−a1=a3−a2=…=an−an−1).
It can be proven that such an array always exists under the constraints given below.

Among all possible arrays that satisfy the given conditions, we ask you to restore one which has the minimum possible maximum element. In other words, you have to minimize max(a1,a2,…,an).

You have to answer t independent test cases.

Input
The first line of the input contains one integer t (1≤t≤100) — the number of test cases. Then t test cases follow.

The only line of the test case contains three integers n, x and y (2≤n≤50; 1≤x<y≤50) — the length of the array and two elements that are present in the array, respectively.

Output
For each test case, print the answer: n integers a1,a2,…,an (1≤ai≤109), where ai is the i-th element of the required array. If there are several answers, you can print any (it also means that the order of elements doesn't matter).

It can be proven that such an array always exists under the given constraints.

Example
Input
5
2 1 49
5 20 50
6 20 50
5 3 8
9 13 22
Output
1 49 
20 40 30 50 10
26 32 20 38 44 50 
8 23 18 13 3 
1 10 13 4 19 22 25 16 7 

翻译：

```cpp
我们有一个秘密的阵法。你不知道这个数组，你必须恢复它。但是，您知道有关此阵列的一些事实：
数组由n个不同的正整数（大于0）组成。
数组包含两个元素x和y（这些元素对您来说是已知的），因此x<y。
如果按递增顺序对数组排序（例如a1<a2<…<an），则所有相邻（连续）元素之间的差异相等（即a2−a1=a3−a2=…=an−1）。
可以证明，这样的数组总是存在于下面给出的约束条件下。
在满足给定条件的所有可能的数组中，我们要求您恢复一个具有最小可能最大元素的数组。换句话说，你必须最小化max（a1，a2，…，an）。
你必须回答t个独立的测试用例。
输入
输入的第一行包含一个整数t（1≤t≤100）——测试用例的数量。接下来是t测试用例。
测试用例的唯一一行包含三个整数n，x和y（2≤n≤50；1≤x<y≤50）-数组的长度和数组中存在的两个元素。
输出
对于每个测试用例，打印答案：n个整数a1，a2，…，an（1≤ai≤109），其中ai是所需数组的第i个元素。如果有几个答案，您可以打印任何答案（这也意味着元素的顺序无关紧要）。
可以证明，在给定的约束条件下，这样的数组总是存在的。

```


第一次：按照问题顺序模拟：

```cpp
#include<iostream>
#include<vector>
#include<math.h>
#include<algorithm>
using namespace std;

int main(){
	long long n,a,b,len;
	scanf("%lld",&n);
	vector<long long> v;
	long long shu,tlen;
	while(n--){
		v.clear();
		shu=-1;
		scanf("%lld %lld %lld",&len,&a,&b);
		len-=2;
		tlen=len;
		v.push_back(a);
		v.push_back(b);
	//	if((b-a)%2==0){//奇数，中间放不下 
			while((b-a-1-len)%(len+1)!=0){// b-a-(1+len)
				--len;
			}
			//中间用了len 个
			shu=(b-a)/(len+1);
			for(long long i=1;i<=len;++i){
				v.push_back(a+shu*i);
			}
			
	//	}
		if(shu==-1){
			len=0;
			shu=b-a;
		}
		// ,开始左边
		
		len=tlen-len;
		tlen=len;
	//	cout<<"len zuo="<<len<<endl;
		for(long long i=1;i<=tlen;++i){
			if((a-shu*i)<0){
			//	len-=i;//又用掉了 i个，剩下 len-i个 
			//	cout<<"len zuo 完了"<<i<<endl;
				break;
			}
			len--;
			v.push_back(a-shu*i);
		} 
	//	cout<<"len you="<<len<<endl;
		//右边
		for(long long i=1;i<=len;++i){
			v.push_back(b+shu*i);
		} 
		long long size=v.size();
//		sort(v,v+size);
		printf("%lld",v[0]);
		for(long long i=1;i<size;++i){
			printf(" %lld",v[i]);
		}
		printf("\n");
	}


	return 0;
}
```

结果：案例手动都过，测试案例1超时，案例2错误

修改思路：因为每两个之间是等差，所以不需要存起来，只需要直到公差即可，因为题目不要求答案呢顺序，所有每次输出当前值+公差。那么问题就只有哪个是第一个了。因为最小值必须大于0，而且要包含给出的两个值(a,b,a<b)，所以，最小值要么是给出值的第一个a（a<b）,要么小于a,所以根据公差一将a一直减去公差（直到满足个数，或者<0）

所以改良ac代码：
```cpp
#include<iostream>
#include<vector>
#include<math.h>
#include<algorithm>
using namespace std;

int main(){
	int t;
	scanf("%d",&t);
	int n,x,y;
	int d,k;//d为公差 
	int s;//初始值 
	while(t--){
		scanf("%d%d%d",&n,&x,&y);
		d=y-x;//初始公差为两个数的差值,这个公差是目前最小，也是最合理的 
		
		k=n-1;//假设最大为y，需要计算前 n-1个    
		// 5 3 8  d=5 
		//如果两个里面可以继续放
		while(d%k!=0){//如果放不下，则k为1 
			k--;
		}
		//修改公差 此时 k==1|k>1
		d/=k;
		
		//前面真的能放下n-1个？  // y - (n-1)*d>0?
		if(d*(n-1)<y){ 
			s=y-(n-1)*d;
		}else{
			//an=a1+d*(n-1)  -> a1为 an/d的余数 
			s=y%d;
			if(s==0)s+=d;
		}
		//a1找到，公差找到，，输出 a1后面 n-1个
		
		for(int i=0;i<n-1;++i){
			printf("%d ",s+i*d);
		} 
		printf("%d\n",s+n*d-d);
	}
	
	
	return 0;
} 
```

