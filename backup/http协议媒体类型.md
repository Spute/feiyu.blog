# 前言
- http报文由起始行、消息头、空行和消息体四部分组成（请求报文和响应报文相同）。http协议对消息体内容做了哪些约定。
- 通讯双方如何识别和处理消息体？通过一些关键的消息头实现。常见的消息头有：

| 消息头                | 含义                                             | 类型     |
|---------------------|------------------------------------------------|--------|
| Content-Type        | 指定消息体的数据类型，告知接收方如何解析消息体。           | 请求头/响应头 |
| Content-Length      | 指定消息体的字节长度，告知接收方读取多少字节的数据。      | 请求头/响应头 |
| Transfer-Encoding    | 指定消息体的传输方式，常用于分块传输编码。                | 请求头/响应头 |
| Content-Encoding    | 指定消息体的压缩编码方式，表示消息体经过某种压缩算法。      | 请求头/响应头 |
| Content-Disposition  | 指定消息体作为内联内容显示还是作为附件下载。              | 响应头    |
| Accept              | 指定客户端能够接收的内容类型，用于内容协商。               | 请求头    |

- 其中Content-Type消息头声明了消息体的数据类型，是解析消息体的关键。

# 媒体类型
- 是什么？HTTP 协议中的 Media Type（媒体类型），也称为 MIME Type（多用途互联网邮件扩展类型），是用来表示在 HTTP 请求或响应中传输的数据格式。
  

>全称：Multipurpose Internet Mail Extensions Type。 MIME 最初是为了扩展电子邮件协议（SMTP）而设计的，以允许在电子邮件中传输除纯文本以外的其他类型的数据（如图像、音频、视频和应用程序文件）。

 
- https://datatracker.ietf.org/doc/html/rfc7231#autoid-8
- 媒体类型由主类型和子类型组成。
- 媒体类型的基本格式为：type/subtype
  - type：主类型（例如，text、image、audio、video、application等）。主类型为application的媒体类型主要用于与软件应用程序相关的数据格式。
  - subtype：子类型，具体描述数据格式。

- 常用的类型如下：

| 媒体类型                      | 说明                          | 使用场景                                      |
|------------------------------|-----------------------------|---------------------------------------------|
| **text/plain**                | 纯文本数据                    | 传输简单的文本文件，或 API 响应中不含格式的数据            |
| **text/html**                 | HTML 文档                     | 传输网页内容，浏览器加载的主要文件类型                    |
| **text/css**                  | CSS 样式表文件                 | 在网页中用于定义 HTML 元素的样式                       |
| **text/javascript**           | JavaScript 代码文件             | 在网页中用于传输和执行 JavaScript 脚本代码            |
| **application/json**          | JSON 格式数据                  | API 请求和响应中传输结构化数据，尤其在 REST API 中使用    |
| **application/xml**           | XML 格式数据                   | 传输和存储结构化数据，尤其在一些旧式的 API 和配置文件中使用 |
| **application/octet-stream**  | 二进制文件流                   | 下载或上传文件时使用，不限定具体格式                      |
| **application/pdf**           | PDF 文档                       | 在线传输或下载 PDF 格式的文档                         |
| **application/zip**           | ZIP 压缩文件                   | 传输压缩归档文件，通常用于文件打包                        |
| **image/jpeg**                | JPEG 图像                      | 传输照片或图片，广泛用于网页、电子邮件等场景               |
| **image/png**                 | PNG 图像                       | 传输无损压缩的图片，常用于网页设计                        |
| **image/gif**                 | GIF 图像                       | 传输简单的动画或低色彩图像，常用于网页中的图形和表情符号       |
| **audio/mpeg**                | MP3 音频文件                    | 传输音频数据，尤其在音乐和播客流媒体服务中使用               |
| **video/mp4**                 | MP4 视频文件                    | 传输视频内容，常用于流媒体平台和视频嵌入网页                |
| **application/x-www-form-urlencoded** | 表单编码类型，使用键值对和 URL 编码形式传输数据                 | 提交 HTML 表单时的默认编码方式，特别是使用 `POST` 方法时。  |
| **multipart/form-data**       | 表单数据                       | 文件上传，或表单数据中含有文件的场景                        |
| **application/javascript**    | JavaScript 应用程序文件           | 传输和执行 JavaScript 应用程序代码                      |
| **application/x-www-form-urlencoded** | 表单编码数据                | 提交网页表单时传输表单数据的默认格式                     |
| **application/vnd.ms-excel**  | Excel 文件                    | 传输 Microsoft Excel 表格文档                        |
| **application/msword**        | Word 文档                      | 传输 Microsoft Word 文档                            |

