---
title: Dijkstra算法各种实现
tags: 算法
categories: []
date: 2020-10-26 20:57:15
---

<!--more-->

首先了解下什么是最短路径算法-----Dijkstra算法详解
来一条传送门-> [最短路径问题---Dijkstra算法详解](https://blog.csdn.net/qq_35644234/article/details/60870719) 



看到这里相信你已经知道了Dijkstra算法的大致思路

先直接上代码

第一种实现，用队列实现，优化
```
//start 起点，len点集个数，map 地图的二维数组，dis 起点到其的距离
void dijkstra(int start,int len,int map[][N],int dis[]){
	int vis[len];
	queue<int>q;//队列，存剩余的节点 
	//初始化
	for(int i=0;i<len;i++){
		dis[i]=INF;
		vis[i]=false;
	}
	//第一个点
	dis[start]=0;
	vis[start]=true;
	q.push(start);
	while(!q.empty()){	
		int MIN=INF;
		int mindex;
		int u=q.front();q.pop();//当前最小值
		for(int i=0;i<n;++i){//遍历，修改dis，同时找到修改后的最小值
			if(map[u][i]!=INF&&vis[i]!=true){
				int temp=map[u][i]+dis[u];
				if(dis[i]>temp){
					dis[i]=temp;
				}
				if(dis[i]<MIN){
					mindex=i;
					MIN=dis[i];
				}
			}
		}
		if(MIN==INF){//没有找到最小值
			break; 
		}
		q.push(mindex);//将最小值放入队列
		vis[mindex]=true;//设为已经访问
	}
}

```
第二种实现，再第一种的基础上，修改参数
```
//start 起点，len点集个数，map 地图的二维数组，dis 起点到其的距离，vis，有没有被选择，用过，用过，说明就是知道起点到该点的最短路径
void dijkstra(int start,int len,int map[][N],int dis[],int vis[]){
	queue<int>q;//队列，存剩余的节点 
	for(int i=0;i<len;i++){
		dis[i]=INF;vis[i]=false;
	}
	dis[start]=0;
	vis[start]=true;
	q.push(start);
	while(!q.empty()){	
		int MIN=INF;
		int mindex;
		int u=q.front();q.pop();
		for(int i=0;i<n;++i){
			if(map[u][i]!=INF&&vis[i]!=true){
				int temp=map[u][i]+dis[u];
				if(dis[i]>temp){
					dis[i]=temp;
				}
				if(dis[i]<MIN){
					mindex=i;
					MIN=dis[i];
				}
			}
		}
		if(MIN==INF){
			break; 
		}
		q.push(mindex);
		vis[mindex]=true;
	}
}
```




第三种实现，只用数组
```
//start 起点，len点集个数，map 地图的二维数组，dis 起点到其的距离，vis，有没有被选择，用过，用过，说明就是知道起点到该点的最短路径
void dijkstra(int start,int len,int map[][N],int dis[],int vis[]){
	for (int i = 0; i <len; ++i)
		dis[i]=map[start][i];
	vis[start]=1;
	dis[start]=0;
	for (int i =0; i<len; ++i){
		int mindex;
		int MIN=INF;
		for(int j=0;j<len;j++){
			if(vis[j]==0&&dis[j]<MIN){
				mindex=j;
				MIN = dis[j];
			}
		}
		if(MIN==INF)
			break;			//如果是没有路径，他就不在返回了
		vis[mindex]=1;
		for(int j=0;j<len;j++){
			int temp=map[mindex][j]+dis[mindex];
			if(vis[j]==0&&temp<dis[j]){
				dis[j]=temp;	
			}
		}
	}
}
```
来一个题：最短路径

```
描述
万圣节的早上，小Hi和小Ho在经历了一个小时的争论后，终于决定了如何度过这样有意义的一天——他们决定去闯鬼屋！

在鬼屋门口排上了若干小时的队伍之后，刚刚进入鬼屋的小Hi和小Ho都颇饥饿，于是他们决定利用进门前领到的地图，找到一条通往终点的最短路径。

鬼屋中一共有N个地点，分别编号为1..N，这N个地点之间互相有一些道路连通，两个地点之间可能有多条道路连通，但是并不存在一条两端都是同一个地点的道路。那么小Hi和小Ho至少要走多少路程才能够走出鬼屋去吃东西呢？

提示：顺序！顺序才是关键。
×Close
提示：顺序！顺序才是关键。
小Ho想了想说道：“唔……我觉得动态规划可以做，但是我找不到计算的顺序，如果我用f[i]表示从S到达编号为i的节点的最短距离的话，我并不能够知道f[1]..f[N]的计算顺序。”

“所以这个问题不需要那么复杂的算法啦，我就稍微讲讲你就知道了！”小Hi道：“路的长度不可能为负数对不对？”

“那是自然，毕竟人类还没有发明时光机器……”小Ho点点头。

于是小Hi问道：“那么如果就看与S相邻的所有节点中与S最近的那一个S'，并且从S到S'的距离为L，那么有可能存在另外的道路使得从S到S'的距离小于L么？”

“不能，因为S'是与S相邻的所有节点中与S最近的节点，那么从S到其他相邻点的距离一定是不小于L的，也就是说无论接下来怎么走，回到L点时总距离一定大于L。”小Ho思考了一会，道。

“也就是说你已经知道了从S到S'的最短路径了是么？”小Hi继续问道。

“是的，这条最短路径的长度是L。”小Ho答道。

小Hi继续道：“那么现在，我们不妨将S同S'看做一个新的节点？称作S1，然后我就计算与S相邻或者与S'相邻的所有节点中，与S最近的哪一个节点S''。注意，在这个过程中，与S相邻的节点与S的距离在上一步就已经求出来了，那么我要求的只有与S'相邻的那些节点与S的距离——这个距离等于S与S'的距离加上S'与这些结点的距离，对于其中重复的节点——同时与S和S'相邻的节点，取两条路径中的较小值。”

小Ho点了点头：“那么同之前一样，与S1（即S与S'节点）相邻的节点中与S'距离最近的节点如果是S''的话，并且这个距离是L2，那么我们可以知道S到S''的最短路径的长度便是L2，因为不可能存在另外的道路比这个更短了。”

于是小Hi总结道：“接下来的问题不就很简单了么，只需要以此类推，每次将与当前集合相邻（即与当前集合中任意一个元素）的所有节点中离S最近的节点（这些距离可以通过上一次的计算结果推导而出）选出来添加到当前集合中，我就能够保证在每一个节点被添加到集合中时所计算的离S的距离是它与S之间的最短路径！”

“原来是这样！但是我的肚子更饿了呢！”言罢，小Ho的肚子咕咕叫了起来。

Close
输入
每个测试点（输入文件）有且仅有一组测试数据。

在一组测试数据中：

第1行为4个整数N、M、S、T，分别表示鬼屋中地点的个数和道路的条数，入口（也是一个地点）的编号，出口（同样也是一个地点）的编号。

接下来的M行，每行描述一条道路：其中的第i行为三个整数u_i, v_i, length_i，表明在编号为u_i的地点和编号为v_i的地点之间有一条长度为length_i的道路。

对于100%的数据，满足N<=10^3，M<=10^4, 1 <= length_i <= 10^3, 1 <= S, T <= N, 且S不等于T。

对于100%的数据，满足小Hi和小Ho总是有办法从入口通过地图上标注出来的道路到达出口。

输出
对于每组测试数据，输出一个整数Ans，表示那么小Hi和小Ho为了走出鬼屋至少要走的路程。

Sample Input
5 23 5 4
1 2 708
2 3 112
3 4 721
4 5 339
5 4 960
1 5 849
2 5 98
1 4 99
2 4 25
2 1 200
3 1 146
3 2 106
1 4 860
4 1 795
5 4 479
5 4 280
3 4 341
1 4 622
4 2 362
2 3 415
4 1 904
2 1 716
2 5 575

Sample Output
123
```
AC代码：

```
#include<iostream>
#include<algorithm>
#include<queue>
#include<string.h>
using namespace std;
const int INF=0x3f3f3f3f;//无限大
const int N=1005;
int map[N][N];//地图 
int dist[N];//最短距离
int n,m;
void dijkstra(int start,int len,int g[][N],int dis[])
{
	int vis[len];
	queue<int>q;//队列，存剩余的节点 
	for(int i=0;i<len;i++){
		dis[i]=INF;
		vis[i]=false;
	}
	dis[start]=0;
	vis[start]=true;
	q.push(start);
	while(!q.empty()){	
		int MIN=INF;
		int mindex;
		int u=q.front();q.pop();
		for(int i=0;i<n;++i){
			if(map[u][i]!=INF&&vis[i]!=true){
				int temp=map[u][i]+dis[u];
				if(dis[i]>temp){
					dis[i]=temp;
				}
				if(dis[i]<MIN){
					mindex=i;
					MIN=dis[i];
				}
			}
		}
		if(MIN==INF){
			break; 
		}
		q.push(mindex);
		vis[mindex]=true;
	}
}
int main(){
	int s,t,a,b,c;
	while(scanf("%d%d%d%d",&n,&m,&s,&t)==4&&n&&m){
		memset(map,0x7f,sizeof(map));
			for(int i=0;i<m;++i){
				scanf("%d %d %d",&a,&b,&c);
				a--;
				b--;
				map[a][b]=map[b][a]=min(map[a][b],c);
			}
			dijkstra(--s,n,map,dist);
			printf("%d\n",dist[--t]);
	}
	return 0;
}
```

还有其他实现，有待继续更新，以上是兼容性比较好的，拿来一个函数直接用，传入参数，不需要考虑太多


