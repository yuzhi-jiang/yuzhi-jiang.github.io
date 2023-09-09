---
title: Connect the Cities（连接城市）
tags: 算法
categories: []
date: 2020-12-12 17:12:30
---



In 2100, since the sea level rise, most of the cities disappear. Though some survived cities are still connected with others, but most of them become disconnected. The government wants to build some roads to connect all of these cities again, but they don’t want to take too much money.  

Input:
The first line contains the number of test cases.
Each test case starts with three integers: n, m and k. n (3 <= n <=500) stands for the number of survived cities, m (0 <= m <= 25000) stands for the number of roads you can choose to connect the cities and k (0 <= k <= 100) stands for the number of still connected cities.
To make it easy, the cities are signed from 1 to n.
Then follow m lines, each contains three integers p, q and c (0 <= c <= 1000), means it takes c to connect p and q.
Then follow k lines, each line starts with an integer t (2 <= t <= n) stands for the number of this connected cities. Then t integers follow stands for the id of these cities.

output:
For each case, output the least money you need to take, if it’s impossible, just output -1.

Sample Input

```cpp
1
6 4 3
1 4 2
2 6 1
2 3 5
3 4 33
2 1 2
2 1 3
3 4 5 6
```
Sample Output

```cpp
1
```


翻译：

```cpp
2100年，由于海平面上升，大部分城市消失了。虽然一些幸存下来的城市仍然与其他城市相连，但大多数城市已经失去联系。政府想再建一些道路来连接所有这些城市，但他们不想花太多钱。
输入
第一行包含测试用例的数量。
每个测试用例以三个整数开始：n，m和k。n（3<=n<=500）代表幸存城市的数量，m（0<=m<=25000）代表可以选择连接城市的道路数量，k（0<=k<=100）代表仍然连接的城市的数量。
为了方便起见，这些城市的签约从1到n。
然后沿着m行，每行包含三个整数p，q和c（0<=c<=1000），意味着p和q需要c来连接。
然后沿着k条线，每一条线以一个整数t（2<=t<=n）表示这个连通的城市的数量。然后t整数表示这些城市的id。
输出
对于每种情况，输出你需要的最少的钱，如果不可能，只输出-1。

```

```cpp
思路：
如果把每个已经可以连通的城市围起来看作一个，并且它们之间的距离为0
这样，剩下来的，不就是一个个点。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201212170732185.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzI3MjI1,size_16,color_FFFFFF,t_70#pic_center)

这样只要选择其中一个起始点作为目标集合，每次从其他点，选择一个最小的到目标集合的点，并且记录价格，放入集合，循环，直到全部放进去

```cpp
#include<iostream>
using namespace std;
#define INF 0x3f3f3f3f
int n,m,k;
int dic[550]={0};
int map[550][550]={0};
int vis[550]={0};
int bs[550];
int gets(){
	int res,cont,tmin,indmin;
	res=cont=0;
	for(int i=1;i<=n;++i){
		dic[i]=map[1][i];//默认初始为1号节点 ，即默认把1号放进铺设集合 
		vis[i]=0;
	} 
	vis[1]=1; 
	dic[1]=0;
	for(int i=1;i<n;++i){//一共执行n-1次最小值 才能把所有的点加入集合 
		tmin=INF;//最小值 
		for(int j=1;j<=n;++j){
			//find min
			if(dic[j]<tmin&&!vis[j]){
				tmin=dic[j];
				indmin=j;//下标 
			}
		}
		if(tmin==INF)
			break;
		// min
		//	cout<<"indmin="<<indmin<<" min="<<tmin<<" res="<<res<<endl;
			vis[indmin]=1;
			dic[indmin]=0;
			res+=tmin;
			cont++; 
		//修改  dic[min][j]  集合外到集合的距离 
			for(int j=1;j<=n;++j){
				if(!vis[j]&&map[indmin][j]<dic[j])
					dic[j]=map[indmin][j];
			}
		
	}
	if(tmin==INF)
		return -1;
	return res; 
}
inline void init()
{
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
		{
			map[i][j]=INF;
		}
		map[i][i]=0;
	}
}
int main(){
	ios::sync_with_stdio(false);
	int a,b,val,num;
	int T;
	scanf("%d",&T);
	while(T--){
		scanf("%d%d%d",&n,&m,&k);
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				map[i][j]=INF;
			}
			map[i][i]=0;
		}
	
		
		for(int i=0;i<m;++i){
			scanf("%d%d%d",&a,&b,&val);
			if(map[a][b]>val){
				map[a][b]=map[b][a]=val;	
			}
		}
		
		
		for(int i=0;i<k;++i){
			scanf("%d",&num);
			for(int j=1;j<=num;++j){
				scanf("%d",&bs[j]);
			
			}
			for(int x=1;x<=num;++x){
				for(int y=1;y<=num;++y){
					map[bs[x]][bs[y]]=0;//该区域的花销为0 
				}
			}
		}
		//int res=gets();
		cout<<gets()<<endl;
		
	}
	return 0;
} 
```

