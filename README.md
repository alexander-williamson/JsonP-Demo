JsonP-Demo
==========

A simple example showing a single cross domain JsonP script load.

Overview
========

There are two Controller/View pairs:

1.	Home
2.	Child

In your browser, navigate to /Home/View

1.	A javascript function has been declared called callbackFunctionNameA. It takes a single json argument and expects that json object to have a .Html html object.
2.	When the /Child/GetChildContent/callbackfunctionNameA is loaded it calls callbackfunctionNameA. The parameter callbackFunctionNameA is the name of the callback function. 
3.	callbackfunctionNameA replaces the HTML content in <div id="childDiv"></div>

Misc
====

Using jQuery for simple dom manipulation.