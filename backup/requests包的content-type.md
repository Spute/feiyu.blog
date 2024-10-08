# Media Type媒体类型是什么
- http协议比较关键的请求头：Content-Type
- https://datatracker.ietf.org/doc/html/rfc7231#autoid-8

# 相关

- 媒体类型由主类型和子类型组成。
- 主类型为application的媒体类型主要用于与软件应用程序相关的数据格式。常用的类型如下：
  - application/json JSON格式数据
  - application/xml XML格式数据
  - application/x-www-form-urlencoded 表单数据编码
  - application/octet-stream 二进制流数据
- 媒体类型定义了html body数据的类型


| Media Type媒体类型| 描述| Header |
|--------|--------|--------|
| application/json| Cell | Cell |
| application/x-www-form-urlencoded | Cell | Cell |
| multipart/form-data | Cell | Cell | 



# 应用示例

- 对应requests包该如何使用
- request对象有了data参数，为什么还要加一个json参数呢？是否代表只能二选一

requests是如何将一些python对象转化为HTTP请求体的？

根据是json参数还是data参数

根据data参数接受的类型

根据请求体content type



表单编码是一个过程？

如果存在data或者files参数那么json参数会被忽略。

使用post方法，将json对象传递给json参数与data参数的区别？

使用 json 参数时，requests会将其序列化为 JSON 格式，并设置请求的 Content-Type 为 application/json。


data参数可以接收字典类型，会将字典转换为表单形式的数据，content-type设置成application/x-www-form-urlencoded。此时设置参数headers={ "Content-Type": "application/json"}，服务器会期望请求主体中的数据是 JSON 格式的字符串，但接收的却是表单形式的数据，会导致异常。


- 服务器一个接口会同时支持解析多种内容类型，如django-rest-framework默认支持


# 相关问题

为啥在请求体为表单数据时，并且有文件类型上传时，非文件类型的字段使用json参数不行，必须使用data参数？

requests对请求内容类型的选择：

表单类型有两种格式，第一种普通表单，第二种支持文件类型上传

"Content-Type": "application/x-www-form-urlencoded"    URL 编码格式

"Content-Type": "multipart/form-data; boundary=1b5d8956f98dcd78d7ed9ef293b07fed" 多部分格式

application/json： json格式

application/octet-stream：用于二进制数据

application/xml：用于 XML 数据格式。

application/pdf：用于 PDF 文件。



直观理解请求内容的区别？

postman等直观地网络请求工具。

在 Postman 工具中，form、json、file 和 raw 这四种格式对应不同的请求体格式和请求头。

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


相同内容不同类型的请求体（表单的请求体与json的请求体），web应用都能解析到吗？是怎么兼容的。

drf可以配置多个解析器parser，支持多种类型请求。

django默认只支持 form-urlencoded 和 multipart/form-data 的请求解析，支持其他请求需要额外处理。


在 HTTP 请求中，所有数据最终都以字节流的形式传输，这是因为 HTTP 协议的底层传输是基于 TCP/IP 的，而 TCP/IP 协议只能处理字节流。

