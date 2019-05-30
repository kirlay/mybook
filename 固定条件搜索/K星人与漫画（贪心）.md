##  K星人与漫画

#### 题目描述

在距离地球50万光年的R星球，生活着文明高度发达的K星人，K星人和地球人不一样，他们不使用纸币交易货物，而是使用一种特殊金属制成的硬币，这种金属非常轻，且可任意折叠，便于携带（好像和我们的纸币没什么两样）。
他们的硬币面值有1元、5元、10元、50元、100元和500元共6种，一天K星人TT外出购买他喜欢看的漫画书《小龙人与喜羊羊》，他事先了解到这本书要x元，看着家里攒下的压岁钱，想着出门要尽量少带几枚硬币，这样可以更快的飞奔到商店买到心怡的漫画。
现在的问题就是用已有的硬币来支付x元，最少需要多少枚硬币？如果没有合法的支付方案，则输出-1。

#### 输入

一行，7个整数，分别表示1元，5元，10元，50元，100元，500元硬币的枚数和x。

#### 输出

一个整数，表示最少需要携带硬币的枚数，没有可行方案输出-1。

#### 样例输入

```
3 2 1 3 0 2 620
```

#### 样例输出

```
6
```

#### 提示

0<=所有数据<=10的9次方



## 思路

​    //刘宇鑫

先输入，并求出总钱数 ，如果总钱数＜需要的钱数，就输出-1 ，如果总钱数=需要的钱数，就输出总钱数，然后进行搜索，面值从大到小依次搜索，求出该面值可以使用的最大数量，并将x减去最大金额，期间如果x==0，输出最少数量，如果没有方案，输出-1

//钱嘉欢

先输入六种纸币分别有几张，同时在h里存纸币总价值，在输入需要付的价格。如果纸币总和小于需要付的价格，就输出-1。如果纸币总和等于需要付的价格，就输出总的纸币数量。循环是从大到小循环，如果这种价格的纸币总价格大于需要付的价格，就在a[i]里面减去需要付的价格除以价格面值，需要付的价格减去只用这种纸币的最大价格，答案加上需要付的价格除以价格的面值。如果这种价格的纸币总价格小于现在需要付的价格，需要付的价格就减去这种纸币的总价格，答案加上这种纸币的总纸币数。如果需要付的价格等于0的时候就跳出循环。最后如果需要付的价格不是0，就输出-1，不然就输出答案。

------------------------------------------------



我的第一感觉是错的，当条件固定的时候，用for循环去枚举，势必会超时

然后想着用背包，也是不可取的

最后还是用到了贪心策略。

同学们，一开始的特例判断的写法，写的真好！

## 代码

~~~c++
//刘宇鑫 钱嘉欢 代码很优雅很容易读懂
#include<iostream>
using namespace std;
unsigned long long a[10],x,ans,h,b[10]={0,1,5,10,50,100,500};
int main(){
	for(int i=1;i<=6;i++)
	{
		cin>>a[i];//输入 
		h=h+a[i]*b[i];//求出总钱数 
	}
	cin>>x;
	if(h<x){//如果总钱数＜需要的钱数，就输出-1 
		cout<<"-1";
		return 0;
	}
	if(h==x)//如果总钱数=需要的钱数，就输出总钱数
	{
		cout<<a[1]+a[2]+a[3]+a[4]+a[5]+a[6];
	}
	ans=0;
	for(int i=6;i>=1;i--)//查找最优方案 
	{
		if(x/b[i]<=a[i])
		{
			ans+=x/b[i];
			x=x%b[i];
		}
		if(x/b[i]>a[i])
		{
			ans+=a[i];
			x=x-a[i]*b[i];
		}
		if(x==0)
		{
			cout<<ans;
			return 0;
		}
	}
	cout<<-1;
	return 0;
}
~~~



~~~c++
//李明烨
#include<cstdio>
#include<iostream>
using namespace std;
long long yuan/*硬币个数*/,money/*书的钱*/,o/*one(1)*/,f/*five(5)*/,t/*ten(10)*/,fif/*fifty(50)*/,hun/*one hundred(100)*/,fhun/*five hundred(500)*/;
void check(long long many/*有多少张*/,long long worth/*值多少钱*/){ 
	if(money==0||many==0||money<worth){
		return ;
	}//如果钱刚好了就不用再花硬币了；如果没有这种硬币就去下一个；如果币值太大就去下一个 
	if(money/worth<=many){//如果需要这种硬币的数量比有的少 
		yuan+=(money/worth);//那么就把需要的加上 
		money%=worth;//减掉这种硬币所花的钱 
	} 
	else{//如果没有那么多硬币 
		yuan+=many;//就把全部纸币加加上，用光 
		money-=many*worth;//把这种硬币所花的钱减掉 
	}
}
int main(){
	scanf("%lld%lld%lld%lld%lld%lld%lld",&o,&f,&t,&fif,&hun,&fhun,&money);//输入 
	if(o+f*5+t*10+fif*50+hun*100+fhun*500<money){
		printf("-1\n");
		return 0; 
	}//特判——如果书太贵，就输出-1 
	if(money==0){
		printf("0\n");
		return 0;
	}//特判——如果书不需要钱，就输出0 
	check(fhun,500);//要最少，就优先选择大的 
	check(hun,100);
	check(fif,50);
	check(t,10);
	check(f,5);
	check(o,1);//为避免代码量，编成了一个函数
	if(money>0){
		printf("-1\n");
		return 0;
	}//如果没有把书“买彻底”，就输出-1 
	printf("%lld\n",yuan);//输出 
	return 0;
}

~~~



~~~c++
//金子杨
#include<iostream>

using namespace std;
struct kfc{
	int mz;
	int ms;
}a[10];
int jg,p;
int main()
{
	a[1].mz=1; a[2].mz=5; a[3].mz=10; a[4].mz=50; a[5].mz=100; a[6].mz=500;
	for(int i=1;i<=6;i++)
		cin>>a[i].ms;
	cin>>jg;
	
	for(int i=6;i>=1;i--)
	{
		if(jg%a[i].mz==0 && a[i].ms>=(jg/a[i].mz))
		{
			p+=(jg/a[i].mz);
			cout<<p<<endl;
			return 0;
		}
		else
		{
			if(a[i].ms>=(jg/a[i].mz))
			{
				p+=(jg/a[i].mz);
				jg-=(a[i].mz*(jg/a[i].mz));
			}
			else
			{
				p+=a[i].ms;
				jg-=a[i].ms*a[i].mz;
			}
		}
	}
	if(jg>0)
	{
		cout<<-1<<endl;
		return 0;
	}
	cout<<p<<endl;
	return 0;
} 
~~~

