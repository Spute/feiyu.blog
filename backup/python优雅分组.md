- 以前的我处理分组
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

- 知道setdefault的我处理分组
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