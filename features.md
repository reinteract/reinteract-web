---
layout: default
title: Features - Reinteract
page: features
---

Standard Python
---------------

Rather than using a specialized language, Reinteract uses Python.
Almost all standard Python code works unmodified in a Reinteract
worksheet. (Library support is more limited: not all of the Python
standard library makes sense in the context of a Reinteract worksheet.)

Efficient Operation
-------------------

When you make a change to a Reinteract worksheet, Reinteract is
frequently able to only execute the portion you change rather than the
entire worksheet. This greatly increases the efficicency of working on
a calculation involving large datasets where individual steps in the
calculation can take a long time to executes.

Inline and Sidebar Results
--------------------------

Reinteract isn't limited to just text output; plots and other graphical
displays can be embedded into worksheets as interactive controls. These
controls can either be displayed inline or in a separate sidebar for
convenient viewing along with the code that creates them.

Extensible
----------

The set of result types that are displayed graphically isn't fixed;
new custom result types can be added by deriving from the supplied
CustomResult class.

Full Editor
-----------

The Reinteract editor provides all the features you would expect in a
code editor, including syntax-highlighting, completion, and auto-indentation.

Notebooks
---------

Reinteract worksheets can be grouped into "notebooks" which include multiple
worksheets along with shared data and Python library code.
