---
layout: post
title: 【高效Python】了解bytes,str和unicode的区别
modified: 2015-11-17
tags: [Python]
---

Python 3里使用**bytes**和**str**来表示字符序列，其中**bytes**包含8-bit字节序列，**str**包含Unicode字符序列。

Python 2里使用**str**和**unicode**来表示字符序列，和Python 3相反，**str**包含8-bit字节序列，**unicode**包含Unicode字符序列。

有多种方式可以将unicode字符表示成binary数据(8-bit字节序列)，最常见的是UTF-8编码。需要注意的是，Python 3中的**str**和Python 2
中的**unicode**并没有绑定特定的编码方式。为了将Unicode字符转化成binary数据，需要使用**encode**方法，将binary数据转化成Unicode字符，
需要使用**decode**方法。

在编写Python程序时，为了避免混乱，最好将编码和解码操作放在程序的最外围。核心程序只使用Unicode字符类型(Python 3中的**str**和Python 2中的**unicode**)，不要对字符编码做任何假设。
这种方式可以让程序方便的支持多种编码输入(比如Latin-1，Shift JIS和Big5)，同时统一按照指定的编码输出(比如UTF-8)。

字符序列的两种表达方式导致两种情况：

* 针对编码后的8-bit字节序列的操作
* 针对Unicode字符序列的操作

实际编程中经常需要在这两种序列之间转换，下面提供两个函数用来做这种转换。
Python 3的版本如下，提供**str**和**bytes**之间的转换：
{% highlight python %}
def to_str(bytes_or_str):
    if isinstance(bytes_or_str, bytes):
        value = bytes_or_str.decode('utf-8')
    else:
        value = bytes_or_str
    return value # Instance of str
	
def to_bytes(bytes_or_str):
    if isinstance(bytes_or_str, str):
        value = bytes_or_str.encode('utf-8')
    else:
        value = bytes_or_str
    return value # Instance of bytes
{% endhighlight %}

Python 2的版本如下，提供**str**和**unicode**之间的转换：
{% highlight python %}
def to_unicode(unicode_or_str):
    if isinstance(unicode_or_str, str):
        value = unicode_or_str.decode('utf-8')
    else:
        value = unicode_or_str
    return value # Instance of unicode

def to_str(unicode_or_str):
    if isinstance(unicode_or_str, unicode):
        value = unicode_or_str.encode('utf-8')
    else:
        value = unicode_or_str
    return value # Instance of str
{% endhighlight %}

在处理这两种字符序列的时候，有两个令人迷惑的地方值得注意。

第一个问题是，在Python 2中，当字符序列只包含7-bit的ASCII字符时，**unicode**和**str**类型是可以互操作的。

* 你可以用+操作符连接这样的**str**和**unicode**字符序列
* 你可以用等于不等于操作符比较这样的**str**和**unicode**字符序列
* 你可以使用**unicode**字符序列来格式化字符串

这些意味着在Python 2中，作为参数**str**和**unicode**是可以互相替代的，只要他们都是7-bit的ASCII字符。在Python 3中，
**bytes**和**str**这两种类型是不能互操作的，即使是空字符串也不行，所以编码时一定要清楚操作的类型是哪个。

第二个问题是，在Python 3中，文件的输入输出默认会使用utf-8编码。而在Python 2中，文件的输入输出默认当作二进制数据处理。
比如，你想输出一些随机数据到文件中，下面的代码在Python 2中是OK的，但在Python 3中不工作。
{% highlight python %}
with open('/tmp/random.bin','w') as f:
    f.write(os.urandom(10))
>>>
TypeError: must be str, not bytes
{% endhighlight %}
导致Python 3出错的原因是当使用文本模式打开文件的时候，Python 3的文件操作函数内部默认会进行utf-8编解码处理，这些接口的参数
应当是包含Unicode字符的**str**类型，而不是已经编码过的**bytes**类型。

解决这个问题的办法就是使用'rb'和'wb'打开文件，这样不论Python 2还是Python 3，程序拿到的都是原始的数据，文件操作函数不会进行编解码处理。

### 简明总结
* 在Python 3中，**bytes**包含8-bit字节序列，**str**包含Unicode字符序列。这两种类型不能互操作。
* 在Python 2种，**str**包含8-bit字节序列，**unicode**包含Unicode字符序列，当只包含7-bit的ASCII字符时，这两种类型可以互操作。
* 使用辅助函数来确保当前操作的类型和期望的类型一致(8-bit字节序列，Unicode字符序列等)。
* 如果想读写二进制数据，一定要使用'rb'或'wb'的方式打开文件