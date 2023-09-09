---
title: G - 编辑距离
tags: 字符串 算法
categories: []
date: 2020-11-09 13:02:49
---

<!--more-->

@author 夜枫


编辑距离，又称Levenshtein距离（也叫做Edit Distance），是指两个字串之间，由一个转成另一个所需的最少编辑操作次数。许可的编辑操作包括将一个字符替换成另一个字符，插入一个字符，删除一个字符。
例如将kitten一字转成sitting：
sitten （k->s）
sittin （e->i）
sitting （->g）
所以kitten和sitting的编辑距离是3。俄罗斯科学家Vladimir Levenshtein在1965年提出这个概念。
给出两个字符串a,b，求a和b的编辑距离。
Input
第1行：字符串a(a的长度 <= 1000)。 第2行：字符串b(b的长度 <= 1000)。
Output
输出a和b的编辑距离
Sample Input

```cpp
kitten
sitting
```

Sample Output

```cpp
3
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109124114250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzI3MjI1,size_16,color_FFFFFF,t_70#pic_center)

根据图可知，我们只需要不断的判断最后一对，取得最小距离，则可取得全部字符串的最小距离，

假设d[i][j]为s1的前 i 个字符 到s2的前j个字符的距离

则d[i][j]的情况有以下几种情况：
1.s1[i]==s2[j]  -> d[i][j]=d[i-1][j-1]
2.s1[i]!=s2[j] 且 s1.len<s2.len ，这种情况因为长度不相等，则不可以一直i-1,j-1,因为d[i][j]的i,j是任意不超过字符串长度的数值，则可以根据图一的第一例，d[i][j]=d[i][j-1]+1
3.同理2，s1[i]!=s2[j] 且 s1.len>s2.len  则d[i][j]=d[i-1][j]+1;
4.s1[i]！=s2[j]  且s1.len ==  s2.len   则d[i][j]=d[i-1][j-1]+1

综合上述，d[i][j]=min(d[i][j-1]+1,d[i-1][j]+1,d[i-1][j-1]+(s1[i]==s2[j] ? 0 : 1));


思路到这里就可以开始做题了，
我们需要计算d[i][j]  1<=i=s1.len  && 1<=j<=s2.len
则需要知道d[0][i],d[j][0]的距离
d[][]数值初始化如下

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201109125935243.png#pic_center)


AC代码：
```cpp
#include<iostream>
#include<string.h>
using namespace std; 
int minDistance(string word1, string word2) {
        int l1=word1.length();
        int l2=word2.length();

        if(l1*l2==0){//其中有零，返回另一个的长度 
            return (l1+l2);
        }
        int dp[l1+1][l2+1];//初始化dp0为0 
        for(int i=0;i<=l2;++i){
            dp[i][0]=i;
        }
        for(int j=0;j<=l1;++j){
            dp[0][j]=j;
        }
        for(int i=1;i<=l1;++i){
            for(int j=1;j<=l2;++j){
                int l=dp[i-1][j]+1;
                int d=dp[i][j-1]+1;
                int r=dp[i-1][j-1];
                if(word1[i-1]!=word2[j-1]){
                    r+=1;
                }
                dp[i][j]=min(l,min(d,r));
            }
        }
        return dp[l1][l2];
    }
int main(){
    	
    	string a,b;
    	cin>>a>>b;
		int res=minDistance(a,b);
    	cout<<res<<endl;
    	
	}
```

