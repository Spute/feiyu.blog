# 原因
- json不支持正负无穷的表述，python应用程序中，需要使用到正负无穷数据。python如何对包含正负无穷的json进行序列化与反序列化。
- RFC 8259 的正式文档规定，json的值只允许6种类型：对象（object）、数组（array）、字符串（string）、数字（number）、布尔值（boolean）和 null 值

[RFC 8259 - The JavaScript Object Notation (JSON) Data Interchange Format](https://datatracker.ietf.org/doc/html/rfc8259)

# 情况
- python对正负无穷数据json序列化后是什么？
- https://docs.python.org/3/library/json.html#infinite-and-nan-number-values

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

# 自定义序列化方式
加入allow_nan=False限制特殊数据的序列化，防止其他应用程序反序列化时异常
```
import json
obj = {1:float('inf'),2:float('-inf')}
print(json.dumps(obj, allow_nan=False))
Traceback (most recent call last):
  File "<input>", line 3, in <module>
  File "/usr/lib/python2.7/json/__init__.py", line 251, in dumps
    sort_keys=sort_keys, **kw).encode(obj)
  File "/usr/lib/python2.7/json/encoder.py", line 207, in encode
    chunks = self.iterencode(o, _one_shot=True)
  File "/usr/lib/python2.7/json/encoder.py", line 270, in iterencode
    return _iterencode(o, 0)
ValueError: Out of range float values are not JSON compliant

```


序列化前替换特殊数据
```
# 替换前
import json
obj = {1:float('inf'),2:float('-inf')}
print(json.dumps(obj))
{"1": Infinity, "2": -Infinity}

# 替换后
import json
obj = {1:float('inf'),2:float('-inf')}
obj[0]="Infinity"
obj[1]="-Infinity"
print(json.dumps(obj))
{"0": "Infinity", "1": "-Infinity", "2": -Infinity}

```


# 自定义反序列方式

parse_constant的参数指定了3个特殊的字符常量'-Infinity' 、 'Infinity' 、 'NaN'，为拓展属性。
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

# 其他
python的json模块的load和loads的区别是什么？前者接收字节流对象，后者接收的是字符串