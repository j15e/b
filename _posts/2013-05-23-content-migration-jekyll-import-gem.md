---
layout: post
title:  "Content migration with jekyll-import gem"
date:   2013-05-23 17:22:00
categories: jekyll update
---

I migrated posts from my very old Wordpress blog to try out Jekyll migration
process using [jekyll-import][jekyll-import] gem as suggested in Jekyll
[migrations manual][migrations].

Things went pretty smooth except for the dependencies management of the importer.
Because there is a lot of importers in the same gems, all dependencies are not
mandatory and you have to install each of them manually to use your importer
(ex. sequel & mysql gems for the Wordpress importer).

I opened an [issue][issue] and got an answer pretty quick by the maintainer.
A dicussion about suggested solutions is already going on, which is great!

In the meantime, enjoy reading my PHP and XHTML papers (your know, CakePHP was
pretty cool back then).

[jekyll-import]: https://github.com/jekyll/jekyll-import
[migrations]: http://jekyllrb.com/docs/migrations/
[issue]: https://github.com/jekyll/jekyll-import/issues/26