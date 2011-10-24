---
layout: default
title: Completion - Design - Reinteract
---

Design notes for Completion feature
-----------------------------------

_Owen Taylor 2007-11-17_

(This actually covers both completion and docstring popups since I see them as being very closely tied together.)

I'm pretty familiar completion in the Eclipse Java Development Tools (JDT) editor, and I think it works pretty well, so what I'm going to do here is first describe the Eclipse behavior in detail, then look at what parts of that carry over to Reinteract and what parts don't or could be improved.

### Eclipse completion behavior ###

 * Completion is triggered by &lt;Control&gt;space. This shows a popup with all the possible completions, with the first one selected.
  * Keybindings:
   * Continuing typing narrows down the list. If a point is reached where there are no possible completions, the popup goes a way. (It doesn't come back if you hit delete).
   * Escape cancels the popup.
   * Return selects the current completion.
   * Up and down move the selected completion.
   * !PageUp/!PageDown move the selected completion by pages.
  * If you hit &lt;Control&gt;Space when there is only one completion that Eclipse knows about it, that completion is inserted without any popup.
  * The completion popup is sized to show 10 possibilities even if there are less
  * When a completion is selected, documentation about that completion is shown in a popup to the right of the completion popup sized to the same height as the completion popup. If the documentation is longer than fits into the window, it is truncated, and there is no way to show the rest.

 * Completion automatically starts after the user types &lt;word&gt;.

 * Completing to a function or constructor inserts (param1, param2, param3) into the buffer with param1 surrounded by a box. Tab after editing param1 moves to param2, and so forth. Tab after editing param3 takes you past the closing ). Tab again cycle back to param1.

 * Normally control-space without any existing word shows all local/global symbols. Inside a function call, without any existing word, control-space shows a popup above the text indicating the function parameters. (Actually the behavior is more complex ... it may show a completion popup for overloads of the function call unless it thinks it knows which overload you want. But that isn't relevant to us.).  The function-parameters popup can be forced with Control-shift even if there is an existing word.

 * Documentation for a symbol is shown as a tooltip when you mouse over it and hover. That tooltip may be truncated, but you can view the rest by hitting F2, which focus the popup and adds a scrollbar if necessary. (Popups always show "Press F2 for Focus" at the bottom until focused)
  * When there is no documentation popup, hitting F2 shows and focuses the popup for the symbol at the insertion cursor location.

 * Alt-/ is "word completion". It cycles through all the possible completions of the current word without any popup.

 * There seems to be some reordering of the set of completion symbols to match what Eclipse thinks it's more likely you will select, though I don't know the exact algorithm.

### Completion in Reinteract ###

I think most the behavior above can be carried over, though it doesn't all need to be implemented at once! The special parameter editing mode is really a different feature, and may not translate well with keyword and optional arguments. One I don't like about the Eclipse system is the F2 to focus a doc tooltip thing, but I can't think of a better way to do it.

 * When completing the first word in a path expression (in "a.b".c) the set of completions should be based on the names in the most recent successfully executed scope.
 * When completing subsequent words in a path expression, it should be done by simply walking the first part ("a.b") using getattr(), and then completing to dir(a.b). Special methods XXXX (_\_getattr\_\_, etc, etc) are ordered after other methods, or omitted completely
.
 * Completion would be disabled for "a.b.c().", since there's too much that could go wrong with making a function call. (In theory evaluating "a.b" could have arbitrary side effects, but that would be evil and I don't think we need to do anything for that other than trap exceptions)
 * Inside a function argument list in a position in the list where a keyword argument would be expected (after all non-keyword arguments), the first completions should be "param1=", "param2=", ...
 * Completion inside an import needs to be handled differently; it probably should work by scanning the directories in sys.path and the notebook's path. A first approximation would be to disable it, since completion to already imported names would just be annoying.



