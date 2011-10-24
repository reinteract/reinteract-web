---
layout: default
title: Add-ons - Reinteract
page: addons
---

Reinteract extensions are python modules that must provide a subclass of reinteract.custom_result.CustomResult, and implement a create_widget() method. This method must return a pygtk widget, that will be displayed in the Reinteract shell.

Available Add-ons
-----------------

<div class="addon">
  <img src="/addon-images/refigure2.png" />
  <a class="name" href="http://rschroll.github.com/refigure2/">refigure2</a>
  <span class="description" markdown="1">
    Embed a matplotlib figure, using functions from pylab.
  </span>
  <span class="author">By Robert Schroll</span>
</div>

<div class="addon">
  <img src="/addon-images/revis.png" />
  <a class="name" href="http://rschroll.github.com/revis/">revis</a>
  <span class="description" markdown="1">
    Embed a [visvis](http://code.google.com/p/visvis/) figure. Visvis is a pure Python plotting library using OpenGL, allowing for fast 3D visualization.
  </span>
  <span class="author">By Robert Schroll</span>
</div>

<div class="addon">
  <a class="name" href="http://rschroll.github.com/revis/">Sympy+Reinteract</a>
  <span class="description" markdown="1">
    Symbolic Math view in Reinteract.
  </span>
  <span class="author">By Jorn Baayen</span>
</div>


<div class="clear" />

Older Add-ons
-------------

<div class="addon">
  <a class="name" href="http://taschenorakel.de/mathias/2007/11/11/playing-reinteract/">reimage</a>
  <span class="description" markdown="1">
    paint images, GDK pixbuffs, GTK widgets and PIL images
  </span>
  <span class="author">By Mathias Hasselmann</span>
</div>

<div class="addon">
  <a class="name" href="http://code.google.com/p/xprext/">rere</a>
  <span class="description" markdown="1">
    Highlight regular expression matches
  </span>
  <span class="author">By Tero Jäntti</span>
</div>

<div class="addon">
  <a class="name" href="http://base-art.net/Articles/92/">repgm</a>
  <span class="description" markdown="1">
    Integrate a [Pigment](https://code.fluendo.com/pigment/trac) canvas inside Reinteract
  </span>
  <span class="author">By Philippe Normand</span>
</div>
