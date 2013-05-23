---
layout: post
status: publish
published: true
title: Using anchor target with XHTML1.1
author: Jean-Philippe Doyle
author_login: admin
author_email: jeanphilippe.doyle@gmail.com
author_url: http://www.devdoyle.com
wordpress_id: 8
wordpress_url: http://blog.devdoyle.com/?p=8
date: 2008-06-03 16:21:35.000000000 -04:00
categories:
- xhtml
- tips
tags: []
comments: []
---
First, you are not supposed to do so if you follow W3C guidelines. But you may want to do it for valuable reasons. In fact, W3C goal is to transfer target attribute from HTML to CSS, which make a lot of sense because a <em>target</em> is more about the way a page is displayed than its content. But CSS3, which should allow this, is not out yet and won't be supported widely any time soon.

There is a <em>target</em> attribute in XHTML definition (as it does in XHTML1.0 transitional) but is not part of XHTML1.1 since strict version (HTML 4.1 also) it is intend to be display on any device, which  may or may not support target/frames (like mobile phone). But you may want to use <em>target</em> for those which can support it, since it should not create any issue if links open in a new window only when possible. This would be possible and clean using CSS3 style sheet but eh, doesn't exist yet!

I have found two way to use the target attribute without getting invalid code : either modify the XHTML1.1 DTD and create your own DTD (Document Type Definition) adding support for the Target module (which won't be XHTML1.1 anymore, but will validate) or add some JavaScript. The concern attribute is, obviously, part of the <a href="http://www.w3.org/TR/xhtml-modularization/abstract_modules.html#s_targetmodule" target="_blank">Target module</a>. I won't explain how to create your own DTD here since it is a bit more complex, but you can read more about how to do so at <a href="http://www.w3.org/MarkUp/Guide/xhtml-m12n-tutorial/">W3C</a> (explanations aren't that clear).

In order to bypass anchor <em>target</em> using JavaScript, you can use instead of <em>target</em> attribute, the <em>rel </em>attribute, which his use is not  intend to be use like that, but it will work and be valid.

    <a rel="_blank" href="example.html">Exemple</a>

Then add this piece of JavaScript code that will create the target attribute from the rel attribute for all anchor in current page :

    function anchorRel() {
      if (!document.getElementsByTagName) return
      var anchors = document.getElementsByTagName("a")
      for (var i=0; i<anchors.length; i++) {
        anchors[i].target = anchors[i].getAttribute("rel")
      }
    }

And add the <em>anchorRel</em> function to your initializing JS function or, if you do not use any, you can simply add the following :

    window.onload = anchorRel;
