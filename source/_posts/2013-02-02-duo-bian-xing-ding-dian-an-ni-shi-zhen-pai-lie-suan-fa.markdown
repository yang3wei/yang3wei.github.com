---
layout: post
title: "多边形顶点按逆时针排列算法"
date: 2013-02-02 15:12
comments: true
categories: 算法相关
tags: [algorithm, C/C++]
---
原文链接：  
[http://www.myexception.cn/program/776883.html](http://www.myexception.cn/program/776883.html)  
<!-- more -->
<pre><code>const double eps = 1e-8;
int sign(double d){
	return d < -eps ? -1 : (d > eps);
}

// 多边形类
struct poly{
	static const int N = 1005; // 点数的最大值
	point ps[N + 5]; // 逆时针存储多边形的点，[0, pn - 1] 存储点
	int pn;  // 点数

	poly() {
        pn = 0;
    }

	// 加进一个点 
	void push(point tp){
		ps[pn ++] = tp;
	}

	// 第k个位置
	int trim(int k){
		return (k + pn) % pn;
	}

	void clear() {
        pn = 0;
    }
};

// 多边形 org 的有向面积
double signArea(poly org){
	int i, g;
	double ans;
	point* ps = org.ps;
	for (ans = i = 0; i < org.pn; i ++) {
		g = org.trim(i + 1);
		ans += (ps[g].y * ps[i].x - ps[g].x * ps[i].y);
	}
	return ans / 2.0;
}

// 如果 org 的点是逆时针的，则调整为逆时针的
void makeAntclockwise(poly& org) {
	if (sign(signArea(org)) < 0) {
		reverse(org.ps, org.ps + org.pn);
	}
}
</code></pre>
