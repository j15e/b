---
layout: post
title:  "Using jekyll pipieline"
date:   2013-06-17 21:33:00
categories: jekyll
---

I would like to use SASS to build a nice theme so let's try [jekyll-asset-pipeline][pipe].

**Update:** Using other gems is not safe and builds aren't done by github.com, you must
build & push to gh-pages manually from this point. Duh!

Found a nice [Rakefile][rakefile] from [@ixti][ixti] to generate & push to master.

As github pages for users must be sent on `master` when generated manually,
I have created a new branch `source` to keep track of my changes.

[pipe]: https://github.com/matthodan/jekyll-asset-pipeline
[rakefile]: https://github.com/ixti/ixti.github.com/blob/source/Rakefile
[ixti]: https://github.com/ixti