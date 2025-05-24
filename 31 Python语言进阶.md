# Python语言进阶

## 重要知识点

**生成式（推导式）的用法**

生成式可以用来生成列表、字典、集合等
```python
prices = {
    'AAPL': 191.88,
    'GOOG': 1186.96,
    'IBM': 149.24,
    'ORCL': 48.44,
    'ACN': 166.89,
    'FB': 208.09,
    'SYMC': 21.29
}
prices1 = [key: value for key, value in prices if value > 100]
```

**嵌套的列表的坑**

```python
names = ['关羽', '张飞', '赵云', '马超', '黄忠']
courses = ['语文', '数学', '英语']
# 录入五个学生三门课程的成绩
# 错误 - 参考http://pythontutor.com/visualize.html#mode=edit
# scores = [[None] * len(courses)] * len(names)
scores = [[None] * len(courses) for _ in range(len(names))] # 生成嵌套列表，其中[None] * len(courses)是对[None]列表内容重复k次
for row, name in enumerate(names):
    for col, course in enumerate(courses):
        scores[row][col] = float(input(f'请输入{name}的{course}成绩: '))
        print(scores)
```
上面的`scores = [[None] * len(courses)] * len(names)`其实每个子列表指向的都是相同地址，当我们改变其中一个子列表时，其它子列表内容也会跟着改变。
也就是相当于复制这个子列表的引用 `len(names)` 次。所有子列表共享同一个内存地址。

正确的做法是使用列表推导式来确保每个子列表都是独立的。

**`heapq`模块（堆排序）** 

堆是一种数据结构，它是一颗完全二叉树。最小堆则是在堆的基础增加了新的规则，它的根结点的值是最小的，而且它的任意结点的父结点的值都小于或者等于其左右结点的值。
堆就是用数组表示的二叉树，分为大根堆和小根堆，大根堆为堆顶元素最大的堆，小根堆为堆顶元素最小的堆


![image](https://github.com/user-attachments/assets/4810f7ea-d96a-440e-b439-62eeda5a0030)

Python的`heapq`模块能够实现一个最小堆。

```python
"""
从列表中找出最大的或最小的N个元素
堆结构(大根堆/小根堆)
"""
import heapq

list1 = [34, 25, 12, 99, 87, 63, 58, 78, 88, 92]
list2 = [
    {'name': 'IBM', 'shares': 100, 'price': 91.1},
    {'name': 'AAPL', 'shares': 50, 'price': 543.22},
    {'name': 'FB', 'shares': 200, 'price': 21.09},
    {'name': 'HPQ', 'shares': 35, 'price': 31.75},
    {'name': 'YHOO', 'shares': 45, 'price': 16.35},
    {'name': 'ACME', 'shares': 75, 'price': 115.65}
]
print(heapq.nlargest(3, list1)) # 大根堆
print(heapq.nsmallest(3, list1))  # 小根堆
print(heapq.nlargest(2, list2, key=lambda x: x['price']))
print(heapq.nlargest(2, list2, key=lambda x: x['shares']))
```

**`itertools`模块**

`itertools`是Python的一个内置模块，它提供了一系列用于迭代的函数，可用于有效地创建迭代器，组合、操作和处理数据。

```python
"""
迭代工具模块
"""
import itertools

# 产生ABCD的全排列
itertools.permutations('ABCD')

# 产生ABCDE的五选三组合
itertools.combinations('ABCDE', 3)

# 产生ABCD和123的笛卡尔积
itertools.product('ABCD', '123')

# 产生ABC的无限迭代器，不断重复提供的元素
itertools.cycle(('A', 'B', 'C'))

# 产生无限迭代器 itertools.count(start, step)
# 从 5 开始，步长为 2，生成整数序列
for i in itertools.count(5, 2):
    if i > 20:
        break
    print(i, end=' ')
```

**`collections`模块**

Python内置的数据类型和方法，`collections`模块在这些内置类型的基础提供了额外的高性能数据类型，
比如基础的字典是不支持顺序的，`collections`模块的`OrderedDict`类构建的字典可以支持顺序，
`collections`模块的这些扩展的类用处非常大，熟练掌握该模块，可以大大简化Python代码。

`collections`有如下子类：




