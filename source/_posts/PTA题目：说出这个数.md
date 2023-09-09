---
title: PTA题目：说出这个数
tags: pta
categories: []
date: 2019-10-13 13:57:23
---



## 读入一个正整数 n，计算其各位数字之和，用汉语拼音写出和的每一位数字。
输入格式：
每个测试输入包含 1 个测试用例，即给出自然数 n 的值。这里保证 n 小于 10​100​​。
输出格式：
在一行内输出 n 的各位数字之和的每一位，拼音数字间有 1 空格，但一行中最后一个拼音数字后没有空格。
输入样例：
1234567890987654321123456789

输出样例：
yi san wu

代码如下：

```c
#include<stdio.h>
#include<math.h>
void a(int a);//说出a代表的数字
int main(void){
 char s[101];//最多10^100位。
 scanf("%s",&s);
 int sum=0;//存和 
 int b[10];//用于存sum的每位数之和
 int i=0,j=0;
 while(s[i]!='\0'){//计算和
  sum+=s[i]-'0';//数据类型转换为int,并计算和
  i++;
 } 
 if(sum==0){//如果和为0；说出0，结束
  a(0);
  return 0;
 }
 while(sum!=0){//不为0,则将sum拆解并存在数组中
  b[j]=sum%10;
  sum/=10;
  j++;
 }
 for(int x=j-1;x>0;--x){//遍历并说出这个数
      a(b[x]);
      printf(" ");
  }
 a(b[0]);
 return 0;
}
void a(int a)
 if(a>=0){
  switch(a){
   case 1:
    printf("壹"); break;
   case 2:
    printf("贰"); break;
   case 3:
    printf("叁"); break;
   case 4:
    printf("肆"); break;
   case 5:
    printf("伍"); break;
   case 6:
    printf("陆"); break;
   case 7:
    printf("柒"); break;
   case 8:
    printf("捌"); break;
   case 9:
    printf("玖"); break;
   case 0:
    printf("拾"); break;
  }
 }
}
```

