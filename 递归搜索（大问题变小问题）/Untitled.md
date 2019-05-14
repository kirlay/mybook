# P1101 单词方阵

## 题目描述

给一n*×*n*的字母方阵，内可能蕴含多个“`yizhong`”单词。单词在方阵中是沿着同一方向连续摆放的。摆放可沿着 88 个方向的任一方向，同一单词摆放时不再改变方向，单词与单词之间可以交叉,因此有可能共用字母。输出时，将不是单词的字母用`*`代替，以突出显示单词。例如：

```cpp
输入：
    8                     输出：
    qyizhong              *yizhong
    gydthkjy              gy******
    nwidghji              n*i*****
    orbzsfgz              o**z****
    hhgrhwth              h***h***
    zzzzzozo              z****o**
    iwdfrgng              i*****n*
    yyyygggg              y******g
```

## 输入输出格式

输入格式：



第一行输入一个数n*n*。(7 \le n \le 1007≤*n*≤100)。

第二行开始输入n \times n*n*×*n*的字母矩阵。



输出格式：



突出显示单词的n*×*n*矩阵。



## 输入输出样例

输入样例#1：

 

复制

```
7
aaaaaaa
aaaaaaa
aaaaaaa
aaaaaaa
aaaaaaa
aaaaaaa
aaaaaaa
```

输出样例#1：

 

复制

```
*******
*******
*******
*******
*******
*******
*******
```

输入样例#2：

 

复制

```
8
qyizhong
gydthkjy
nwidghji
orbzsfgz
hhgrhwth
zzzzzozo
iwdfrgng
yyyygggg
```

输出样例#2：

 

复制

```
*yizhong
gy******
n*i*****
o**z****
h***h***
z****o**
i*****n*
y******g
```



### 思路

先输入，然后一个一个点暴力过去，每个点都搜索八个方向，如果可以，就标记，输出时有标记的输出原来的字母，不然输出“*”



### 学生代码

```c++
#include<iostream>
#include<cstdio>
using namespace std;
int n,b[150][150];
int dx[9]={0,-1,-1,-1,0,1,1,1},
	dy[9]={-1,-1,0,1,1,1,0,-1};
char a[150][150];//yizhong
void bfs(int x,int y)
{
	for(int i=0;i<8;i++)
	{
		if(a[x+dx[i]][y+dy[i]]=='y'&&a[x+2*dx[i]][y+2*dy[i]]=='i'&&
		   a[x+3*dx[i]][y+3*dy[i]]=='z'&&a[x+4*dx[i]][y+4*dy[i]]=='h'&&
		   a[x+5*dx[i]][y+5*dy[i]]=='o'&&a[x+6*dx[i]][y+6*dy[i]]=='n'&&
		   a[x+7*dx[i]][y+7*dy[i]]=='g')
		{
			b[x+dx[i]][y+dy[i]]++;
			b[x+2*dx[i]][y+2*dy[i]]++;
			b[x+3*dx[i]][y+3*dy[i]]++;
			b[x+4*dx[i]][y+4*dy[i]]++;
			b[x+5*dx[i]][y+5*dy[i]]++;
			b[x+6*dx[i]][y+6*dy[i]]++;
			b[x+7*dx[i]][y+7*dy[i]]++;
		}
	}
}
int main()
{
	cin>>n;
	for(int i=1;i<=n;i++)
		for(int j=1;j<=n;j++)
			cin>>a[i][j];
	for(int i=0;i<=n+1;i++)
		for(int j=0;j<=n+1;j++)
			bfs(i,j);  //我觉得这里可以根据b数组进行优化。
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
			if(b[i][j]!=0) cout<<a[i][j];
			else cout<<"*";
		cout<<endl;
	}
	return 0;
}
```



### 学生代码2

```c++
#include<iostream>
using namespace std;
int cnt,n;
char c[105][105],map[8]={' ','y','i','z','h','o','n','g'},t[105][105];
int dx[9]={0,-1,-1,0,1,1,1,0,-1},
	dy[9]={0,0,1,1,1,0,-1,-1,-1};
void dfs(int x,int y)
{
	for(int i=1;i<=8;i++)
	{
		int tx=dx[i]+x;
		int ty=dy[i]+y;
		if(c[tx][ty]!=map[2]) continue;
		bool flag=true;
		for(int j=3;j<=7;j++)
		{
			if(c[dx[i]*(j-2)+tx][dy[i]*(j-2)+ty]!=map[j])
			{
				flag=false;
				break;
			}
		}
		if(flag==true)
		{
			t[x][y]='*';
			t[tx][ty]='*';
			for(int j=3;j<=7;j++)
			{
				t[tx+dx[i]*(j-2)][ty+dy[i]*(j-2)]='*';
			}
		}
	}
	return ;
}
int main()
{
	cin>>n;
	for(int i=1;i<=n;i++)
		for(int j=1;j<=n;j++)
			cin>>c[i][j];
			
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
		{
			if(c[i][j]=='y')
			{
				dfs(i,j);
			}
		}
	}
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=n;j++)
		{
			if(t[i][j]!='*') cout<<'*';
			else cout<<c[i][j];
		}
		cout<<endl;
	}
	return 0;
} 
```





