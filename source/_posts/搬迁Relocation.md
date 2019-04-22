---
title: 搬迁Relocation
tags:
- 最短路
- 状压
- dp
---

<!--more-->

##  最短路+状压dp

由于我们看到他的k很小（1<=k<=5）就想到了~~爆搜~~状压

我们肯定要知道k个城镇与每个点之间的距离，最短路就跑k遍；

枚举选的农场，开始状压；

至于状压：
```
    	f [当前状态为i (二进制中，1表示走过)]  [当前所在的城镇为j]
    	= f[i^(1<<(j-1))][p] + dis[p][j] (p为之前所在的城镇)
    	
    	初始时：f[1<<(k-1)][k] = dis[k][s](s为选的农场，k为第一个经过的城镇)
    	
    	最后枚举最后所在的城镇，加上他到选的农场的距离，取最小值；
```

具体见代码，大佬勿喷qwq。。。

```cpp
#include<iostream>
#include<cstdio>
#include<bits/stdc++.h>
using namespace std;
long long n,m,k,wz[6];
long long tot,head[10005],nx[100005],to[100005],w[100005];
long long dis[6][10005],vis[10005],f[2005][6];
void jia(long long aa,long long bb,long long cc)
{
	tot++;
	nx[tot]=head[aa];
	to[tot]=bb;
	w[tot]=cc;
	head[aa]=tot;
	return;
}
struct node
{
	long long id,val;
};
bool operator <(node aa,node bb)
{
	return aa.val>bb.val;
}
node bh(long long aa,long long bb)
{
	node tt;
	tt.id=aa;
	tt.val=bb;
	return tt;
}
void spfa(long long rt)//其实是dijkstra。。。
{
	priority_queue<node>q;
	q.push(bh(wz[rt],0));
	memset(vis,0,sizeof(vis));
	dis[rt][wz[rt]]=0;
	while(!q.empty())
	{
		node kk=q.top();
		q.pop();
		if(vis[kk.id]) continue;
		vis[kk.id]=1;
		for(long long i=head[kk.id];i;i=nx[i])
		{
			long long yy=to[i];
			if(dis[rt][yy]>dis[rt][kk.id]+w[i])
			{
				dis[rt][yy]=dis[rt][kk.id]+w[i];
				q.push(bh(yy,dis[rt][yy]));
			}
		}
	}
}
int main()
{
	memset(dis,0x3f3f3f3f,sizeof(dis));
	scanf("%lld%lld%lld",&n,&m,&k);
	for(long long i=1;i<=k;++i)
	{
		scanf("%lld",&wz[i]);
	}
	for(long long i=1;i<=m;++i)
	{
		long long x,y,z;
		scanf("%lld%lld%lld",&x,&y,&z);
		jia(x,y,z);
		jia(y,x,z);
	}
	for(long long i=1;i<=k;++i)
	{
		spfa(i);//预处理k个城镇到各点的距离
	}
	long long ans=0x3f3f3f3f;
	for(long long i=1;i<=n;++i)//枚举选的农场
	{
		long long flag=0;
		for(long long j=1;j<=k;++j) if(i==wz[j]) {flag=1;break;}
		if(flag) continue;//城镇不能选，不是吗？
		memset(f,0x3f3f3f,sizeof(f));
		for(long long j=1;j<=k;++j)
		{
			f[1<<(j-1)][j]=dis[j][i];//见上初始时
		}
		for(long long j=1;j<(1<<k);++j)
		{
			for(long long pp=1;pp<=k;++pp)//枚举当前所在的城镇
			if(j&(1<<(pp-1)))
			{
				for(long long qq=1;qq<=k;++qq)//枚举之前所在的城镇
				if(pp!=qq&&(j&(1<<(qq-1))))
				{
					f[j][pp]=min(f[j][pp],f[j^(1<<(pp-1))][qq]+dis[qq][wz[pp]]);//转移
				}
			}
		}
		for(long long j=1;j<=k;++j)//枚举最后所在的城镇
		{
			ans=min(ans,f[(1<<k)-1][j]+dis[j][i]);//取最小值
		}
	}
	printf("%lld",ans);
	return 0;
}
```
完结撒花。。。

```

```