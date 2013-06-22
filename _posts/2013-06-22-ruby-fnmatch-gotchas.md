---
layout: post
title:  "Using File#fnmatch? to mimick Dir#glob"
date:   2013-06-22 21:33:00
categories: ruby
---

Someone opened an [issue][issue] on sinatra-assetpack recently regarding
unexpected behaviour of our Dir#glob-like feature to match source files in a
given assets package.

I found out that [File#fnmatch?][fnmatch?] method we use to reproduce a glob-like matching
on assets soruces is not behaving exactly the same way a Dir#glob would
under certain conditions.

There was a very tiny hint in the documentation :

>The pattern is not a regular expression; instead it follows rules **similar**
to shell filename globbing.

Specifically in this case, `fnmatch?` will not match starting `/` or sarting `.`
with subfolders `/**/` matching.

But you can specify an option flag to enable matching of those cases
(likely what you expect in most cases) :

{% highlight ruby %}
File.fnmatch?(expected, test, File::FNM_PATHNAME | File::FNM_DOTMATCH)
{% endhighlight %}

[issue]: https://github.com/rstacruz/sinatra-assetpack/issues/108
[fnmatch?]: http://www.ruby-doc.org/core-2.0/File.html#method-c-fnmatch-3F