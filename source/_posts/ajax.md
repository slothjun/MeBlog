---
title: AJAX
---

## AJAX 简介 

AJAX 全称 Asynchronous JavaScript and XML

AJAX = 异步 JavaScript 和 XML。

AJAX 是一种用于创建快速动态网页的技术。

## AJAX 工作流程图

![](http://39.104.171.29/image/ajax.gif )

## ajax基于Internet

AJAX是基于现有的Internet标准，并且联合使用它们：

  XMLHttpRequest 对象 (异步的与服务器交换数据)
  JavaScript/DOM (信息显示/交互)
  CSS (给数据定义样式)
  XML (作为转换数据的格式)
  lamp  AJAX应用程序与浏览器和平台无关的！

## ajax实例

### 基础实例
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script>
  function loadXMLDoc(){

      var xmlhttp;
      if (window.XMLHttpRequest){
        //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
        xmlhttp=new XMLHttpRequest();
      }
      else{
        // IE6, IE5 浏览器执行代码
        xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
      }
      xmlhttp.onreadystatechange=function(){
        if (xmlhttp.readyState==4 && xmlhttp.status==200){
          document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
        }
      }
      xmlhttp.open("GET","https://www.runoob.com/try/ajax/ajax_info.txt",true);
      xmlhttp.send();
  }
</script>
</head>
<body>

<div id="myDiv"><h2>使用 AJAX 修改该文本内容</h2></div>
<button type="button" onclick="loadXMLDoc()">修改内容</button>

</body>
</html>
```
内容解析：

xmlHttpRequest 请求运行流程 

1.创建xmlHttpRequest对象

2.使用xmlHttpRequest对象开启一个请求方式

3.发送请求
```javascript
1.创建xmlHttpRequest对象
xmlhttp=new XMLHttpRequest(); //创建xmlHttpRequest对象，IE7+, Firefox, Chrome, Opera, Safari 浏览器可执行代码

xmlhttp=new ActiveXObject("Microsoft.XMLHTTP"); //同样也是创建XMLHttpRequest对象，针对IE 6,5老版本的创建方式

2.使用xmlHttpRequest对象开启一个请求

//open(method,url,async) 
//    规定请求的类型、URL 以及是否异步处理请求。(开启一个请求)
//    method：请求的类型；GET 或 POST
//    url：文件在服务器上的位置，服务器请求路径
//    async：true（异步）或 false（同步),ajax中此选项一般都是 true
xmlhttp.open("GET","https://www.runoob.com/try/ajax/ajax_info.txt",true);

//send(string)
//    将请求发送到服务器。
//    string：仅用于 POST 请求,用于添加post请求中的数据
xmlhttp.send();
```
### GET 于 POST 的对比

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。

然而，在以下情况中，请使用 POST 请求：

1.无法使用缓存文件（更新服务器上的文件或数据库）
2.向服务器发送大量数据（POST 没有数据量限制）
3.发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

### xmlHttpRequest.setRequestHeader(header,value)

此方法用于，向请求添加 HTTP 头。

header: 规定头的名称
value: 规定头的值

例如
```html
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
```
### responseText and responseXML

responseText 用于获取服务器中的文本信息资源

responseXML 用于获取服务器中的XML格式资源

使用实例
```html
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```
### onreadystatechange 事件

当请求被发送到服务器时，我们需要执行一些基于响应的任务。

每当 readyState 改变时，就会触发 onreadystatechange 事件。

readyState 属性存有 XMLHttpRequest 的状态信息。

下面是 XMLHttpRequest 对象的三个重要的属性：

onreadystatechange ：存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。

readyState ：
  
    存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。

    0: 请求未初始化

    1: 服务器连接已建立
    
    2: 请求已接收
    
    3: 请求处理中
    
    4: 请求已完成，且响应已就绪

status ：200: OK  HTTP 响应状态码。404 :未找到页面 

使用实例：
```javascript
function myFunction()
{
    loadXMLDoc("/try/ajax/ajax_info.txt",function(){
        if (xmlhttp.readyState==4 && xmlhttp.status==200){
          //要做的事情 todo
            document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
        }
    });
}
```
参考网址：https://www.runoob.com/ajax/ajax-tutorial.html