---
title: 'phantomjs的domready事件实现'
layout: post
tags:
    - phantomjs
    - FE
---
phantomjs原生只有onload事件，而没有domready事件的实现。以下是自己的实现：

	const PHANTOM_FUNCTION_PREFIX = '/* PHANTOM_FUNCTION */';

	var page = require('webpage').create();

	page.onConsoleMessage = function(msg) {
	    if (msg.indexOf("PHANTOM_FUNCTION") === 0) {
		page.onDOMContentLoaded();
	    } else {
		console.log(msg);
	    }
	};

	page.onInitialized = function() {
	    page.evaluate(function(domContentLoadedMsg) {
		document.addEventListener('DOMContentLoaded', function() {
		    console.log("PHANTOM_FUNCTION");
		}, false);
	    }, "PHANTOM_FUNCTION");
	};

	page.onDOMContentLoaded = function() {
	    // your code here
	    console.log('DOMContentLoaded');
	};
