# 前言

数据分组、聚合计数作为常规的数据操作，时常会在日常开发中碰到。寻找一种简明高效的实现方式很有必要。

# 优雅分组
## 普通实现

- 以前的我处理分组。if else实现。
```
groups = {}
numbers = [1, 2, 1, 3, 2, 4]
for num in numbers:
    k = num
    v = groups.get(k)
    if v:
        v.append(num)
    else:
        groups[k] = [num]
print(groups)
{1: [1, 1], 2: [2, 2], 3: [3], 4: [4]}
```

## 字典setdefault方法

```
groups = {}
numbers = [1, 2, 1, 3, 2, 4]
for num in numbers:
    # 如果key不存在，创建空列表并返回
    # 如果key存在，返回现有值
    groups.setdefault(num, []).append(num)
print(groups)
{1: [1, 1], 2: [2, 2], 3: [3], 4: [4]}


groups = {}
numbers = [1, 2, 1, 3, 2, 4]
# 等价于：
for num in numbers:
    if num not in groups:
        groups[num] = []
    groups[num].append(num)
print(groups)

{1: [1, 1], 2: [2, 2], 3: [3], 4: [4]}

```

## collections.defaultdict方法

defaultdict 内部通过 C 实现，访问不存在的键时效率更高。
```
from collections import defaultdict

# 数据列表
data = [
    {"name": "Alice", "age": 25, "city": "New York"},
    {"name": "Bob", "age": 30, "city": "Los Angeles"},
    {"name": "Charlie", "age": 25, "city": "New York"},
    {"name": "David", "age": 30, "city": "Chicago"},
]

# 创建 defaultdict，允许为键提供更复杂的默认值工厂函数，比如 int、list、set 等，甚至可以是自定义函数。
grouped_data = defaultdict(list)

# 按 age 分组
for item in data:
    grouped_data[item["age"]].append(item)

# 打印分组结果
for age, items in grouped_data.items():
    print(f"Age {age}:")
    for i in items:
        print(f"  {i}")

```
# 词频统计collections.Counter

可以用来统计可迭代对象中元素的出现次数。
```
from collections import Counter

# 要统计的文本
text = "apple banana apple orange banana apple apple orange"

# 将文本分割为单词列表
words = text.split()

# 使用 Counter 统计词频
word_counts = Counter(words)

# 输出统计结果
print("词频统计结果：")
for word, count in word_counts.items():
    print(f"{word}: {count}")


词频统计结果：
apple: 4
banana: 2
orange: 2

```
- 常用方法：
most_common(n)：返回出现频率最高的前 n 个元素。
update(iterable)：更新已有的计数。
subtract(iterable)：减少计数值。