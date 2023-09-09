---
title: Decrease the Sum of Digits
tags: 字符串 算法
categories: []
date: 2020-12-12 17:50:59
---

<!--more-->

You are given a positive integer n. In one move, you can increase n by one (i.e. make n:=n+1). Your task is to find the minimum number of moves you need to perform in order to make the sum of digits of n be less than or equal to s.

You have to answer t independent test cases.

Input
The first line of the input contains one integer t (1≤t≤2⋅104) — the number of test cases. Then t test cases follow.

The only line of the test case contains two integers n and s (1≤n≤1018; 1≤s≤162).

Output
For each test case, print the answer: the minimum number of moves you need to perform in order to make the sum of digits of n be less than or equal to s.

Example
Input
5
2 1
1 1
500 4
217871987498122 10
100000000000000001 1
Output
8
0
500
2128012501878
899999999999999999

翻译：

```cpp
给你一个正整数n。一步，你可以把n增加1（即使n：=n+1）。您的任务是找出要使n的位数之和小于或等于s所需执行的最小移动次数。
你必须回答t个独立的测试用例。
输入
输入的第一行包含一个整数t（1≤t≤2⋅104）-测试用例的数量。接下来是t测试用例。
测试用例的唯一一行包含两个整数n和s（1≤n≤1018；1≤s≤162）。
输出
对于每个测试用例，打印答案：为了使n的位数之和小于或等于s，您需要执行的最小移动次数。
例子
输入
5
2 1
11
500 4
217871987498122 10
1000000000000001 1
输出
8
0
500
2128012501878
899999999999999999
```
第一次代码：ac但是改成cin,cout时间容易超时（1500ms）
```cpp
#include<iostream>
#include<string.h>
using namespace std;
typedef long long ll;
ll str=0.;
ll len;
char ch[22];
int s;
ll pp;
ll getsum(int ln){
	int res=0;
		for(int i=0;i<ln;++i){
			res+=(ch[i]-'0');
		}
		cout<<endl;
	if(res<0)res=0;
	return res;
}
void solv(){
	str=0;
	memset(ch,0,sizeof(ch));
	
	scanf("%s %d",&ch,&s);
	len=strlen(ch);
	pp=1;
	int t=getsum(len);
	
	
	if(t<=s){
		printf("0\n");
		return;
	}
	
	//字符串反转
	char cht;
        int i=0;
	for(i=0;i<len/2;++i){//12345  54321   123456  654321
		cht=ch[i];
		ch[i]=ch[len-i-1];
		ch[len-i-1]=cht;
	} 
	
	for(i=0;i<len;++i){
		str+=((10-(ch[i]-'0'))*pp);
		ch[i]='0';
		ch[i+1]+=1;
		pp*=10;
		if(i+1>=len){
			len+=1;
			ch[i+1]='1';
		}
		t=getsum(len);
		if(t<=s){
			break;
		}
	}
	
	printf("%lld\n",str);
	
}

int main(){
	int n;
	scanf("%ld",&n);
	while(n--){
		solv();
	}
	
	return 0;
} 
```

优化思路：每次判断
`t=getsum(len);
		if(t<=s){
			break;
		}`
		可以在第一次的基础上进行
不需要每次调用函数

第二次：ac,93ms
```cpp
#include<iostream>
#include<string.h>
using namespace std;
typedef long long ll;
ll str=0.;
ll len;
char ch[22];
int s;
ll pp;
ll getsum(int ln){
//	cout<<"ln="<<ln<<endl;
	int res=0;
		for(int i=0;i<ln;++i){
	//		cout<<"ch= "<<ch[i]<<" ";
			res+=(ch[i]-'0');
		}
	if(res<0)res=0;
	return res;
}
void solve(){
	str=0;
	memset(ch,0,sizeof(ch));
	
	scanf("%s %d",&ch,&s);
	len=strlen(ch);
	pp=1;
	int t=getsum(len);
	
	//cout<<"t="<<t<<endl;
	if(t<=s){
		printf("0\n");
		return;
	}
	
	//字符串反转  原因：因为要进为  如： 500 -> 1000   反转后 005 -> 0001 
	char cht;
	for(int i=0;i<len/2;++i){//123465
		cht=ch[i];
		ch[i]=ch[len-i-1];
		ch[len-i-1]=cht;
	} 
	
	for(int i=0;i<len;++i){
		str+=((10-(ch[i]-'0'))*pp);
		t=t-(ch[i]-'0')+1; 
		ch[i]='0';
		ch[i+1]+=1;
		pp*=10;//根据*10得到该位的十进制上的值 
		if(i+1>=len){//进位  
			len+=1;
			ch[i+1]='1';
			
		}
		//t=getsum(len);
	//	cout<<"t="<<t<<endl;
		//t=t-(ch[i]-'0')+1; 
		if(t<=s){
			break;
		}
	}
	printf("%lld\n",str);
}

int main(){
	int n;
	cin>>n;
	while(n--){
		solve();//solve解决方案 
	}
	
	return 0;
} 
```

