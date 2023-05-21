# [mozilla doc](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)

## 协议结构
![](../img/http_proto.webp)

## header
- GET 
- POST
- PUT
- DELETE
- HEAD 请求一个与 GET 请求的响应相同的响应，但没有响应体
- CONNECT 建立一个到由目标资源标识的服务器的隧道
- OPTIONS 用于描述目标资源的通信选项
- TRACE 沿着到目标资源的路径执行一个消息环回测试
- PATCH 用于对资源应用部分修改


## body
### post三种常见的包体内容




#### application/x-www-form-urlencoded
数据被编码成以 '&' 分隔的键值对，同时以 '=' 分隔键和值。键和值中非字母或数字的字符会被 URL 编码，这也是这种类型不支持二进制数据的原因（应使用 multipart/form-data 代替）。

```http request
POST /test HTTP/1.1
Host: foo.example
Content-Type: application/x-www-form-urlencoded
Content-Length: 27

field1=value1&field2=value2
```



#### multipart/form-data
每个值是在一块数据（“主体部分”）中发送的，由用户代理指定的分隔符（“boundary”）将每个部分分离开。键值在每个部分的 Content-Disposition 标头中给出。

- 上传 example.txt 文件
```http request
POST /upload HTTP/1.1
Host: example.com
Content-Type: multipart/form-data; boundary=---------------------------974767299852498929531610575
Content-Length: 554

-----------------------------974767299852498929531610575
Content-Disposition: form-data; name="file"; filename="example.txt"
Content-Type: text/plain

This is an example file.

-----------------------------974767299852498929531610575--
```

- 下载 example.txt 文件
```http request
HTTP/1.1 200 OK
Content-Type: application/octet-stream
Content-Disposition: attachment; filename="example.txt"

This is the content of the file.
```



- text/plain 或者 application/json
```http request
POST /test HTTP/1.1
Host: foo.example
Content-Type: text/plain
Content-Length: 27

xxxxxxxx
```




## 身份验证机制
值包含Basic认证方案的用户名和密码，这些信息是使用Base64编码的

`root:123456` == `cm9vdDoxMjM0NTY=`

### 基本认证（Basic Authentication）
```http request
GET /index.html HTTP/1.1
Host: www.example.com
Authorization: Basic cm9vdDoxMjM0NTY=
```

- 验证成功包
```http response
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Cache-Control: private, max-age=0
Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
Date: Fri, 21 May 2023 13:30:00 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips mod_fcgid/2.3.9 PHP/5.4.16
Content-Length: 1234

<html>
<head>
<title>Authorization Response</title>
</head>
<body>
<h1>Welcome to My Website!</h1>
<p>You are authorized to access this page.</p>
</body>
</html>
```

- 验证失败响应包
```http response
HTTP/1.1 407 Proxy Authentication Required
Content-Length: 135
Content-Type: text/html
Proxy-Authenticate: Basic realm="Proxy Server"

<html>
<head>
<title>Proxy Authentication Error</title>
</head>
<body>
<h1>407 Proxy Authentication Required</h1>
<p>The proxy server requires authentication.</p>
<p>Please provide proper credentials to continue.</p>
</body>
</html>
```

### 代理验证（Proxy-Authenticate）
```http request
GET /example.html HTTP/1.1
Host: www.example.com
Proxy-Authorization: Basic cm9vdDoxMjM0NTY=
```

- 验证成功包
```http response
HTTP/1.1 401 Unauthorized
Content-Type: text/html; charset=UTF-8
WWW-Authenticate: Basic realm="Restricted Area"
Date: Fri, 21 May 2023 13:35:00 GMT
Content-Length: 345

<html>
<head>
<title>Authentication Error</title>
</head>
<body>
<h1>401 Unauthorized</h1>
<p>You are not authorized to access this resource.</p>
<p>Please provide proper credentials to continue.</p>
</body>
</html>
```

- 验证失败响应包
```http response
HTTP/1.1 407 Proxy Authentication Required
Date: Fri, 21 May 2023 13:19:30 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips mod_fcgid/2.3.9 PHP/5.4.16
Proxy-Authenticate: Basic realm="Proxy Authentication Required"
Content-Length: 381
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<title>407 Proxy Authentication Required</title>
</head>
<body>
<h1>Proxy Authentication Required</h1>
<p>This server could not verify that you are authorized to access the document requested.
 Either you supplied the wrong credentials (e.g., bad password), 
 or your browser doesn't understand how to supply the credentials required.</p>
</body></html>
```
