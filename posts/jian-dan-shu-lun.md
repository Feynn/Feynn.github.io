---
title: '简单树论'
date: 2022-10-31 16:42:10
tags: [笔记]
published: true
hideInList: false
feature: 
isTop: false
---
# Prufer
有标号无根树和 prufer 序列形成双射的关系。所以有一些性质。

其构造方式是拿一棵树出来，它有许多叶子。找出这些叶子中编号最小id，记录下它的父亲，丢掉。重复这一过程，会得到一个长度为 $m-2$ 的序列，即 prufer 序列。用堆可以做到单 log，当然也有线性做法（如果它丧心病狂想卡的话），懒得记。

但是这玩意更多是用在无根树的计数方面。它具有很好的性质：每个元素都是可以取到 $[1,m]$ 中的任意数，独立的，即无根树的数量是 $m^{m-2}$。还有一个性质是一个点在树中的度等于序列中出现次数加一。主要是这两个性质，有几个比较基础的应用：

[P4430](https://www.luogu.com.cn/problem/P4430) 无脑板子。树的形态有 $m^{m-2}$ 种，边的顺序有 $(m-1)!$ 种，乘起来即可。

[P2290](https://www.luogu.com.cn/problem/P2290) 板子。根据性质可以确定每个点在 prufer 序列中的出现次数，简单组合即可。然后有一些需要注意的边角情况但不重要。

[P4981](https://www.luogu.com.cn/problem/P4981) 板子，直接输出 $m^{m-2}$ 即可。

[CF156D](https://www.luogu.com.cn/problem/CF156D) 一道比较综合的题目。 [link](https://www.luogu.com.cn/blog/chuan-liu-bu-xi/cf156d)