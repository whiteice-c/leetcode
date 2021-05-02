训练营同学来自n（1≤n≤105）个城市，每个城市的同学人数分别为a1,a2,…,an（1≤an≤109）。小Z将派出n辆车迎接同学。同一个城市的同学需要坐同一辆车，一辆车只能载来自同一个城市的同学，一辆车只能跑一次，且不能超过每辆车的载客量b1,b2, …, bn（1≤bn≤109）。请问有多少种不同的方式把这n个城市的同学安排到这n辆车？



```
4
1 2 3 4
2 4 3 4
```

```
8
```



```c++
#include<iostream>
#include<stdio.h>
#include<math.h>
#include<string>
#include<string.h>
#include<algorithm>
using namespace std;
#define ll long long 
const int maxn = 1e5+10;
const ll mod = 100000007;
ll ans=1;
ll a[maxn];
ll b[maxn];
int n;
int get_id(ll x)
{
	int lb=1,ub=n,mid,ans=-1;
	while(ub>=lb)
	{
		mid=(lb+ub)/2;
		if(b[mid]>=x)
		{
			ans=mid; ub=mid-1;
		}
		else lb=mid+1;
	}	
	return ans;
}
int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++) scanf("%lld",&a[i]);
	for(int i=1;i<=n;i++) scanf("%lld",&b[i]);
	sort(a+1,a+1+n);
	sort(b+1,b+1+n);
	ll nd=1;
	for(int i=n;i;i--)
	{
		ll pos=(ll)get_id(a[i]);
		if(pos==-1)
		{
			ans=0;
			break;
		}
		pos=n-pos+1;
		if(pos<nd)
		{
			ans=0;
			break;	
		}	
		ans=ans*(pos-(nd-1))%mod;
		nd++;
	}	
	printf("%lld",ans);
	return 0;
}

```





小Z组织训练营同学进行一次拔河比赛，要从n（2≤n≤60,000）个同学中选出两组同学参加（两组人数可能不同）。对每组同学而言，如果人数超过1人，那么要求该组内的任意两个同学的体重之差的绝对值不超过k（包含k）。问最多有几个同学能参加比赛？

```
5 2
20
3
16
9
5
```

```
3
```



```c++
#include<iostream>
#include<stdio.h>
#include<math.h>
#include<string>
#include<string.h>
#include<algorithm>
using namespace std;
#define ll long long 
const int maxn = 1e5+10;
const ll mod = 100000007;
int n;
ll a[maxn];
ll k;
ll Max[maxn];
ll ans;
ll mMax[maxn];
void wk(int x)
{
	int mid,bns=x,lb=x,ub=n;
	while(ub>=lb)
	{
		mid=(lb+ub)/2;
		if(a[mid]-a[x]<=k)
		{
			bns=mid; lb=mid+1;
		}
		else ub=mid-1;
	}
	Max[x]=bns;
}
int main()
{
	scanf("%d%lld",&n,&k);
	for(int i=1;i<=n;i++) scanf("%lld",&a[i]);
	sort(a+1,a+1+n);
	for(int i=1;i<=n;i++) wk(i);
	for(int i=n;i;i--) mMax[i]=max(mMax[i+1],Max[i]-i+1);
	for(int i=1;i<=n;i++)
	{
		ans=max(ans,(Max[i]-i+1) + mMax[(int)Max[i]+1]);
	}
	printf("%lld",ans);
	return 0;
}

```





小Z为训练营同学预订了n（1≤n≤500）个回程航班，每个航班的同学数依次为a1,a2,…,an(1≤ai≤106)。现在只有一个司机，司机需要严格按照航班顺序依次将第1到第n个航班的同学送往机场。该司机可从租车公司租到任意载客量的车，但是最多只能租g次(1<=g<=n)，并且每次只能租1辆。假设司机用载客量为m的车送ai个同学到机场（当然要求不超载），那么就造成了m-ai的载量浪费。请问该司机送完所有批次同学到机场后，最低的载量浪费是多少？

```
6 3
7 9 8 2 3 2
```

```
3
```



```c++
#include<iostream>
#include<stdio.h>
#include<math.h>
#include<string>
#include<string.h>
#include<algorithm>
using namespace std;
#define ll long long 
const int maxn = 510;
const ll mod = 100000007;
const int INF = 1e9;
int n,m;
int a[maxn];
int sum[maxn],Max[maxn][maxn];
int dp[maxn][maxn];
int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++) scanf("%d",&a[i]);
	for(int i=1;i<=n;i++) sum[i]=sum[i-1]+a[i];
	for(int i=1;i<=n;i++)	
	{
		Max[i][i]=a[i];
		for(int j=i+1;j<=n;j++)
		Max[i][j]=max(Max[i][j-1],a[j]);
	}
	for(int i=0;i<maxn;i++)
		for(int j=0;j<maxn;j++)
			dp[i][j]=INF;
	dp[0][0]=0;
	for(int i=1;i<=n;i++)
	{
		for(int j=0;j<m;j++)
		{
			for(int k=0;k<i;k++)
				dp[i][j+1]=min(dp[i][j+1],dp[k][j]+ Max[k+1][i]*(i-k) - (sum[i]-sum[k]) );	
		}
	}
	int ans=INF;
	for(int j=0;j<=m;j++) ans=min(ans,dp[n][j]);
	printf("%d",ans);
	return 0;
}

```

