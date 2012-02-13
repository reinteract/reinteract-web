---
layout: default
title: Plotting - Tutorial - Reinteract
---

One of the interesting things you can do with Reinteract is to create graphical representations of results and include those results directly. In the examples in this section, we'll be using functionality from [Numpy](http://numpy.scipy.org) along with the replot module that is part of Reinteract.

<div class="codegroup">
{% highlight python %}from numpy import *{% endhighlight %}
{% highlight python %}from replot import plot{% endhighlight %}
</div>

Numpy is a large and powerful system, but most basically what it provides is the ability to operate on an array of data as if it was a single value. Some examples:

<div class="codegroup">
{% highlight python %}a = array([1,2,3]){% endhighlight %}
{% highlight python %}a ** 2{% endhighlight %}
<pre class="output">array([1, 4, 9])</pre>
&nbsp;
{% highlight python %}a = array([3,4,5]){% endhighlight %}
{% highlight python %}a * b{% endhighlight %}
<pre class="output">array([3, 8, 15])</pre>
</div>

We'll use the Numpy function `linspace()` to create an array of data for our X values:

<div class="codegroup">
{% highlight python %}x = linspace(0, 2*pi, 100){% endhighlight %}
</div>

This creates an array of 100 equally spaced numbers between 0 and 2\*pi. We call `replot.plot` passing in two arrays: one to act as the X values and one to act as our Y values. The Y values that we'll plot are an array of 100 values derived by calling `sin` on each of the X values.

<div class="codegroup">
{% highlight python %}plot(x, cos(x)){% endhighlight %}
<pre class="output"><img src="plot1.png" /></pre>
</div>

You can also plot multiple data sets in a single command by passing in several pairs of X and Y arrays:

<div class="codegroup">
{% highlight python %}plot(x, cos(x), x, sin(x)){% endhighlight %}
<pre class="output"><img src="plot2.png" /></pre>
</div>

`replot` uses the [matplotlib](http://matplotlib.sourceforge.net/) library to do its plotting. See the documentation for [Axes.plot()](http://matplotlib.sourceforge.net/matplotlib.axes.html#Axes-plot) for complete information about the arguments you can pass to `replot.plot`. As an example of a more complicated usage, you can pass style options for each plot:

<div class="codegroup">
{% highlight python %}plot(x, cos(x), 'r--', # red dashed lines
     x, sin(x), 'bo')  # blue circles{% endhighlight %}
<pre class="output"><img src="plot3.png" /></pre>
</div>
