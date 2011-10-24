---
layout: default
title: Roadmap - Reinteract
---

The first line on the wiki:

 "Reinteract is a system for interactive experimentation with Python"

is pretty close to the mission statement for Reinteract. (I might
rephrase it to be "using Python" not "with Python"). The essential
nature of Reinteract is to be able to move from seamlessly from
fooling around to having a document that records what you did in a
reproducible fashion. Additional attributes:

 - It's easy and obvious for simple stuff.
 - It allows using a familiar real language with an active large ecosystem
 - It scales up to non-trivial problems. Reinteract would be a lot
simpler (and have less weird corners) if it re-executed worksheets
from the scratch. It also wouldn't be particularly useful for a lot of
real problems.

So, what is a Reinteract user experimenting with? Roughly in the order
I consider interesting:

 - Scientific and other data analysis: this is why I wrote Reinteract,
and why incremental re-execution is important.
 - Algorithms: I often use reinteract when I want to develop a bit of
Python code (or non-Python) code that does something hard.
 - The Python language itself and education: Reinteract is in many
ways quite good for this - being able to go back and fix errors is
huge. However, the places where it isn't quite Python have the
potential to be a challenge.
 - Different Python libraries. If you understand the limitations of
Reinteract and its management of side effects it can be useful for
this, but for many libraries (especially networking, UI, etc), don't
fit well into the Reinteract stateless model.

And what are some open projects for Reinteract? Off the top of my head:

 - Better management of side effects. If Reinteract detects that it
can't make a "snapshot" of a state of a variable, then Reinteract
should back up to where it was able to make a snapshot and execute
from there.

 - Annotation of external APIs. Sometimes when Reinteract tries to use
copy.copy to make a snapshot, it gets an exception, and then it could
do something like described above. But at other times the copy
apparently works, but doesn't do what is expected. It should be
possible (by some side of look-aside API description files, perhaps)
to describe important Python APIs, like the core libraries so
Reinteract can do a better job.

 - "Sidebars" (http://www.reinteract.org/trac/wiki/SidebarDesign) - be
able to use an interactive drawing API and see what's going on.

 - Reinteract adapted solutions for a broader range of items - vector
graphics, 3D, etc. Based on leading Python solutions in the area.

 - Interaction with plots and other widgets embedded in the Reinteract
canvas - zoom in, use crosshairs to find coordinates, etc.

 - Test with Python 2.7, start thinking about Python 3 (PyGTK is the
big blocker for Python 3, but I think there has been progress
recently.)

 - Front-end / backend-split. Have the UI in one process, the backend
in another. This would give more reliable ways of interrupting
worksheet interaction than the current thread-based hacks, and allow
working with libraries not compatible with the GLib main loop. A
HTML/JS based frontend is also possible.

 - Data files in worksheets - have views for CSV data, etc. Cache data
sets in memory once loaded, rather than reparsing them on worksheet
execution.

Of course, some of those (many of those) are significantly hard projects.
