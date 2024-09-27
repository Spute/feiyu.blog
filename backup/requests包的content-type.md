- http协议比较关键的请求头：Content-Type
- https://datatracker.ietf.org/doc/html/rfc7231#autoid-8
- 媒体类型由主类型和子类型组成。
- 主类型为application的媒体类型主要用于与软件应用程序相关的数据格式。常用的类型如下：
  - application/json JSON格式数据
  - application/xml XML格式数据
  - application/x-www-form-urlencoded 表单数据编码
  - application/octet-stream 二进制流数据
- 媒体类型定义了html body数据的类型
Media Type媒体类型
- 对应requests包该如何使用
- 服务器一个接口会同时支持解析多种内容类型，如django-rest-framework默认支持

| Header | Header | Header |
|--------|--------|--------|
| A| Cell | Cell |
| B | Cell | Cell |
| C | Cell | Cell | 