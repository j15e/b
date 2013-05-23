---
layout: post
status: publish
published: true
title: CakePHP ; Image Magick Resize Helper
author: Jean-Philippe Doyle
author_login: admin
author_email: jeanphilippe.doyle@gmail.com
author_url: http://www.devdoyle.com
wordpress_id: 7
wordpress_url: http://blog.devdoyle.com/?p=7
date: 2008-06-01 20:33:19.000000000 -04:00
categories:
- cakephp
tags: []
comments: []
---
*Also published on <a href="http://bakery.cakephp.org/articles/view/image-magick-convert-resizing-helper-with-cache">The Bakery</a> at CakePHP.

Here is my helper based on Josh Hundley <a href="http://bakery.cakephp.org/articles/view/image-resize-helper">image resize helper</a>. My helper is using ImageMagick instead of PHP GD libraries which are more tricky to use and provide less options in my opinion.

You may had more functions to the helper related to ImageMagick abilities. You can find more informations about the resize function and other possibilities at <a href="http://www.imagemagick.org/script/command-line-options.php#resize">ImageMagick documentation.</a>

	class MagickConvertHelper extends Helper {
		var $helpers = array('Html');
		/**
		 * path to cache folder
		 *
		 * @var string
		 * @access public
		 */
			var $cachePath = 'cache';
		/**
		 * path to ImageMagick convert tool
		 *
		 * @var string
		 * @access public
		 */
		var $convertPath = '/usr/bin/convert';

		/**
		 * Automatically resizes an image and returns formatted img tag or only url (optional)
		 *
		 * @param string $filePath Path to the image file, relative to the webroot/ directory.
		 * @param integer $width Width of returned image
		 * @param integer $height Height of returned image
		 * @param boolean $tag Return html tag (default: true)
		 * @param boolean $cachePath Path to cache folder (default: this->cachePath)
		 * @param boolean $quality Quality of returned image (default: 100)
		 * @param boolean $aspect Maintain aspect ratio (default: true)
		 * @param array	$options Array of HTML attributes.
		 * @access public
		 */
		function resize($filePath, $width, $height, $tag=true, $cachePath=null, $quality=100, $aspect=true, $options = array()) {
			if(!$cachePath) $cachePath = $this->convertPath;
			$htmlOptions = $this->__getHtmlOptions($options);

			$fullpath = ROOT.DS.APP_DIR.DS.WEBROOT_DIR.DS.$this->themeWeb;
			$url = $fullpath.$filePath;

			// Verify image exist
			if (!($size = getimagesize($url))) {
				return;
			}

			// Relative file (for return use)
			$relfile = $this->webroot.$this->themeWeb.$cachePath.'/'.$width.'x'.$height.'_'.basename($filePath);
			// Location on server (for resize use)
			$cachefile = $fullpath.$cachePath.DS.$width.'x'.$height.'_'.basename($filePath);


			// Verify if file is cached
			$cached = false;
			if (file_exists($cachefile)) {
				if (@filemtime($cachefile) > @filemtime($url)) {
					$cached = true;
				}
			}

			// Verify resize neccessity
			$resize = false;
			if (!$cached) {
				$resize = ($size[0] > $width || $size[1] > $height) || ($size[0] < $width || $size[1] < $height);
			}

			if ($resize) {
				if($aspect) {
					// Use Image Magick build-in keep ratio option
					$resizeOption = '\'>\'';
				} else {
					$resizeOption = '';
				}
				exec($this->convertPath.' -resize '.$width.'x'.$height.$resizeOption.' -quality '.$quality.' '.$url.' '.$cachefile);
			}

			if($tag) {
				return $this->Html->image($relfile, $htmlOptions);
			} else {
				return $relfile;
			}
		}
	}
