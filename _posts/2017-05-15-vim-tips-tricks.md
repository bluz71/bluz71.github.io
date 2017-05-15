---
title: Vim Tips & Tricks
layout: default
---

Vim Tips & Tricks
=================

As a long time Vim user here are some cool tips n' tricks worth sharing with
the Vim community at large.

One time normal mode command whilst in insert mode
--------------------------------------------------
Whilst in insert mode you can quickly execute a single normal operation via:
{% highlight viml %}
Control-o <<command>>
{% endhighlight %}
When the normal mode command has completed you automatically be returned to
insert mode.

A useful example would be centering the text currently being edited:
{% highlight viml %}
Control-o zz
{% endhighlight %}

Expression register in insert mode
----------------------------------

Set global replacement as a default
-----------------------------------
{% highlight viml %}
set gdefault
{% endhighlight %}

Substitute in a visual block
----------------------------
{% highlight viml %}
'<,'>s/\%Vfoo/bar/c
{% endhighlight %}


Complete a line with *Control-x Control-l*
------------------------------------------

Repeat last visual selection with *gv*
--------------------------------------

Format text with *gq* command
---------------------------

Launch browser with *gx* command
------------------------------

Sort a visual selection
-----------------------

Count the number of pattern matches
-----------------------------------

Delete all lines containing pattern
-----------------------------------

Delete all lines not containing pattern
---------------------------------------

Vim as a *sed* replacement
--------------------------

Completion for spellings
------------------------

Better wrapping with *breakindent*
----------------------------------

{% highlight viml %}
set showbreak=\\\\\
{% endhighlight %}

Smarter *j* and *k* navigation 
------------------------------

{% highlight viml %}
nnoremap <expr> j v:count ? 'j' : 'gj'
nnoremap <expr> k v:count ? 'k' : 'gk'
{% endhighlight %}

Set *infercase*
---------------

{% highlight viml %}
set infercase
{% endhighlight %}

Set *relativenumber*
---------------

{% highlight viml %}
set relativenumber
{% endhighlight %}

Improve performance for files with long lines
---------------------------------------------
{% highlight viml %}
set synmaxcol=200
set norelativenumber
{% endhighlight %}

Enable wildmenu and wildmode
----------------------------

Make dot work over visual line selections
-----------------------------------------
{% highlight viml %}
xnoremap . :norm.<CR>
{% endhighlight %}

Execuate a macro over visual line selections
--------------------------------------------

{% highlight viml %}
xnoremap Q :'<,'>:normal @q<CR>
{% endhighlight %}

Make *Y* should behave like *D* and *C*
----------------------------------------
{% highlight viml %}
noremap Y y$
{% endhighlight %}
