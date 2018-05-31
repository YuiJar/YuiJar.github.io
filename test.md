---
Ajax 和 jQuery中的Ajax 的理解和使用
---
##一、什么是Ajax
Ajax 是 Asynchronous JavaScript And XML 的缩写，即异步JavaScript 和 XML 技术。用来实现与服务器进行异步交互的功能。

Ajax 和 jQuery中的Ajax 都是针对浏览器无状态情况下的解决方案，jQuery中的Ajax 是对Ajax的封装。
##二、Ajax的具体使用
###2.1 Ajax对象创建
Ajax 中最核心的技术就是XMLHttpRequest

####2.1.1 主流浏览器
主流浏览器包括 火狐、Google 、Safari 、 Opera 等，具体语法格式如下：
	
    var obj = new XMLHttpRequest(); 
####2.1.2 IE 浏览器
这里的IE 浏览器是指IE7 以下的版本

    // IE6, IE5 浏览器执行代码 
    var obj = new ActiveXObject("Microsoft.XMLHTTP"); 

在开发实际的项目时，需要兼容不同版本的浏览器，我们在创建Ajax 对象是需要进行相关判断，具体实现代码如下：

	<!DOCTYPE html>  
	<html lang="en">  
	<head>  
	    <meta charset="UTF-8">  
	    <title>创建Ajax对象</title>  
	</head>  
	<body>  
	<script type="text/javascript">  
	    //判断用户的浏览器类型，决定使用何种Ajax对象  
	    if (window.XMLHttpRequest){  
	        // IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码  
	        var obj = new XMLHttpRequest();  
	    }else{  
	        // IE6, IE5 浏览器执行代码  
	        var obj = new ActiveXObject("Microsoft.XMLHTTP");  
	    }  
	    alert(obj);  
	</script>  
	</body>  
	</html>  

###2.2 Ajax 常用方法和属性

<table>
<thead>
<tr>
  <th>方法</th>
  <th>描述</th>
</tr>
</thead>
<tbody>
<tr>
  <td>open(method,url,async)</td>
  <td>规定请求的类型、URL 以及是否异步处理请求。<br/>
      method：请求的类型；GET 或 POST<br/>
      url：文件在服务器上的位置<br/>
      async：true（异步）或 false（同步）
  </td>
</tr>
<tr>
  <td>send(string)</td>
  <td>将请求发送到服务器。<br/>
      string：仅用于 POST 请求
  </td>
</tr>
<tr>
  <td>setRequestHeader("header","value")	</td>
  <td>用于单独指定请求的某个HTTP头，此方法一般在请求类型
      为POST时使用，并且必须在open()之后调用<br/>
      header：指定HTTP头<br/>
      value ： 指定HTTP头的值
  </td>
</tr>
</tbody>
</table>


<table>
<thead>
<tr>
  <th>属性</th>
  <th>描述</th>
</tr>
</thead>
<tbody>
<tr>
  <td>responseText</td>
  <td>获得字符串形式的响应数据。
  </td>
</tr>
<tr>
  <td>responseXML</td>
  <td>获得 XML 形式的响应数据。
  </td>
</tr>
<tr>
  <td>onreadystatechange</td>
  <td>存储函数（或函数名），每当 readyState 属性改变时就会调用该函数。
  </td>
</tr>
<tr>
  <td>readyState</td>
  <td>存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。<br/>
0: 请求未初始化<br/>
1: 服务器连接已建立<br/>
2: 请求已接收<br/>
3: 请求处理中<br/>
4: 请求已完成，且响应已就绪
  </td>
</tr>
<tr>
  <td>status</td>
  <td>200: "OK"<br/>
404: 未找到页面
  </td>
</tr>
</tbody>
</table>

接下来通过一个实例来演示Ajax(GET方法)：

	<!DOCTYPE html>  
	<html lang="en">  
	<head>  
	    <meta charset="UTF-8">  
	    <title>Ajax实例演示（GET）</title>  
	</head>  
	<body>  
	<script type="text/javascript">  
	    //创建Ajax对象  
	    var obj = new XMLHttpRequest();  
	    //感知Ajax状态，当Ajax状态发生改变时会触发onreadystatechange  
	    obj.onreadystatechange = function(){  
	        if(obj.readyState == 4 && obj.status == 200){  
	            var content = obj.responseText;  
	            alert(content);  
	        }  
	    }  
	  
	    //创建一个http请求，并设置请求地址  
	    obj.open("get","./test.php?id=1");  
	  
	    //发送请求  
	    obj.send();  
	      
	  
	</script>  
	</body>  
	</html>  

<font color=red >
注意：在使用GET方式传递中文参数时，要使用 JavaScript 中的 encodeURIComponent() 函数经中文字符换成十六进制形式，防止在某些浏览器（如IE浏览器）中出现中文乱码问题
</font>

接下来通过一个实例来演示Ajax(POST方法)：

	<!DOCTYPE html>  
	<html lang="en">  
	<head>  
	    <meta charset="UTF-8">  
	    <title>Ajax实例演示(POST)</title>  
	</head>  
	<body>  
	<script type="text/javascript">  
	    //创建Ajax对象  
	    var obj = new XMLHttpRequest();  
	    //感知Ajax状态，当Ajax状态发生改变时会触发onreadystatechange  
	    obj.onreadystatechange = function(){  
	        if(obj.readyState == 4 && obj.status == 200){  
	            var content = obj.responseText;  
	            alert(content);  
	        }  
	    }  
	  
	    //创建一个http请求，并设置请求地址  
	    obj.open("post","./test.php");  
	  
	    //设置HTTP头协议  
	    obj.setRequestHeader("content-type","application/x-www-form-urlencoded");  
	    var info = "id=1";  
	  
	    //发送请求  
	    obj.send(info);  
	      
	  
	</script>  
	</body>  
	</html>  

##三、jQuery  中的 Ajax 的具体使用
####实例 <br/>
通过 AJAX 加载一段文本：

jQuery 代码：

	$(document).ready(function(){
	  $("#b01").click(function(){
	  htmlobj=
	$.ajax({url:"/jquery/test1.txt",async:false})
	;
	  $("#myDiv").html(htmlobj.responseText);
	  });
	});

HTML 代码：

	<div id="myDiv"><h2>Let AJAX change this text</h2></div>
	<button id="b01" type="button">Change Content</button>

或者

	<!DOCTYPE HTML>  
	<html>  
	<head>  
	    <meta charset="UTF-8">  
	    <title>jQuery 中的 Ajax实例演示</title>  
	    <script type="text/javascript" src="./jquery-1.8.2.min.js"></script>  
	</head>  
	<body>  
	    <div id="myself"><h2>通过 AJAX 改变文本</h2></div>  
	    <button id="test" type="button">Ajax改变内容</button>  
	  
	    <script type="text/javascript">  
	        $(function(){  
	            //按钮单击时执行  
	            $("#test").click(function(){      
	                //Ajax调用处理  
	                $.ajax({  
	                    type: "POST",  
	                    url: "./test.php",  
	                    data: "id=1",  
	                    success: function(data){  
	                        $("#myself").html(data);  
	                    }  
	                       
	                });  
	                  
	             });  
	        });  
	    </script>    
	</body>  
	</html>  

具体方法属性详情可见 <br/>
http://www.w3school.com.cn/jquery/ajax_ajax.asp
http://www.w3school.com.cn/jquery/jquery_ajax_get_post.asp



若有错误，欢迎指正
