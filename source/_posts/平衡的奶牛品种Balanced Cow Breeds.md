---
title: 平衡的奶牛品种Balanced Cow Breeds
tags:
- dp
---

<!--more-->

所以，我用了dp转移，开两个状态：

      前一维代表 H 还没匹配的前括号数；
      后一维代表 G 还没匹配的前括号数；
再把空间滚一滚，讨论一下是  ‘（’  还是    ‘)’   ,放  H  还是  G  ，大力转移；

具体见代码（蒟蒻第一篇题解，希望能过qwq）；（有点丑不要介意）

```cpp
#include<iostream>
#include<cstring>
#include<cmath>
#include<cstdio>
using namespace std;
const int mod=2012;
char s[1005];
int n,f[1005][1005],g[1005][1005];//f是之前的状态，g是当前的状态。
int main()
{
	scanf("%s",s+1);
	n=strlen(s+1);
	int oo=0;
	f[0][0]=1;//开始自然一个都没有。。
	for(int i=1;i<=n;++i)
	{
		if(s[i]=='(')
		{
			oo++;//oo维护还没匹配的左括号总数。
			for(int j=0;j<=oo;++j)
			{//j枚举的是 H 的左括号还没匹配的数量。
				if(j) g[j][oo-j]=(g[j][oo-j]+f[j-1][oo-j])%mod;//左括号变成 H
				if(oo-j) g[j][oo-j]=(g[j][oo-j]+f[j][oo-j-1])%mod;//左括号变成 G
			}
		}
		else if(s[i]==')')
		{
			oo--;//同上
			for(int j=0;j<=oo;++j)
			{//同上
				g[j][oo-j]=(g[j][oo-j]+f[j+1][oo-j])%mod;//一个 H 被匹配掉了
				g[j][oo-j]=(g[j][oo-j]+f[j][oo-j+1])%mod;//一个 G 被匹配掉了
			}
		}
		for(int j=0;j<=oo;++j) f[j][oo-j]=g[j][oo-j],g[j][oo-j]=0;//滚存一下
	}
	printf("%d",f[0][0]);//最后肯定没有不匹配的，所以直接输出f[0][0]。
	return 0;
}
```