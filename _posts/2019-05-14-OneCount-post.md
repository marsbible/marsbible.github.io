---
layout: post
title: 计算从1到N的数字中1的个数 
modified: 2019-05-14
tags: [面试]
categories: [技术]
---

### 简单算法 
{% highlight cpp %}
//naive算法，计算每个数的1，相加
int singleOneNumber(int n) {
    int ret = 0;
    while(n != 0) {
        if(n % 10 == 1) ret++;
        n = n / 10;
    }
    return ret;
}

int oneNumber(int n) {
    int r = 0;
    for(int i=0;i<=n;i++) {
        r += singleOneNumber(i);
    }

    return r;
}

{% endhighlight %}

### 优化算法 
{% highlight cpp %}
/*
  递归的分析n，从高位到低位进行递归。
  以32345为例，可以分解为如下的子问题：
  (3)0000-(3)2345之间的1 + (2)0000-(2)9999之间的1 + (1)0000-(1)9999之间的1 + 0-9999之间的1 (不考虑最高位的1)
  + 1(0000)-1(9999)之间的1 (只考虑最高位)，递归进行计算即可，也可以用迭代的方式进一步优化效率。


  记H(N)为某个数的最高位的数字，比如H(12345)1,H(2345)=2
  记L(N)为某个数的长度，比如L(12345)=5，L(2345) = 4

  F(N) = F(N-H(N)*10^L(N)) +  H(N)*F(10^L(N)-1) + min(N-10^L(N)+1,10^L(N))

*/
int H(int n) {
    int h = 0;
    while(n != 0) {
        h = n % 10;
        n = n/10;
    }

    return h;
}

int L(int n) {
    int l = 0;
    while(n != 0) {
        n = n/10;
        l++;
    }
    return l;
}

int exp10(int exp) {
    int r = 1;
    while(exp > 0) {
        r = r*10;
        exp--;
    }
    return r/10;
}

int oneNumber(int n) {
    if(n == 0) return 0;
    if(n <= 9) {
        return 1;
    }
    int base = H(n)*exp10(L(n));
    int next = n - base;

    int r = oneNumber(next);
    for(int i=0;i<H(n);i++) {
        r += oneNumber(exp10(L(n))-1);
    }

    return r + (H(n)==1?(next+1):exp10(L(n)));
}
{% endhighlight %}

