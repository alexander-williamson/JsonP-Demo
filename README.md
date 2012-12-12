JsonP-Demo
==========

A simple example showing a single cross domain JsonP script load.

Overview
========

There are two basic static HTML Views:

1.	Home (JsonpDemo/Views/Home/Index.cshtml)
2.	Child (JsonpDemo/Views/Child/GetChildContent.cshtml)

Json Results
------------
The magic happens in (JsonpDemo/Controllers/ChildController.cs) after that page is requested from \Home\Index in the &lt;script&gt; injector.

1.	/Child/GetChildContent is called with a single argument. This argument will be the scripts callback function name (e.g. callbackfunctionNameA)
2.	We generate and convert the View to a HTML string
3.	We Jsonify the HTML String and wrap the serialisation
4.	We return a new json object with the callback name wrapping the Html. 

In your browser, navigate to /Home/View

1.	A javascript function has been declared called callbackFunctionNameA. It takes a single json argument and expects that json object to have a .Html html object.
2.	When the /Child/GetChildContent/callbackfunctionNameA is loaded it calls callbackfunctionNameA. The parameter callbackFunctionNameA is the name of the callback function. 
3.	callbackfunctionNameA replaces the HTML content in &lt;div id="childDiv"&gt;&lt;/div&gt;

Misc
====

Using jQuery for simple dom manipulation.
ReturnObject is a basic object to simplify serialization.