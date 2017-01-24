---
title: Testing
tags: bar1, foo, haskell
date: 30 January, 2015
published: 2015-01-30 08:13:31
---

The Haskell kernel density estimation code in
[the last article][blog8] does work, but it's distressingly slow.
Timing with the Unix `time` command (not all that accurate, but it
gives a good idea of orders of magnitude) reveals that this program
takes about 6.3 seconds to run.  For a one-off, that's not too bad,
but in the next article, we're going to want to run this type of KDE
calculation thousands of times, in order to generate empirical
distributions of null hypothesis PDF values for significance testing.
So we need something faster.

<!--more-->

It's quite possible that the Haskell code here could be made quite a
bit faster.  I didn't spend a lot of time thinking about optimising
it.  I originally tried a slightly different approach using a mutable
matrix to accumulate the kernel values across the data points, but
this turned out to be slower than very simple code shown in the
previous article (something like ten times slower!).  It's clear
though that this calculation is very amenable to parallelisation --
the unnormalised PDF value at each point in the $(\theta, \phi)$ grid
can by calculated independently of any other grid point, and the
calculation for each grid point accesses all of the data points in a
very regular way.
