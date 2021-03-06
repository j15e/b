---
layout: post
status: publish
published: true
title: XHTML 1.1 ; iframe issue
author: Jean-Philippe Doyle
author_login: admin
author_email: jeanphilippe.doyle@gmail.com
author_url: http://www.devdoyle.com
wordpress_id: 3
wordpress_url: http://blog.devdoyle.com/?p=3
date: 2007-10-24 22:36:42.000000000 -04:00
categories:
- xhtml
- tips
tags:
- xhtml
- iframe
- javascript
comments: []
---
An XHTML 1.1 page <a href="http://www.w3.org/TR/xhtml-media-types/#application-xhtml-xml" title="W3C XHTML media types" target="_blank">should be served as application/xhtml+xml</a> when possible and as text/html if not (for older browsers).

That being said, I was trying to figure out how to include a Google map on that kind of page. Following is the code provided by Google maps :

    <iframe width="674" height="300" frameborder="0" scrolling="no" marginheight="0"
      marginwidth="0" src="http://maps.google.ca/maps?f...."></iframe>'

Simple? Well .. not in this case since you can't use iframe tag in valid XHTML 1.1. But I found using an object tag instead was a good solution for most navigators accepting the application/xhtml+xml content-type :

    <object width="674" height="300" type="text/html"
      data="http://maps.google.ca/maps?f...."></object>

But, it ain't working with older browsers such as IE 5/6. So I thought about using JavaScript, a good idea that require a few tricks.

First, you can't use <em>document.write </em>when served as XML. Doh. But instead of using JavaScript to display either iframe or object tag, I decided to write down the object tag in the document and hide it to old browser which doesn't display it properly and show them the alternate iframe using document.write (which can be use when served as text/html).

Then, to get valid xml when the page is served as application/xhtml+xml, you must use the &lt;![CDATA[]]&gt; tag. But this is not working when served as text/html and will generate syntax errors. So, I divided to comment those tags, and it now works great on all browsers :

    <script type=“text/javascript”>
    /*
    <![CDATA[
    */
    ie = false
    /*@cc_on
    @if (@_win32) ie = true;
    @end
    @*/
    if (ie) document.write('<div style="display:none;">')
    /*
    ]]>
    */
    </script>
    <object width=“674″ height=“300″ type=“text/html” data=“http://maps.google.ca/…”></object>
    <script type=“text/javascript”>
    /*

    <![CDATA[
    */
    if (ie) document.write('</div><iframe width="674" height="300" frameborder="0" scrolling="no"
    marginheight="0" marginwidth="0" src="http://maps.google.ca/..."></iframe>')
    /*
    ]]>
    */
    </script>
