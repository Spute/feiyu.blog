# 原因
- json不支持正负无穷的表述，python应用程序中，需要使用到正负无穷数据。python如何对包含正负无穷的json进行序列化与反序列化。
- RFC 8259 的正式文档规定，json的值只允许6种类型：对象（object）、数组（array）、字符串（string）、数字（number）、布尔值（boolean）和 null 值

[RFC 8259 - The JavaScript Object Notation (JSON) Data Interchange Format](https://datatracker.ietf.org/doc/html/rfc8259)

# 情况
- python对正负无穷数据json序列化后是什么？

```
import json
json.dumps({1:float('inf'),2:float('-inf')})
'{"1": Infinity, "2": -Infinity}'
```

使用JavaScript反序列化报错
```
const jsonString = '{"1": Infinity, "2": -Infinity}';
const obj = JSON.parse(jsonString);
console.log(obj.value);
VM187:1 Uncaught SyntaxError: Unexpected token 'I', "{"1": Infinity, "... is not valid JSON
    at JSON.parse (<anonymous>)
    at <anonymous>:2:18
```

使用python反序列化成功
```
r = json.loads('{"1": Infinity, "2": -Infinity}')
print(type(r["1"]))
<class 'float'>

```

# 自定义

```
import json
json_str = '{"1": Infinity, "2": -Infinity}'
# 自定义解析方式
def parse_constant(s):
    if s == 'Infinity':
        return "正无穷"
    elif s == '-Infinity':
        return "负无穷"
    else:
        return s
print(json.loads(json_str, parse_constant=parse_constant))
{'1': '正无穷', '2': '负无穷'}

```

# 方案
python的json模块的load和loads的区别是什么？前者接收字节流对象，后者接收的是字符串

parse_constant的参数指定了3个特殊的字符常量'-Infinity' 、 'Infinity' 、 'NaN'，为拓展属性。json的定义并不支持'-Infinity' 、 'Infinity' 、 'NaN'。

python中float('inf')可以表示正无穷，如果只用print出来是‘inf’, 序列化出来是“Infinity”