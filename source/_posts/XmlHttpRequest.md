---
title: XhlHttpRequest
date: 2018-03-11 11:12:42
tags:
---
## 一、什么是XmlHttpRequest
XmlHttpRequest是ajax标准的实现，最先有微软公司设计，随后被Google,Mozilla等使用.现在已成为异步请求的标准所有现代浏览器均支持 XMLHttpRequest 对象（IE5和IE6 使用 ActiveXObject）。XMLHttpRequest用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

## 二、基本使用
```javascript
// 1.创建xhr对象
var xhr = new XmlHttpRequest();
// 2.打开连接
xhr.open('get', 'http://a.com/data.php', true);
// 3.注册数据加载完毕的回调
xhr.onload = function() {
    if(xhr.statu == 200) {
        console.log(xhr.responseText);
    }
};
// 4.发送请求
xhr.send();
```
<!--more-->
## 三、常用方法与属性
设置属性时需要在open方法之后，send方法之前。
### 1.设置请求头
```javascript
xhr.open('get', 'http://a.com/data.php', true);
xhr.setRequestHeader("Content-Type","text/plain");
xhr.send();
```
注意：setRequestHeader必须在open()方法之后，send()方法之前调用，否则会抛错。

### 2.获取响应头
```javascript
var etagHeader = xhr.getResponseHeader('etag');
var allHeaders = xhr.getAllResponseHeaders();
```
注意：
1. getResponseHeader不能获取到Set-Cookie字段，因为w3c标准中基于安全考虑作了限制。
2. getAllResponseHeaders在跨域请求中只能获取到“simple response header”和“Access-Control-Expose-Headers” 里的字段。"simple response header"包括的 header 字段有：Cache-Control,Content-Language,Content-Type,Expires,Last-Modified,Pragma;

### 3.设置返回数据类型
xhr.responseType可设置的数据类型如下
1. ""	String字符串	默认值(在不设置responseType时)
2. "text"	String字符串
3. "document"	Document对象	希望返回 XML 格式数据时使用
4. "json"	javascript 对象	存在兼容性问题，IE10/IE11不支持
5. "blob"	Blob对象
6. "arrayBuffer"	ArrayBuffer对象


### 4.获取返回数据
在加载完数据后可以用以下方式获取返回数据
1. xhr.response
2. xhr.responseText 只有当 responseType 为"text"、""时，xhr对象上才有此属性，此时才能调用xhr.responseText，否则抛错
3. xhr.responseXML 只有当 responseType 为"text"、""、"document"时，xhr对象上才有此属性，此时才能调用xhr.responseXML，否则抛错

### 4.获取请求状态
```
xhr.readyState
```
有以下状态
0 UNSET（初始状态） xhr对象已经被创建，open方法还未调用
1 OPENED（连接打开） open方法已经被调用，send方法还未调用
2 HEADERS_RECEIVED（收到响应头）send方法已经被调用
3 LOADING（正在接收数据）数据传输中
4 DONE（加载完毕）整个传输过程完成，不管成功还是失败

### 5.设置超时时间
```
xhr.timeout = 5000；
```
单位是毫秒，超过此时间将会触发ontimeout事件

### 6.发送cookie信息
```
xhr.withCredentials = true;
```
默认不携带cookie信息，需要设置为true才会发送cookie

## 五、相关事件
### 1.onreadystatechange
每当xhr.readyState改变时触发；但xhr.readyState由非0值变为0时不触发。
### 2.onloadstart
调用xhr.send()方法后立即触发，若xhr.send()未被调用则不会触发此事件。
### 3.onprogress
xhr.onprogress在下载阶段（即xhr.readystate=3时）触发，每50ms触发一次。
### 4.onload
当请求成功完成时触发，此时xhr.readystate=4
### 5.onloadend
当请求结束（包括请求成功和请求失败）时触发
### 6.onabort
当调用xhr.abort()后触发
### 7.ontimeout
xhr.timeout不等于0，由请求开始即onloadstart开始算起，当到达xhr.timeout所设置时间请求还未结束即onloadend，则触发此事件
### 8.onerror
在请求过程中，若发生Network error则会触发此事件。注意，只有发生了网络层级别的异常才会触发此事件，对于应用层级别的异常，如响应返回的xhr.statusCode是4xx时，并不属于Network error，所以不会触发onerror事件，而是会触发onload事件。
### xhr.upload的事件
1. xhr.upload.onloadstart
2. xhr.upload.onprogress xhr.upload.onprogress在上传阶段(即xhr.send()之后，xhr.readystate=2之前)触发，每 50ms触发一次；
3. xhr.upload.onload 若发生Network error时，上传还没有结束，则会先触发xhr.upload.onerror，再触发xhr.onerror；若发生Network error时，上传已经结束，则只会触发xhr.onerror
4. xhr.upload.onloadend
