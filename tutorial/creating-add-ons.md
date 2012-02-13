---
layout: default
title: Creating Add-ons - Tutorial - Reinteract
---

Reinteract extensions are simply subclasses of reinteract.custom_result.CustomResult that implement one or more special methods.

Basics
------
As an example, let's look at the replot extension.

<div class="codegroup">
{% highlight python %}from replot import plot{% endhighlight %}
{% highlight python %}result = plot([1,2,1]){% endhighlight %}
</div>

The object created is a instance that inherits from CustomResult.  This is how Reinteract knows to treat it specially.

<div class="codegroup">
{% highlight python %}from reinteract.custom_result import CustomResult{% endhighlight %}
{% highlight python %}isinstance(result, CustomResult){% endhighlight %}
<pre class="output">True</pre>
</div>

The object must have a ``create_widget()`` method, which should return a gtk.Widget.  This widget is embedded in the Reinteract notebook in place of the CustomResult.

<div  class="codegroup">
{% highlight python %}import gtk{% endhighlight %}
{% highlight python %}widget = result.create_widget(){% endhighlight %}
{% highlight python %}isinstance(widget, gtk.Widget){% endhighlight %}
<pre class="output">True</pre>
</div>

Here, we create a simple CustomResult that embeds a gtk.Button:

<div class="codegroup">
{% highlight python %}
class Button(CustomResult):
    def __init__(self, label):
        self._label = label
    
    def create_widget(self):
        return gtk.Button(self._label)
{% endhighlight %}
&nbsp;
{% highlight python %}Button('Hello World!'){% endhighlight %}
<pre class="output"><form onclick="return false"><input type="submit" value="Hello World!" /></form></pre>
</div>

Reinteract will call the ``show()`` method on the returned widget.  But you are responsible for showing any child widgets.  Also note that the cursor will remain the text cursor unless you change it.

<div class="codegroup">
{% highlight python %}
class Button(CustomResult):
    def __init__(self, label):
        self._label = label
        
    def create_widget(self):
        button = gtk.Button(self._label)
        eb = gtk.EventBox()
        eb.add(button)
        eb.connect("realize", lambda widget:
            widget.window.set_cursor(gtk.gdk.Cursor(gtk.gdk.LEFT_PTR)))
        eb.show_all()
        return eb
{% endhighlight %}
</div>

Dealing with Mutations
----------------------
For simple CustomResults that are created and displayed in the same line, Reinteract's state management works fine.  If your CustomResult is designed to be modified, however, you must consider whether these modifications will be correctly tracked.  In general, if the changes are contained in the object itself, it will behave correctly.  But if the changes result in modifications to state held outside of the object, there will be problems.

As a concrete example, consider CairoResult, below.  It contains a cairo Context which can be drawn upon.

{% highlight python %}
from reinteract.custom_result import CustomResult
import cairo
import gtk

class CairoResult(CustomResult):
    
    def __init__(self, width=200, height=200):
        self.width, self.height = width, height
        self.surface = cairo.ImageSurface(cairo.FORMAT_ARGB32, width, height)
        self.context = cairo.Context(self.surface)
    
    def create_widget(self):
        self.widget = gtk.DrawingArea()
        self.widget.set_size_request(self.width, self.height)
        self.widget.connect('expose-event', self.on_expose_event)
        return self.widget
        
    def on_expose_event(self, event, widget):
        cr = self.widget.window.cairo_create()
        cr.set_source_surface(self.surface, 0, 0)
        cr.paint()
{% endhighlight %}
<div class="codegroup">
{% highlight python %}from cairoresult import CairoResult{% endhighlight %}
{% highlight python %}cr = CairoResult(){% endhighlight %}
{% highlight python %}cr.context.set_source_rgb(1,0.5,0){% endhighlight %}
<pre class="output warning">'cr.context' apparently modified, but can't copy it</pre>
{% highlight python %}cr.context.rectangle(0, 50, 200, 20){% endhighlight %}
<pre class="output warning">'cr.context' apparently modified, but can't copy it</pre>
{% highlight python %}cr.context.fill(){% endhighlight %}
<pre class="output warning">'cr.context' apparently modified, but can't copy it</pre>
{% highlight python %}cr{% endhighlight %}
</div>

This will create a square with an orange rectangle across it.  The warnings are an indication that Reinteract cannot properly copy the state in order to rewind it later.  Indeed, if you edit the rectangle position and re-execute, you will find two rectangle in the square instead of just one.

There are two ways to deal with this.  The first is to ensure that only the object itself is modified, so that Reinteract can make the appropriate copies.  In this case, it would mean creating methods that track and store the drawing actions, and then playing these actions back during the ``create_widget()`` method.  This is what replot does.

A less sophisticated method is to require that the creation, modification, and display of the CustomResult all happen at the same time.  This means they must all occur in the same block.  While you could just wrap the commands above in an ``if True:`` block, Reinteract provides a better solution: the build block.

<div class="codegroup">
{% highlight python %}
build CairoResult() as cr:
    cr.context.set_source_rgb(1,0.5,0)
    cr.context.rectangle(0, 50, 200, 20)
    cr.context.fill()
{% endhighlight %}
</div>

The build block acts much like a with block, with a few enhancements.
1. If the context manager does not have an ``__enter__()`` method, it is assigned to the target following 'as'.  (If it does, the result of that method call is assigned.)
2. The context manager need not have an ``__exit__()`` method.
3. The target is displayed after in the notebook after the ``__exit__()`` method is called.  This happens even if there is no explicit target.

Because the CustomResult is created, modified, and displayed every time the block is executed, there is no need to track its modifications.

Sidebars
--------
In Reinteract 0.6, notebooks gained sidebars.  These areas can hold the widgets created by CustomResults to the right of the code.  This makes it easier to see both the code and the widget on wide screens.

To tell Reinteract to put a CustomResult's widget in the sidebar, give the CustomResult a ``display`` attribute with the value ``"side"``.  If you would like your users to be able to choose where the widget should be displayed, the convention is to include a ``display`` keyword in the initializer of the CustomResult.  A value of ``"side"`` indicates the widget should appear in the sidebar; ``"inline"`` indicates it should appear in the flow of the worksheet.

Printing
--------
Reinteract 0.6 also gained the ability to print notebooks.  In order to have something other than ``unicode(result)`` in the print-out, CustomResults must implement a ``print_result()`` method.  This method is passed two arguments: a gtk.PrintContext and a Boolean, ``render``.  It returns the height of the printed object.  If ``render`` is False, we're measuring for pagination, and ``print_result()`` should just return the height without rendering anything.  Otherwise, render to the Cairo context returned by the ``get_cairo_context()`` method of the gtk.PrintContext.  This Cairo context has already been translated appropriately for printing from y=0.

{% highlight python %}
class CairoResult(CustomResult):
     ⋮
    def print_result(self, context, render):
        if render:
            cr = context.get_cairo_context()
            cr.set_source_surface(self.surface, 0, 0)
            cr.paint()
        return self.height
{% endhighlight %}
