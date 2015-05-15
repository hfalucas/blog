---
title: AngularJS Client Side Downloads
categories:
  - tutorials
---
<p>When you click a button to download content often the magic to download the file is happening server-side. In the server-side language you use (PHP, Python, Ruby, etc) server side headers are set.
</p>
<p>
	In PHP you would do something like this:
</p>
<pre class="prettyprint lang-php">header('Content-type: application/pdf');
header('Content-Disposition: attachment; filename="filename.pdf"');
readfile($file);
</pre>
<p>
	In the above code we are setting the content mime-type and then setting the content-disposition as an attachment. This is a pretty simple example of how to do downloaded files server-side.</p>
<p>
	But wait...
</p>
<p>[=This is AngularChat, not ServerSideChat. How do we do this the AngularJS way? This 
	<a href="http://jsfiddle.net/gukQp/" target="_blank">fiddle</a> shows a fantastic way of doing so.
</p>
<p>
	First we set up our HTML to attach a controller class to the view. We then create an AngularJS directive that will print out an anchor for the view.
</p>
<pre class="prettyprint lang-html">&lt;div ng-app="myApp"&gt;
    &lt;div ng-controller="MyCtrl"&gt;
        &lt;my-download id="content" get-data="getBlob()" /&gt;
    &lt;/div&gt;
&lt;/div&gt;
</pre>
<p>
	In our controller for the AngularJS code we can return a Blob type. A Blob object represents a file-like object of raw data. In this example we will return JSON, however if you know the raw data of the file you wish to download you can change this getBlob method.=]</p>
<pre class="prettyprint lang-javascript">var module = angular.module('myApp', []);
module.controller('MyCtrl', function ($scope){
    var data = {a:1, b:2, c:3};
    var json = JSON.stringify(data);
    $scope.getBlob = function(){
        return new Blob([json], {type: "application/json"});
    }
});
module.directive('myDownload', function ($compile) {
    return {
        restrict:'E',
        scope:{ getUrlData:'&amp;getData'},
        link:function (scope, elm, attrs) {
            var url = URL.createObjectURL(scope.getUrlData());
            elm.append($compile(
                '&lt;a class="btn" download="backup.json"' +
                    'href="' + url + '"&gt;' +
                    'Download' +
                    '&lt;/a&gt;'
            )(scope));
        }
    };
});
</pre>
<p>
	When this is all said and done, this is a cool way to use AngularJS directives to download files on the client-side. Directions from this post were found on <a href="http://stackoverflow.com/questions/16342659/directive-to-create-adownload-button" target="_blank">StackOverflow</a></p>