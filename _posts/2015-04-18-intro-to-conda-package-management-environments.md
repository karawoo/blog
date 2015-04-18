---
layout:     post
title:      Intro to conda package management and environments
date:       2015-04-18 13:07
summary:    The basics for someone new to Python, but not new to programming or package management.
comments:   True
published:  True
tags:       ['Python', 'conda', 'anaconda', 'package management', 'environments']
categories: ['Python']
---

I know a little Python, but I've always been discouraged from learning more by
the notoriously confusing package management system. I'm determined to wrap my
head around this once and for all, so yesterday I asked on Twitter for any
favorite resources on this topic.

<blockquote class="twitter-tweet" lang="en"><p>Hey pythonistas, what are your favorite n00b resources for demystifying Python package management and environments with Anaconda?</p>&mdash; Kara Woo (@kara_woo) <a href="https://twitter.com/kara_woo/status/589208572031938560">April 17, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Some of the responses boiled down to "haha good luck", but some were extremely
helpful and I thought I'd share them here, both for my own future reference and
for anyone else who might find them englightening.

First, [Paul Hobson](https://twitter.com/pmhobson) offered to field questions
over email and wrote an
**[exceedingly helpful gist](https://gist.github.com/phobson/8a4808bf6879f5f2cfb4)**
in response. After reading this I finally feel like I grasp what environments
are beyond "places to have different versions of packages, or something", how to
actually *use* them and install packages, and where everything lives. Many of
the introductions to conda I've read assume that the reader already understands
what environments are and how they relate to the actual scripts/IPython
notebooks/etc. you're writing, but for me this part has been really confusing
until now.

Other tweeps shared some helpful links:

* [Daniel Chen](https://twitter.com/chendaniely) shared
  [his notes](http://chendaniely.github.io/blog/2015/02/05/open-sourcing-a-python-project/)
  on writing a Python package. This is beyond the level of what I'm trying to
  do, but will be very useful if I start writing my own packages.
* [Ethan White](https://twitter.com/ethanwhite) linked to
  [PyData's tutorial](http://conda.pydata.org/#getting-started).
* [Matt Davis](https://twitter.com/jiffyclub) wrote a helpful
  [post](http://penandpants.com/2014/10/09/state-of-conda-oct-2014/) on "the
  good, the annoying, and the bad" of conda as of October 2014. He also linked
  to a Python-for-data-science
  [tutorial](https://tswicegood.github.io/python-data-science-intro/tools.html)
  on Anaconda and IPython notebooks.

Other links I've found helpful:

* [Using the Anaconda Python Distribution](http://davebehnke.com/using-python-anaconda-distribution.html) by Dave Behnke.
* Continuum's [blog post](http://www.continuum.io/blog/conda) on Python packages
  and environments with conda.
