JsonP-Demo
==========

A simple example showing a single cross domain JsonP script load.

Overview
========

There are two basic static HTML Views:

1.	Home (JsonpDemo/Views/Home/Index.cshtml)
2.	Child (JsonpDemo/Views/Child/GetChildContent.cshtml)

There are two basic controllers for each of the views.

Step by Step
============

Load up /Index/Home. This is a basic html page with a couple of existing scripts setup.

	<head>
		<title>JsonP Demo : Home Page</title>
		<script src="http://ajax.aspnetcdn.com/ajax/jquery/jquery-1.8.0.js" type="text/javascript"></script>
		<script type="text/javascript">
			function callbackFunctionNameA(data) {
				$("#childDiv").html(data.Html);
			}
		</script>
	</head>
	
In the <body> tag of Home/Index, there is a placeholder for the content &lt;div id="childDiv"&gt;&lt;/div&gt;. Content will be loaded into here by callbackFunctionNameA.
	
	<div id="childDiv"></div>
    <script src="/Child/GetChildContent/callbackFunctionNameA" type="text/javascript"></script>

/Child/GetChildContent is a partial HTML page. This will be loaded into a JsonP object, returned and then injected by callbackFunctionNameA.

	<h2>Child Content</h2>
	<p>This is the child :) which has come from a different controller. This page could be located in a separate domain if required.</p>
	
/Child/GetChildContent/callbackFunctionNameA will return a JsonP object. Note the callback function (callbackFunctionNameA) has been prepended to the Json object.

	callbackFunctionNameA({"Html":"\r\n\r\n\u003ch2\u003eChild Content\u003c/h2\u003e\r\n\u003cp\u003eThis is the child :) which has come from a different controller. This page could be located in a separate domain if required.\u003c/p\u003e"});
	
As this is treated as an external script object, it will be evaluated and will call callbackFunctionNameA with the json object as the parameter.
We inject the data.Html into the childDiv element.

	$("#childDiv").html(data.Html);
	
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