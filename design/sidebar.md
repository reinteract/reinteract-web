---
layout: default
title: Sidebar - Design - Reinteract
---

Design notes for Sidebar Feature
--------------------------------

_Owen Taylor 2008-10-18_

The basic linear flow of Reinteract worksheets works well for calculations\: you do some calculations, show them, then you do other calculations based on those calculations, and so forth. But there are other tasks where it's a trickier fit - often these tasks involve incremementally construction of an object by multiple calls with side effects. Consider trying to use Reinteract to experiment with the [cairo drawing library](http://www.cairographics.org). In the standard worksheet flow, it would look something like\:

{% highlight python %}
surface = cairo.ImageSurface(cairo.FORMAT_RGB24, 200, 200)
cr = cairo.Context(surface)
cr.set_source_rgba(1,1,1) # white
cr.paint()
cr.set_source_rgba(1,1,1) # black
cr.move_to(0, 0)
cr.line_to(0, 0)
cr.stroke()
surface
# Surface displays inline
{% endhighlight %}

(This example is hypothetical - as of Oct 20008, there's no mechanism managing the side effects of the painting calls on the Context that affect the ImageSurface.) The main problem with this is simply one of positioning ... the surface is below all the code modifying it and is likely to be off the bottom of the visible area. It's also pretty unnatural that the way to incrementally work is to start off with:

{% highlight python %}
surface = cairo.ImageSurface(cairo.FORMAT_RGB24, 200, 200)
cr = cairo.Context(surface)
# ... add stuff here ....
surface
# Surface displays inline
{% endhighlight %}

and add code in the middle. The basic idea of a sidebar is that you type:

{% highlight python %}
cr = recairo.Context(200, 200)
{% endhighlight %}

and a sidebar pane opens to the right of the worksheet and an empty canvas is displayed there. Then subsequent drawing calls made on the Context object ''update'' the canvas so it always shows the results at the end of the drawing commands in the worksheet.

Elaborations
------------

 * The same idea should work well for other domains: obviously drawing complex figures with matplotlib, but also possibly things like creating HTML DOM or GTK+ widget trees.

 * You could have a slider underneath the canvas that would allow you to "time travel" back to previous versions of the canvas. The worksheet would scroll to the relevant line and highlight it.

 * Multiple sidebar figures would appear in the same sidebar with a vertical scrollbar if necessary. Positioning the cursor in the worksheet should automatically scroll the sidebar window to the corresponding figure for the current section of code.

 * You could display the figure other places than a sidebar. A separate window might make sense in some cases, or inline in the worksheet as "floats". (Floats are hard or impossible to do with the current !GtkTextView implementation, but you could imagine alternate ways of implementing worksheets in the future.)

 * There is some conceptual link between this and the idea of a [Monad](http://en.wikipedia.org/wiki/Monad_%28functional_programming%29) in Haskell and other functional languages. It's a restricted context where you break out of the overall flow and do a series of things with side effects. You could almost imagine extending the sidebar concept to other things with side effects, like writing to a file. (The sidebar shows the file contents, perhaps.)
