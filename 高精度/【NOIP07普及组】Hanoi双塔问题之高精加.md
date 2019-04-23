### 1163: 【NOIP07普及组】Hanoi双塔问题

**题目描述**

给定A,B,C三根足够长的细柱，在A柱上穿着2n个圆盘（上小下大），共有n个不同的尺寸，每个尺寸都有两个相同的圆盘，注意这两个圆盘是不加区分的(下图为n=3的情形)。现要将这些圆盘移到C柱上，在移动过程中可放在B柱上暂存。要求:

![img](http://oj.jzxx.net/upload/1163.jpg)

 (1)每次只能移动一个圆盘； (2) A、B、C三根细柱上的圆盘都要保持上小下大的顺序； 任务:设An为2n个圆盘完成上述任务所需的最少移动次数，对于输入的n，输出An。

**输入**
一个正整数n，表示在A柱上放有2n个圆盘

**输出**
仅一行，包含一个正整数，为完成上述任务所需的最少移动次数An

**样例输入**
1
**样例输出 **
2
提示[-]
对于50%的数据， 1<=n<=25 对于100% 数据， 1<=n<=200

#### 思路

![1555303796932](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\1555303796932.png)

#### 源码

```c++
#include<iostream> 
#include<cstring> 
using namespace std; 
struct big_int { 
	int len,a[1005]; 
	big_int operator+ (big_int x){ 
		big_int ans; 
		memset(ans.a,0,sizeof(ans.a)); 
		ans.len=len; 
		if(ans.len<x.len) 
			ans.len=x.len; 
		for(int i=0;i<ans.len;i++){ 
			ans.a[i]+=a[i]+x.a[i]; 
			ans.a[i+1]+=ans.a[i]/10; 
			ans.a[i]%=10; 
		} 
		if(ans.a[ans.len]) 
			ans.len++; 
			return ans; 
	} 
	void print(){ 
		for(int i=len-1;i>=0;i--) 
			cout<<a[i]; 
	} 
}; 
big_int f[205]; 
int n; 
int main(){ 
	cin>>n; 
	f[1].len=1; 
	f[1].a[0]=2; 
	for(int i=2;i<=n;i++) 
		f[i]=f[i-1]+f[i-1]+f[1]; 
	f[n].print(); 
	return 0; 
}
```

