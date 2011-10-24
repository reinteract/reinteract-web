---
layout: default
title: Plotting - Tutorial - Reinteract
---

How it Works
------------

The basic idea of reinteract is that reinteract saves a copy of the scope dictionary after every statement. If it needs to recompile starting from a particular statement, it then has the right scope to execute that statement in.

{% highlight python %}
# scope: { a: 1, l: [1,2,3,4] }
b = a + 1
# scope: { a: 1, b: 2, l: [1,2,3,4] }
{% endhighlight %}

This is pretty efficient because variables that stay the same can be shared between successive scopes, so the only memory used is for the scope dictionaries themselves. However, if reinteract detects that a statement modifies a variable, then it makes a shallow copy of the variable using copy.copy()

{% highlight python %}
# scope: { a: 1, l: [1,2,3,4] }
# reinteract does scope['l'] = copy.copy(scope['l'])
l[0] = 5
# scope: { a: 1, l: [5,2,3,4] }
{% endhighlight %}

Some types of modification are easy to detect, like the slice assignment above or assigning to an attribute. However, some modifications are harder to detect. Compare:

{% highlight python %}
l.append(2)    # modifies l
l.count(2)     # no modification
{% endhighlight %}

The rule that reinteract uses is that a bare method call like the ones above is probably a modification, and it conservatively makes a copy in that case. You can avoid a copy by using a temporary variable or explicitly printing the result.

{% highlight python %}
print l.count(2)    # no copy

c = l.count(2);     # no copy
{% endhighlight %}
