---
layout: default
title: How it Works - Tutorial - Reinteract
---

How it Works
------------

The basic idea of reinteract is that reinteract saves a copy of the scope dictionary after every statement. If it needs to recompile starting from a particular statement, it then has the right scope to execute that statement in.

<div class="codegroup">
<div class="comment">{% highlight python %}# scope: { a: 1, l: [1,2,3,4] }{% endhighlight %}</div>
{% highlight python %}b = a + 1{% endhighlight %}
<div class="comment">{% highlight python %}# scope: { a: 1, b: 2, l: [1,2,3,4] }{% endhighlight %}</div>
</div>

This is pretty efficient because variables that stay the same can be shared between successive scopes, so the only memory used is for the scope dictionaries themselves. However, if reinteract detects that a statement modifies a variable, then it makes a shallow copy of the variable using copy.copy()

<div class="codegroup">
<div class="comment">{% highlight python %}# scope: { a: 1, l: [1,2,3,4] }}
# reinteract does scope['l'] = copy.copy(scope['l']){% endhighlight %}</div>
{% highlight python %}l[0] = 5{% endhighlight %}
<div class="comment">{% highlight python %}# scope: { a: 1, l: [5,2,3,4] }{% endhighlight %}</div>
</div>

Some types of modification are easy to detect, like the slice assignment above or assigning to an attribute. However, some modifications are harder to detect. Compare:

<div class="codegroup">
{% highlight python %}l.append(2)    # modifies l{% endhighlight %}
{% highlight python %}l.count(2)     # no modification{% endhighlight %}
</div>

The rule that reinteract uses is that a bare method call like the ones above is probably a modification, and it conservatively makes a copy in that case. You can avoid a copy by using a temporary variable or explicitly printing the result.

<div class="codegroup">
{% highlight python %}print l.count(2)    # no copy{% endhighlight %}
<pre class="output">1</pre>
{% highlight python %}c = l.count(2);     # no copy{% endhighlight %}
</div>
