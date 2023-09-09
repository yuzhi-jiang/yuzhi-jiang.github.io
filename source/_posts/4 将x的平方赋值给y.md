---
title: 4 将x的平方赋值给y
categories: []
date: 2019-11-04 22:10:46
tags:
---

<!--more-->

4 将x的平方赋值给y

假设x的值为3，计算x的平方并赋值给y，分别以“y = x ∗ x”和“x ∗ x = y”的形式输出x和y的值。

输入格式：

本题无输入

输出格式：

按照下列格式输出代入x=3的结果：

```c
y = x * x
x * x = y
```

分析：如题，基础赋值题。

```c
#include<stdio.h>
int main()
{
 int x = 3;
 int y = x*x;
 printf ("%d = %d * %d\n%d * %d = %d",y,x,x,x,x,y);
 return 0;
}
```

~~不想打这么多也可以这样：~~ 

```c
#include<stdio.h>
int main()
{
 printf ("9 = 3 * 3\n3 * 3 = 9");
 return 0;
}
```