## multipart/form-data 格式
- 作用：适合表单数据中含有文件的场景
- 构成
  - 边界 (boundary)：每个部分（part）之间通过一个边界（boundary）来分隔。边界是一个唯一的字符串，表示各个部分之间的分隔符，由客户端和服务器协商使用，通常由服务器通过 Content-Type 请求头的 boundary 参数来获取。边界的格式为：--boundary_string，每个部分以 --boundary_string 开始，结束时以 --boundary_string-- 表示终止。
  - 部分：每部分的结构： 每个部分的内容由三部分组成：
    - 请求头：每个部分会有自己独立的请求头，用来描述该部分的数据类型和数据名称。常见的头部包括 Content-Disposition 和 Content-Type。
      - Content-Disposition：每个部分的 Content-Disposition 指定了数据的名称和文件名（如果是文件）。form-data 表示表单数据。name 表示该部分的键名。filename（可选）表示上传文件的名称。
      - Content-Type（可选）：如果是上传文件，可以使用 Content-Type 指定文件的 MIME 类型（如 image/png、application/pdf 等）。对于非文件数据，此头部可以省略。
    - 空行：请求头和内容之间会有一个空行。
    - 内容：该部分的实际数据（比如文件或表单数据）。

- multipart/form-data 报文示例

```
POST /upload HTTP/1.1
Host: www.example.com
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 302

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="name"

John Doe
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="example.txt"
Content-Type: text/plain

This is the content of the file.
------WebKitFormBoundary7MA4YWxkTrZu0gW--

```

- application/x-www-form-urlencoded 报文示例
```
POST /submit HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 23

name=John+Doe&age=30

```
# requests包与消息体
- 常见错误：使用requests发起请求，错误使用消息体参数，导致服务端出现解析报错。
  - 奇怪现象：设置请求头为application/json一定报错，不设置反而可能没问题（需要服务端同时支持json格式和url编码的表单数据格式的解析）。
  - 原因：当使用data参数时，消息体使用的application/x-www-form-urlencoded格式，但请求头被定义为application/json，服务端自然无法解析。
```
import requests

headers = {'Content-Type': 'application/json'}
data = {'key': 'value'}

response = requests.post('https://api.example.com/endpoint', data=data, headers=headers)
```
- 使用json参数与data参数的区别？
- 为啥在请求体为表单数据时，并且有文件类型上传时，非文件类型的字段使用json参数不行，必须使用data参数？
- 正确理解requests包的请求参数，其中与消息体有关参数如下，对应的数据格式如下：
  - data：发送 URL 编码的表单数据（消息体数据格式application/x-www-form-urlencoded）。
  - json：发送 JSON 格式的数据（消息体数据格式application/json）。
  - files：用于文件上传，生成 multipart/form-data 格式的消息体。

-  不推荐 requests 包同时传递 data、json 和 files 这三个参数时。因为消息体格式只能对应一种媒体类型，会导致一些意料之外的行为：
  - json 参数会优先于 data 参数。如果两者都提供，只有 json 会被发送。
  - 当同时使用 files 和 json 时，json 数据会被忽略，因为 files 参数会强制使用 multipart/form-data 编码。

- requests是如何将python对象转化为不同媒体类型的HTTP请求？具体逻辑是？
  - 查看相关源码

# 网络请求工具与消息体

- 直观理解请求内容的区别？使用postman等直观地网络请求工具。在 Postman 工具中，form、json、file 和 raw 这四种格式对应不同的请求体格式和请求头。

- form：
  - Content-Type: multipart/form-data
  - Content-Type: application/x-www-form-urlencoded
- json：Content-Type: application/json
- file：Content-Type: multipart/form-data
- raw：手动设置（根据数据格式）
  - Content-Type: application/json （如果是 JSON）
  - Content-Type: text/plain （如果是纯文本）
  - Content-Type: text/xml （如果是 XML）
  - Content-Type: text/html （如果是 HTML）

# 服务端与请求体
- 服务器一个接口可以支持解析多种内容类型（如表单的请求体与json的请求体）解析吗？是怎么兼容的。
  - drf可以配置多个解析器parser，支持多种类型请求。
  - django默认只支持 form-urlencoded 和 multipart/form-data 的请求解析，支持其他请求需要额外处理。

# 浏览器与请求体

1. application/x-www-form-urlencoded 报文

这是 HTML 表单的默认编码类型，表单字段以 URL 编码的方式发送。

表单示例：
```
<form method="POST" action="/submit" enctype="application/x-www-form-urlencoded">
  <input type="text" name="name" value="John Doe">
  <input type="text" name="age" value="30">
  <button type="submit">Submit</button>
</form>
```

2. multipart/form-data 报文
multipart/form-data 通常用于提交包含文件的表单，数据以分块的形式发送，每个字段作为一个独立的部分。
表单示例（含文件上传）：
```
<form method="POST" action="/upload" enctype="multipart/form-data">
  <input type="text" name="name" value="John Doe">
  <input type="file" name="file">
  <button type="submit">Submit</button>
</form>
```