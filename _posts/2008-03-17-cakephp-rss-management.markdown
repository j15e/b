---
layout: post
status: publish
published: true
title: CakePHP ; dynamic RSS management
author: Jean-Philippe Doyle
author_login: admin
author_email: jeanphilippe.doyle@gmail.com
author_url: http://www.devdoyle.com
wordpress_id: 6
wordpress_url: http://blog.devdoyle.com/2008/03/17/cakephp-rss-management/
date: 2008-03-17 21:30:34.000000000 -04:00
categories:
- cakephp
- tips
tags: []
comments: []
---
How to deal with multiple dynamic RSS feeds?

I used 2 objects to manage my RSS, one to store them (component) and one to help displaying them (helper).

<ol>
	<li>Create a component to manage them</li>
	<li>Create an helper to display them (meta-tags and HTML links)</li>
</ol>
<h3>Component</h3>
Let's create a simple component that will allow us to add RSS within our controllers.

    class FeedsListComponent extends Object {

        private $feeds = array();

        function startup() {
        }

        function add($title,$url,$link=true) {
           $this->feeds[] = array($title,$url,$link);
        }

        function feeds() {
           return $this->feeds;
        }
    }

Exemple of usage :

    $this->FeedsList->add('All news','/news/.rss');
    $this->FeedsList->add('Canada news','/news/canada.rss');

The third argument is optional and specify whether or not the link should be automatically listed on the page.

Now you may either
<ul>
	<li>Add feeds that should display on all pages (general ones) directly in the app controller feeds</li>
	<li>Add feeds that you wish to display in specific controllers / actions in those</li>
</ul>
You may add the FeedsList component in the app controller so you can access it from all controllers.
<h3>Helper</h3>
An helpter will help display feeds <em>metas</em> and html <em>links</em>.

    class FeedsListHelper extends AppHelper {

    	var $helpers = array('Html');

    	function metas($feeds){
    		$meta='';
    		foreach($feeds as $feed)
    			$meta .= $this->Html->meta('rss', $feed[1], array('title' => $feed[0]));
    		return $meta;
    	}

    	function links($feeds){
    		$links='';
    		foreach($feeds as $feed) if($feed[2]) $links .= '
    			<div class="rssFeed"">
    			<a href="'.$feed[1].'" title="'.$feed[0].'">
    			<img src="/img/feed-icon-14x14.png" alt="'.$feed[0].'" /></a>
    			<a href="'.$feed[1].'" title="'.$feed[0].'">'.$feed[0].'</a>
    			</div>';
    		return $links;
    	}
    }
<h3>View</h3>
You can then use the 2 helper's functions in your layout/views, providing the FeedList array :

    echo $feedList->metas($feeds);
    echo $feedList->links($feeds);

or directly writing a feed title and url :

    echo $feedList->links(array(array('All news','/news.rss',true)));
