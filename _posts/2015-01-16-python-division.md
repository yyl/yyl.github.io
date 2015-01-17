---
layout: post
title:  "Division in Python"
date:   2015-01-16 16:30:00
categories: python
---

Turns out there are more than one way to divide numbers in Python.

{% highlight python %}
In [1]: 2/1
Out[1]: 2
In [2]: 1/2
Out[2]: 0
In [3]: 1/1.5
Out[3]: 0.6666666666666666
In [4]: 1.0/2
Out[4]: 0.5
{% endhighlight %}

As you see, by default, python treats division as integer with integer unless you explicitly specify the number to be float with the dot. Also, the integer division always obtain the floor of the result. Note for negative numbers, the floor means _further_ from zero.

{% highlight python %}
In [1]: -1/2
Out[1]: -1
{% endhighlight %}

Python function `div` of module `operator` works the same way as `/`.

{% highlight python %}
In [8]: import operator
In [9]: operator.div(1,2)
Out[9]: 0
In [10]: operator.div(1,2.0)
Out[10]: 0.5
In [12]: operator.div(-1,2)
Out[12]: -1
{% endhighlight %}

In addition, python supports one more type of division operator `//`. To explain how it works, we need to look at Python 3 first.

In [Python 3](https://www.python.org/dev/peps/pep-0238/), `/` has been changed. It is called "true division", and will return approximate result regardless of the input type. Alternatively, `//` is called "floor division". It will return the floored result, which is the same as the original `/`.

To keep consistency, `//` has been added Python 2 as well. However, `/` has not been changed. Therefore, in Python 2, `//` behaves just like in Python 3: return floored result regardless of the input type.

{% highlight python %}
In [7]: 1//2
Out[7]: 0
In [8]: 2//2
Out[8]: 1
In [9]: 3//3.5
Out[9]: 0.0
{% endhighlight %}

We could obtain same functionality of Python 3 for `/` and `//` in Python 2 by importing from future.

{% highlight python %}
In [11]: from __future__ import division
In [12]: 1/2	# true division
Out[12]: 0.5
In [13]: 2/2.5
Out[13]: 0.8
In [14]: 2/2
Out[14]: 1.0
In [15]: 1//2	# floor division
Out[15]: 0
In [16]: 2//2.5
Out[16]: 0.0
{% endhighlight %}

In other languages, division has been handled differently. Java seems to have the similar way of handling division, that is divisions are treated as integer division by default, unless you explicitly specify that input is `float` or `double`.

{% highlight java %}
java> 1/1
java.lang.Integer res0 = 1
java> 1/2
java.lang.Integer res1 = 0
java> 1.0/2
java.lang.Double res2 = 0.5
java> -1/2
java.lang.Integer res3 = 0
java> -1.0/2
java.lang.Double res4 = -0.5
java> 1/2f
java.lang.Float res0 = 0.5
java> 1/ (double) 2
java.lang.Double res1 = 0.5
java> (double) 1/2
java.lang.Double res2 = 0.5
{% endhighlight %}

One difference is, the negative integers in Java are rounded _towards_ zero, which is the opposite of how Python does. This is what called "real division", in which the system throws away the part that does not belong to integer - decimal part - of the result.