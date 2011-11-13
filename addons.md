---
layout: default
title: Add-ons - Reinteract
page: addons
---

Reinteract extensions are python modules that must provide a subclass of reinteract.custom_result.CustomResult, and implement a create_widget() method. This method must return a pygtk widget, that will be displayed in the Reinteract shell.

Available Add-ons
-----------------

<div class="addon">
  <h3><a class="name" href="http://rschroll.github.com/refigure2/">refigure2</a></h3>
  <img src="/addon-images/refigure2.png" />
<div class="codegroup">
{% highlight python %}from refigure2 import *{% endhighlight %}
{% highlight python %}from numpy import *{% endhighlight %}
&nbsp;
{% highlight python %}t = arange(0.0, 2.0, 0.01){% endhighlight %}
{% highlight python %}s = sin(2*pi*t){% endhighlight %}
&nbsp;
{% highlight python %}
build figure():
    plot(t, s, linewidth=1.0)
    xlabel('time (s)')
    ylabel('voltage (mV)')
    title('A simple plot')
    grid(True)
{% endhighlight %}
</div>
  <div class="description">
    Embed a matplotlib figure, using functions from pylab.
  </div>
  <div class="support">Supports Printing and Sidebars</div>
  <div class="author">By Robert Schroll</div>
</div>

<div class="addon">
  <h3><a class="name" href="http://rschroll.github.com/revis/">revis</a></h3>
  <img src="/addon-images/revis.png" />
<div class="codegroup">
{% highlight python %}from revis import *{% endhighlight %}
&nbsp;
{% highlight python %}
build figure():
    a = gca()
    a.bgcolor = 'k'
    a.axis.axisColor = 'w'
    
    mesh = solidTeapot((32,32,80),
               scaling=(50,50,50))
    mesh.faceColor = 0.4, 1, 0.4
    mesh.specular = 'r'
    
    ax.axis.xLabel = 'x-axis'
    ax.axis.yLabel = 'y-axis'
    ax.axis.zLabel = 'z-axis'
{% endhighlight %}
</div>
  <div class="description">
    Embed a <a href="http://code.google.com/p/visvis/">visvis</a> figure. Visvis is a pure Python plotting library using OpenGL, allowing for fast 3D visualization.
  </div>
  <div class="support">Supports Printing and Sidebars</div>
  <div class="author">By Robert Schroll</div>
</div>

<div class="addon">
  <h3><a class="name" href="http://jbaayen.blogspot.com/2009/09/part-of-what-makes-sympy-so-useful-is.html">Sympy+Reinteract</a></h3>
  <div class="description">
    Symbolic Math view in Reinteract.
  </div>
  <div class="author">By Jorn Baayen</div>
</div>


<div class="clear" />

Older Add-ons
-------------

<div class="addon">
  <h3><a class="name" href="http://taschenorakel.de/mathias/2007/11/11/playing-reinteract/">reimage</a></h3>
  <div class="description">
    paint images, GDK pixbuffs, GTK widgets and PIL images
  </div>
  <div class="author">By Mathias Hasselmann</div>
</div>

<div class="addon">
  <h3><a class="name" href="http://code.google.com/p/xprext/">rere</a></h3>
  <div class="description">
    Highlight regular expression matches
  </div>
  <div class="author">By Tero Jäntti</div>
</div>

<div class="addon">
  <h3><a class="name" href="http://base-art.net/Articles/92/">repgm</a></h3>
  <div class="description">
    Integrate a <a href="https://code.fluendo.com/pigment/trac">Pigment</a> canvas inside Reinteract
  </div>
  <div class="author">By Philippe Normand</div>
</div>
