---
layout: post
status: publish
published: true
title: CakePHP ; Model without table
author: Jean-Philippe Doyle
author_login: admin
author_email: jeanphilippe.doyle@gmail.com
author_url: http://www.devdoyle.com
wordpress_id: 5
wordpress_url: http://blog.devdoyle.com/2008/03/17/cakephp-model-without-a-table/
date: 2008-03-17 20:53:05.000000000 -04:00
categories:
- cakephp
- tips
tags:
- tableless
comments: []
---
Why do this? To validate data on a form that won't be stored in a database and still use both validation component and form helper.

Exemple : you want to create a form that allow users to share by email any article on your website.
<h3>1 - Model</h3>

	class ArticleEmail extends AppModel {

		// Specify to not use a table
		var $useTable = false;

		// Specify your schema (instead of a table)
		var $_schema = array(
			'to_email' => array(
				'type' => 'string',
				'length' => 100),
			'message' => array(
				'type' => 'text'),
			'from_name' => array(
				'type' => 'string',
				'length' => 100),
			'from_email' => array(
				'type' => 'string',
				'length' => 100),
		);

		// Usual validations
		var $validate = array(
			'to_email' => array(
				'rule'=> VALID_EMAIL,
				'message'=> 'Email must be valid'
				),
			'message' => VALID_NOT_EMPTY,
			'from_name' => VALID_NOT_EMPTY,
			'from_email' => array(
				'rule'=> VALID_EMAIL,
				'message'=> 'Email must be valid'
				),
		);
	}
<h3>2 - View</h3>
Same thing as if it  was a database model (that's what you want).
<h3>3 - Controller</h3>
Specify to use the tableless model if you are using your model inside an other controller (here I am using ArticleEmail in my Article controller for the form that sends an article link to a user friend).

When you add models to a controller $uses, include the original model too :

	var $uses = array('Article','ArticleEmail');

Instead of the usual "create/update" code :

	$this->Article->create();
	if ($this->Article->save($this->data)) {

You must use this one that won't try to save it to the database when all fields validates :

	$this->ArticleEmail->create();
	$this->ArticleEmail->data = $this->data;
	if($this->ArticleEmail->validates()) {

Notice that you must set the data before using validates() as opposite to save() method.

You are done. Everything else will be done automatically as usual.
