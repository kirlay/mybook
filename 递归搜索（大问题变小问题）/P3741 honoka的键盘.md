# P3741 honoka的键盘

## 题目背景

honoka 有一个只有两个键的键盘。

## 题目描述

一天，她打出了一个只有这两个字符的字符串。当这个字符串里含有"VK"这个字符串的时候，honoka 就特别喜欢这个字符串。所以，她想改变至多一个字符（或者不做任何改变）来最大化这个字符串内"VK"出现的次数。给出原来的字符串，请计算她最多能使这个字符串内出现多少次"VK"。(只有当"V"和"K"正好相邻时，我们认为出现了"VK"。)

## 输入输出格式

输入格式：



第一行给出一个数字 n,代表字符串的长度。第二行给出一个字符串 s。



输出格式：



第一行输出一个整数代表所求答案。



## 输入输出样例

输入样例#1：

 

复制

```
2
VK
```

输出样例#1：

 

复制

```
1
```

输入样例#2：

 

复制

```
2
VV
```

输出样例#2：

 

复制

```
1
```

输入样例#3：

 

复制

```
1
V
```

输出样例#3：

 

复制

```
0
```

输入样例#4：

 

复制

```
20
VKKKKKKKKKVVVVVVVVVK
```

输出样例#4：

 

复制

```
3
```

输入样例#5：

 

复制

```
4
KVKV
```

输出样例#5：

 

复制

```
1
```

## 说明

对于 100%的数据，1<=n<=100 。

### 思路

皇帝f(n,1)表示：有n个长度的字符，1个魔法（魔法可以改变一个字符），皇帝想知道的答案，来自以下几个大臣的答案，并且取大臣们的答案的最大值

第一个大臣：f(n-1,1)，表示删除最后一个字符，

第2个大臣：f(n-2,0)+1，表示用魔法改变了一个字符

​	这里我们错了很多次，改变字符，分成两种情况，case1：(s[l-1]=='V' && s[l-2]=='V')  case2：(s[l-1]=='K' && s[l-2]=='K'&&s[l-3]!='V')

​	特别是情况2，如果不把s[l-3]!='V'加进去是错误的。

第3个大臣是：f(n-2,1)+1,表示最后是VK的情况。



#### 源代码

~~~c++
#include<bits/stdc++.h>
using namespace std;
string s;
int l;
int mm[105][2];
int shu(int l){
	int ans=0;
	for(int i=0;i<l;i++){
		if(s[i]=='V' && s[i+1]=='K'){
			ans++;
		}
	}
	return ans;
}
int f(int l,int m){
	int king;
	if(l<=1) return 0;
	if(m==0) return shu(l);
	if(mm[l][m]!=-1) return mm[l][m];
	int x=0;
	if((s[l-1]=='V' && s[l-2]=='V') || (s[l-1]=='K' && s[l-2]=='K'&&s[l-3]!='V')) x=1+f(l-2,0);
	int y=f(l-1,1);
	int z=0;
	if(s[l-1]=='K' && s[l-2]=='V'){
		z=1+f(l-2,1);
	}
	king=x;
	if(y>king) king=y;
	if(z>king) king=z;
	mm[l][m]=king;
	return king;
}
int main(){
	cin>>l;
	cin>>s;
	memset(mm,255,sizeof(mm));
	cout<<f(l,1);
	return 0;
}

~~~



###  思路二

首先输入； 
进入循环； 
判断是否为“V K”；（大写）
如果是，ans++，并把它贬称“v k”； （小写）
进入循环； 
判断是否为“V V”或“K K”，因为“V V”可改成“V K”，因为“K K”可改成“V K”，一前一后； 
如果是，ans++ ，并break，因为改变至多一个字符（或者不做任何改变）；
最后输出；

###  源代码2

```c++
#include<bits/stdc++.h>
using namespace std;
int n,ans;
string s;
int main(){
	cin>>n;
	cin>>s;
	for(int i=0;i<n-1;i++)
		if(s[i]=='V'&&s[i+1]=='K'){
			ans++;
			s[i]='v';
			s[i+1]='k';
		}
	for(int i=0;i<n-1;i++)
		if(s[i]==s[i+1]){
			ans++;
			break;
		}
	cout<<ans;
	return 0;
}

```

