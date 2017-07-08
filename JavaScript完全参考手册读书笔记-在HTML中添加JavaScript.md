# 在HTML中添加JavaScript

1. 在&lt;script>元素中
```javascript
alert("this is javascript");

document.write("<script>alert('safe!');<"+"/script>");
```
2. 在HTML事件处理程序特征中，例如onclick

```html
<html>
<head>
<meta charset="utf-8">
<title>HTML Event Handler Attributes</title>
</head>
<body onload="alert('page loaded');" onunload="alert('page unloaded');">
<form>
	<input type="button" value="press me" onclick="alert('You pressed my button!');">
</form>
<p><a href="#" onmouseover="alert('rolled over');">Roll this link</a></p>
</body>
</html> 
```

```html
<html>
<head>
<meta charset="utf-8">
<title>Event Trigger Example</title>
<script>
function alertTest() {
  alert("Danger! Danger!");
}
</script>
</head>
<body>
<form action="#" method="get">
  <input type="button" value="Don't push me!" onclick="alertTest();">
</form>
</body>
</html>


```
3. 作为链接文件，使用&lt;script>元素的src特征引用
```html
<html>
<head>
<meta charset="utf-8">
<title>Linked Script</title>
<script src="safecombined.js"></script>
</head>
<body>
<form action="#" method="get">
	<input type="button" id="button1" value="press me">
</form>
</body>
</html> 
```
```javascript
/*safecombined.js*/
var JSREF = {};
 
JSREF.addLoadEvent = 
    function(newFunction) { 
	  var oldFunction = window.onload; 
	  if  (typeof window.onload != "function") 
	     { 
		   window.onload = newFunction; 
		 } 
	  else 
	     { 
		   window.onload = function () { 
		     if (oldFunction) { oldFunction(); } 
			   newFunction(); 
			};
		  }   
	 }; 
	

JSREF.alertTest = function (){ alert("Danger! Danger!");};

JSREF.bindEvents = function () {document.getElementById('button1').onclick= function () { JSREF.alertTest(); } };

JSREF.addLoadEvent(JSREF.bindEvents); 
```
4. 使用伪URL javascript:某些链接中的语法
```html
<a href="javascript:void(0);" onclick="alert(111)">点我点我</a>

```