#### HTTP METHOD
HTTP METHOD: CONNECT DELETE GET HEAD OPTIONS PATCH POST PUT TRACE
安全的方法: 不会改变服务的状态，为只读操作.该方法的多次调用不会产生副作用，
副作用指的是资源的状态.
幂等的方法: 安全的方法都是幂等的方法.该方法多次调用返回的效果一致，客户端可以
重复调用并且期望同样的结果.
* 可缓存的方法:  GET HEAD POST
* 非安全的方法: CONNECT DELETE POST PUT PATCH TRACE 
* 安全的方法: GET HEAD OPTIONS
* 幂等的方法: GET HEAD PUT DELETE OPTIONS TRACE
* 非幂等的方法: POST PATCH CONNECT

为什么PATCH不是幂等的?
```
A PATCH is not necessarily idempotent, although it can be. 
Contrast this with PUT; which is always idempotent. The word 
"idempotent" means that any number of repeated, identical requests 
will leave the resource in the same state. For example if an 
auto-incrementing counter field is an integral part of the resource, 
then a PUT will naturally overwrite it (since it overwrites everything), 
but not necessarily so for PATCH.
```
为什么TRACE是不安全的?
```
跨站式追踪攻击(Cross-Site Tracing)
具体而言，是客户端发 TRACE / TRACK 请求至服务器，如果服务器按照标准实现了 TRACE / TRACK 响应，
则在 response body 里会返回此次请求的完整头信息.通过这种方式，客户端可以获取某些敏感的 header 字段，
例如 httpOnly 的 Cookie 等.
```

##### TRACE 
TRACE 请求会在目的服务器端发起一个 环回 诊断。行程最后一站的服务器会弹回一条 TRACE 响应，
并在响应主体中携带它收到的原始请求报文.

##### OPTIONS 
作用: 
1. 检测服务器所支持的请求方法
2. CORS中的预检请求(检测某个接口是否支持跨域)

```
request:
OPTIONS /index.html HTTP/1.1
OPTIONS * HTTP/1.1


response:
HTTP/1.1 204 No Content
Allow: OPTIONS, GET, HEAD, POST
Cache-Control: max-age=604800
Date: Thu, 13 Oct 2016 11:45:00 GMT
Server: EOS (lax004/2813)
```

##### HEAD
HEAD
HEAD 方法与 GET 方法的行为很类似，但服务器在响应中只返回首部.
不会返回实体的主体部分。这就允许客户端在未获取实际资源的情况下，
对资源的首部进行检查.使用 HEAD，可以:

1. 在不获取资源的情况下了解资源的情况(比如，判断其类型);
2. 通过查看响应中的状态码，看看某个对象是否存在;
3. 通过查看首部，测试资源是否被修改了.
服务器开发者必须确保返回的首部与 GET 请求所返回的首部完全相同.遵循 HTTP/1.1 规范，就必须实现 HEAD 方法.
___

### HTTP Protocol

#### HTTP 1.0

#### HTTP 1.1

#### HTTP 2.0

#### HTTP 3.0

___

#### HTTP HEADERS

##### Transfer-Encoding
> [Transfer-Encoding](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Transfer-Encoding)
> 为逐条首部

Directives:
chuncked: 分块传输编码只在HTTP1.1版本中提供.在这种情况下Content-Length头会被忽略.
```
如果一个HTTP消息（请求消息或应答消息）的Transfer-Encoding消息头的值为chunked，
那么，消息体由数量未定的块组成，并以最后一个大小为0的块为结束
每一个非空的块都以该块包含数据的字节数（字节数以十六进制表示）开始，跟随一个CRLF （回车及换行），然后是数据本身，最后块CRLF结束。在一些实现中，块大小和CRLF之间填充有白空格（0x20）
最后一块是单行，由块大小（0），一些可选的填充白空格，以及CRLF.
最后一块不再包含任何数据，但是可以发送可选的尾部，包括消息头字段消息最后以CRLF结尾
HTTP/1.1 200 OK

eg1:
Content-Type: text/plain
Transfer-Encoding: chunked
 
25
This is the data in the first chunk
 
1C
and this is the second one
 
3
con
 
8
sequence
 
0

eg2:
HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked
Trailer: Expires

7\r\n
Mozilla\r\n
9\r\n
Developer\r\n
7\r\n
Network\r\n
0\r\n
Expires: Wed, 21 Oct 2015 07:28:00 GMT\r\n
\r\n
```


##### Content-Encoding
> [Content-Encoding](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding)
> 端到端首部

___

#### URL编码
refs: RFC1738 RFC3986

url只能使用ascii码进行发送，使用十六进制数字的"%"代替不安全字符.
URL不能包含空格，通常使用+号或者%20代替空格.

ASCII TABLE: 

![ASCII TABLE](ASCII.gif)

#### MIME类型

##### application/x-www-form-urlencoded
1. 对于get请求的表单提交(method默认是get):
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>form get</title>
</head>
<body>

<form action="demo-form.php">
First name: <input type="text" name="FirstName" value="Mickey&"><br>
Last name: <input type="text" name="LastName" value="Mouse"><br>
<input type="submit" value="提交">
</form>

</body>
</html>
```
会将参数转换为查询字符串形式https://www.runoob.com/try/demo_source/demo-form.php?FirstName=Mickey%26&LastName=Mouse



2. 对于post请求的表单提交:
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>form post</title>
</head>
<body>

<form action="demo-form.php" method="POST">
First name: <input type="text" name="FirstName" value="Mickey&"><br>
Last name: <input type="text" name="LastName" value="Mouse"><br>
<input type="submit" value="提交">
</form>

</body>
</html>
```
会将参数以key=value&key=value的形式在请求体作为form data出现.
FirstName=Mickey%26&LastName=Mouse

___

##### multipart/form-data
用于上传文件等多种数据传输.
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>form post</title>
</head>
<body>

<form action="demo-form.php" method="POST">
First name: <input type="text" name="FirstName" value="Mickey&你是笨蛋吗"><br>
Last name: <input type="text" name="LastName" value="Mouse"><br>
<input type="submit" value="提交">
</form>

</body>
</html>
```
multipart/form-data不会对参数编码，使用boundary分割每个part.

请求头:
content-type: multipart/form-data; boundary=----WebKitFormBoundaryX62GbrtDMMgqbQTd
请求体:
```txt
------WebKitFormBoundaryX62GbrtDMMgqbQTd
Content-Disposition: form-data; name="FirstName"

Mickey&你是笨蛋吗
------WebKitFormBoundaryX62GbrtDMMgqbQTd
Content-Disposition: form-data; name="LastName"

Mouse
------WebKitFormBoundaryX62GbrtDMMgqbQTd--
```

___

##### application/json
适合restful接口.
content-type: application/json


##### text/plain
使用纯文本进行传输.