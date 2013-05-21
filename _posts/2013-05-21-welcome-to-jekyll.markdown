---
layout: post
title:  "Trying Jekyll!"
date:   2013-05-21 18:15:26
categories: jekyll update
---

Let's see what's so cool about this! Here are my first comments & problems with jekyll.

I know some of theses comments will probably sounds stupid when I'll read in a few days.

## Not watching changes by default

I was a bit confuse by `jekyll serve` not watching changes by default : why would you
run the server if you never update content? Search the help and found `jekyll serve -w`.

## Problem with Python dependency

The `pygments` gem is enable by default and it creates a ruby segfault on my setup.
I do have python2, but for some weird reasons, I get a ruby + python segfault at the
same time!

Pygments can be disable with a config key found in the scafolled `_config.yml`.
Not very documented, but there by default.

I think a project like this should avoid mixed languages dependencies by default as it
complexify the setup process.

## Underscores?

Why does everything has to begin with an underscore? Weird.

## Not using a Gemfile?

The documentation does not suggest using a Gemfile : I just don't like this.

Also, the default setup does not ignore the Gemfile and copy it to destination folder.