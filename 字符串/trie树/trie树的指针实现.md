### trie树的指针实现

#### 1471：【例题1】Phone List

**【题目描述】**
原题来自：POJ 3630
给定 n 个长度不超过 10 的数字串，问其中是否存在两个数字串 S,T，使得 S 是 T 的前缀，多组数据。
**【输入】**
第一行一个整数 T，表示数据组数。
对于每组数据，第一行一个数 n，接下来 n 行输入 n 个数字串。
**【输出】**
对于每组数据，若存在两个数字串 S，T，使得 S 是 T 的前缀，则输出 NO ，否则输出 YES 。
请注意此处结果与输出的对应关系！
**【输入样例】**
`2
3
911
97625999
91125426
5
113
12340
123440
12345
98346`
**【输出样例】**
`NO
YES`
**【提示】**
数据范围：
对于 100% 的数据，1≤T≤40,1≤n≤104 。
【来源】
http://ybt.ssoier.cn:8088/problem_show.php?pid=1471

### 思路

（1）先构建一棵trie树，这是一棵10叉树,如下面的程序insert，每次单词的结束会涂上一个颜色

（2）按照每个单词的路径，去查询一次find，在查询的路径中有2个以上涂过颜色的点，那么就返回true了

（3）因为有多组测试数据，所以每组数据都建立的一棵树，用完以后要把树删除del_tree



### 源代码

```c++
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
int t,n;
bool ans;
string s1,a[10005];
struct Node
{
	bool is_col;
	Node* ch[10];
};
Node *root,*nil;
void insert()
{
	int len=s1.size();
	Node *p=root;
	for(int i=0;i<len;i++)
	{
		int bh=s1[i]-'0';
		if(p->ch[bh]==nil)
		{
			p->ch[bh]=new Node;
			p->ch[bh]->is_col=false;
			for(int j=0;j<10;j++)
			{
				p->ch[bh]->ch[j]=nil;
			}
		}
		p=p->ch[bh];
	}
	p->is_col=true;
}
int find(string s){
	int len=s.size();
	int ans=0;
	Node * p=root;
	for(int i=0;i<len;i++){
			int bh=s[i]-'0';
			if(p->ch[bh]->is_col==true) ans++;
			p=p->ch[bh];
	}
	return ans;
}
bool search(){
	for(int i=1;i<=n;i++){
		int cnt=find(a[i]);
		if(cnt>=2) return true;
	}
	return false;
}
void solve()
{
	root=new Node;
	nil=new Node;
	root->is_col=false;
	for(int i=0;i<10;i++)
	{
		root->ch[i]=nil;
	}
	ans=false;
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		cin>>s1;
		a[i]=s1;
		insert();
	}
	if(search())
		{
			printf("NO\n");
			
		}else
	printf("YES\n");
}
void del_tree(Node * p){
	if(p==nil) return;
	for(int i=0;i<10;i++)
	{
		del_tree(p->ch[i]);
	}
	delete p;
}
int main()
{
	scanf("%d",&t);
	for(int i=1;i<=t;i++)
	{	
		del_tree(root);
		solve();
	}
	return 0;
}
```

