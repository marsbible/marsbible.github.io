---
layout: post
title: 了解bytes,str和unicode的区别
modified: 2015-11-17
tags: [python]
---

Python 3里使用**bytes**和**str**来表示字符序列，其中**bytes**包含8-bit字节序列，**str**包含Unicode字符序列。

Python 2里使用**str**和**unicode**来表示字符序列，和Python 3相反，**str**包含8-bit字节序列，**unicode**包含Unicode字符序列。

有多种方式可以将unicode字符表示成binary数据(8-bit字节序列)，最常见的是UTF-8编码。需要注意的是，Python 3中的**str**和Python 2
中的unicode并没有绑定特定的编码方式。为了将Unicode字符转化成binary数据，需要使用**encode**方法，将binary数据转化成Unicode字符，
需要使用**decode**方法。




### Unordered Lists

* Item one
* Item two
* Item three

## Code Snippets

{% highlight css %}
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
{% endhighlight %}
