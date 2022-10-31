---
title: 'CSP2020探险记'
date: 2022-10-28 08:18:58
tags: [游记]
published: true
hideInList: false
feature: 
isTop: false
---
注：原文写于2020.11.15，当时写在洛谷博客上，现在搬run到博客园中……

~~以下内容为反面教材~~

# 前言
2020年11月7日，我参加了CSP2020入门组的复赛。考完之后感觉很不好，洛谷评测230，结果出来是270，~~感觉一等是没有希望了~~，但不管怎么说吧，我还是应当好好分析一下这次考试的得失。

# 试题分析

## PART 1
[优秀的拆分](https://www.luogu.com.cn/problem/P7071)

这是一道大水题，据我们考前分析，历届CSP入门组复赛第一题的难度成递减趋势（看2019年第一题[数字游戏](https://www.luogu.com.cn/problem/P5660)就知道了）。~~本人当时做得也比较顺利，直接AC。~~

这道题就是一道转二进制的题，多的不想说，直接贴代码（加了文件输入输出的原版代码）：
```cpp
#include<cstdio>

int main(){
	
	freopen("power.in","r",stdin);
	freopen("power.out","w",stdout);
	
	int a;
	scanf("%d",&a);
	if(a%2==1){
		printf("-1");
		return 0;
	}
	int now=1;
	while(now<=a)now*=2;
	now/=2;
	while(a>0){
		if(a>=now){
			printf("%d ",now);
			a-=now;
		}
		now/=2;
	}
	
	return 0;
}
```

## PART 2
[直播获奖](https://www.luogu.com.cn/problem/P7072)

~~相当于是半道水题~~。

刚开始以为是排序题，一看数据规模，N到了10的5次方，O(N×N)的算法直接凉凉（某同学就是因为没看数据规模从而只剩40分）。

再一看，每个选手的成绩为不超过600的正整数，于是乎，我想到了桶排。每输入一个成绩，对应的sum＋1，在从后往前扫一遍直到人数满足要求。复杂度为O(KN)，（K是一个不超过600的常数）。轻松AC。

相较来看，比去年[公交换乘](https://www.luogu.com.cn/problem/P5661)至少从代码量上来说要简单得多。

贴代码（同上，原版代码）：
```cpp
#include<cstdio>
#define N 605

int num,k,a,sum[N];

int main(){
	
	freopen("live.in","r",stdin);
	freopen("live.out","w",stdout);
	
	scanf("%d%d",&num,&k);
	for(int i=1;i<=num;i++){
		scanf("%d",&a);
		sum[a]++;
		int now=0,m=i*k/100>0?i*k/100:1;
		for(int j=602;j>=0;j--){
			now+=sum[j];
			if(now>=m){
				printf("%d ",j);
				break;
			}
		}
	}
	
	return 0;
}
```

## PART 3
[表达式](https://www.luogu.com.cn/problem/P7073)

~~考场上的我，年少轻狂，自以为想出了二叉树就能拿满，结果成为这次考试最大的遗憾~~，然后65分。

本蒟蒻不喜欢字符串，非常不喜欢字符串，~~想当年做中缀表达式求值一类的字符串题时痛苦的不得了。~~

于是，这道题也不例外。

写了半天读入字符串的代码，有绕了半天求值。的确，我把那棵二叉树建出来了，但我估计是写法的常数复杂度太高，于是炸掉了。

相比于去年第三题的变型背包[纪念品](https://www.luogu.com.cn/problem/P5662)，今年的第三题难了不少。

贴上代码，虽然有点不好意思，但那也是我辛辛苦苦写出来的，这道题的代码我删了freopen，并加了一些注释，以方便大家理解当时的我的做法：

```cpp
#include<cstdio>
#include<stack>
#define N 100005
using namespace std;

bool a[N];
int m,q,e[N*10],ere[N],top,root;
//在e数组中，0代表!，-1代表&，-2代表|，正整数代表x的下标 

char w;
int read(){
    char in=getchar();
    if(in=='\n')return -1;//读到回车返回-1 
    while(in==' ')in=getchar();//过滤掉多余空格 
    if(in!='x'){//如果是二元运算符返回0 
        w=in;
        return 0;
    }
    else{//否则返回这个xi代表的i 
        int b=0;
        in=getchar();//丢弃'x'字符 
        while(in>='0'&&in<='9'){//当读到数字时重复 
            b*=10;
            b+=(in-'0');
            in=getchar();
        }
        return b;
    }
}

struct tree{
    int data,lc,rc,fa;
}t[N*10];
stack<int>zt;
bool f[N*10];//f[i]代表第i个部分为树根的子树的初值 
void dfs(int wh){
    if(e[wh]>0){
        f[wh]=a[e[wh]];
        return;
        //如果是数字，直接返回它的值 
    }
    if(e[wh]==0){//如果是！运算，返回取反后的左子树的值 
        dfs(t[wh].lc);
        f[wh]=!f[t[wh].lc];
    }
    else if(e[wh]==-1){//如果是&运算 
        dfs(t[wh].lc);
        dfs(t[wh].rc);
        f[wh]=f[t[wh].lc]&&f[t[wh].rc];
    }
    else{//如果是|运算 
        dfs(t[wh].lc);
        dfs(t[wh].rc);
        f[wh]=f[t[wh].lc]||f[t[wh].rc];
    }
    return;
}

//find_ans(wh,last,l_d)  代表现在要求以wh为根节点的树的值的位置
//并且上一棵子树的根节点为last，上一棵子树的值为l_d 
bool find_ans(int wh,int last,bool l_d){
    if(wh==0)return l_d;//如果wh为0，意味着它是根节点的父亲，直接返回l_d 
    if(e[wh]==0){
        return find_ans(t[wh].fa,wh,!l_d);
        //如果是!运算，直接返回取反后的被影响过的子树的值 
    }
    if(e[wh]==-1){//如果是&运算 
        int ch=t[wh].lc,other=t[wh].rc;
        //ch为变化过的子树的根，other是没变过的 
        if(ch!=last)ch=t[wh].rc,other=t[wh].lc;
        //如果发现ch不等于带过来的last，交换 
        return find_ans(t[wh].fa,wh,f[other]&&l_d);
    }
    if(e[wh]==-2){//如果是|运算 
        int ch=t[wh].lc,other=t[wh].rc;
        if(ch!=last)ch=t[wh].rc,other=t[wh].lc;
        //同上 
        return find_ans(t[wh].fa,wh,f[other]||l_d);
    }
}

int main(){

    int r=read();//输入表达式的第一个部分 
    while(r!=-1){//当表达式没到回车时 
        if(r>0){//如果是数字 
            e[++top]=r;
            ere[r]=top;
            //ere[i]代表xi在e数组里的下标 
        }
        if(r==0){//如果是运算符 
            if(w=='!')e[++top]=0;
            else if(w=='&')e[++top]=-1;
            else if(w=='|')e[++top]=-2;
        } 
        r=read();
    }

    int in;
    scanf("%d",&m);
    for(int i=1;i<=m;i++){
        scanf("%d",&in);
        if(in==1)a[i]=true;
        else a[i]=false;
    }//读入每个变量的初值，xi的初值为a[i] 

    //建树 
    for(int i=1;i<=top;i++){//遍历每一个部分 
        t[i].data=e[i];//记录第i个节点的data 
        if(e[i]<=0){//如果是运算符 
            if(e[i]==0){//如果是！运算 
                int now=zt.top();
                zt.pop(); 
                t[now].fa=i;
                t[i].lc=now;
                //则栈顶元素的父亲是该元素 
            }
            else{//如果是&或|运算 
                int s1=zt.top();
                zt.pop();
                int s2=zt.top();
                zt.pop();
                t[s1].fa=t[s2].fa=i;
                t[i].lc=s1;
                t[i].rc=s2;
                //则栈顶的两个元素的父亲都是该元素 
            }
        }
        zt.push(i);//把该元素的下标压栈 
    }

    for(int i=1;i<=top;i++){
        if(t[i].fa<=0){
            root=i;
            break;
            //找到一个父亲为初始值（0）的节点
            //那么它就是根节点，等会进行dfs求初值时要用 
        }
    }
    dfs(root);//进行dfs求初值 

    scanf("%d",&q);
    while(q--){
        int u;//输入每一个问题 
        scanf("%d",&u);
        if(find_ans(t[ere[u]].fa,ere[u],!a[u]))printf("1\n");
        else printf("0\n");
        //输出整个表达式的值(即是以根节点为树根的表达式的值) 
    }

    return 0;
}
```

## PART 4

[方格取数](https://www.luogu.com.cn/problem/P7074)

我以为这道题是贪心的做法（毕竟数据规模摆在那里的，基本上要用O(M×N)的算法才能过），然后贪了半天没贪出来，于是乎只能垂头丧气地写了一个爆搜，结果莫名其妙地爆零了。

啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊

同班的说这道题很简单，我却直接爆零了！！！

然后请教了某些大神，发现读错题了，~~把可以向上下右看成可以上下左右了。~~

~~心情极度郁闷~~，但最后竟然还有5分，也不知道哪里来的。

我觉得吧，去年最后一题[加工零件](https://www.luogu.com.cn/problem/P5663)对我而言要友好得多，那道题暴力随便50，用了图论或者DP的话要AC也不难。只能说本蒟蒻生不逢时，或者是修炼的不够。

代码就不贴出来了吧，~~0分代码就不放出来丢人现眼了。~~

# 总结
从这次考试中还是能说明很多问题的，包括考试技巧、知识掌握等等等等。

总之，在接下来的一年里努力吧。

CSP2021提高组见，希望那时的我能比现在的我做得更好。