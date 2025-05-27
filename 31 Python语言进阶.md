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

![image](https://github.com/user-attachments/assets/22fb5081-d0d5-41fe-819f-f72abaf5ae78)

`Counter`：`dict`的子类，键是元素，值是元素的计数，它的`most_common()`方法可以帮助我们获取出现频率最高的元素。

```python
"""
找出序列中出现次数最多的元素
"""
from collections import Counter

words = [
    'look', 'into', 'my', 'eyes', 'look', 'into', 'my', 'eyes',
    'the', 'eyes', 'the', 'eyes', 'the', 'eyes', 'not', 'around',
    'the', 'eyes', "don't", 'look', 'around', 'the', 'eyes',
    'look', 'into', 'my', 'eyes', "you're", 'under'
]
counter = Counter(words)
print(counter.most_common(3))
```

# 数据结构和算法

评价算法的好坏：渐近时间复杂度和渐近空间复杂度。（渐近时间复杂度的大O标记）

几个常见算法：

1. 排序算法（选择、冒泡和归并）和查找算法（顺序和折半）

**选择排序**：每次从待排序的数据中选择最小（或最大）的元素，放到已排序序列的末尾，直到全部数据排序完成。时间复杂度 O^2

`lambda`匿名函数，语法格式是`lambda arguments: expression`

```python
def select_sort(items, comp = lambda x, y: x > y): #函数作为参数传入
    for i in range(len(items)-1):
        min_index = i
        for j in range(i+1, len(items)): # 找到i后最小的位置
            if comp(items[i], items[j]):
                min_index = j
        items[i], items[min_index] = items[min_index], items[i]
    return items
```

**冒泡排序**：重复地遍历待排序的列表，比较相邻的元素并交换它们的位置来实现排序。该算法的名称来源于较小的元素会像"气泡"一样逐渐"浮"到列表的顶端。实际在每一轮遍历中，由于比较前后两元素的大小，最大的元素会逐步冒泡到列表的最后

```python
def bubble_sort(items, comp = lambda x, y: x > y):
    for i in range(len(items)):
        sort = False
        for j in range(len(items)-1-i):
            if comp(items[j], items[j+1]):
                items[j], items[j+1] = items[j+1], items[j]
                sort = True
        if not sort:
            break
    return items
```

**双向冒泡排序**：冒泡排序的一种变形。该算法与冒泡排序的不同处在于排序时是以双向在序列中进行排序，即先从左往右排序，那么下一轮就从右往左，循环往复。

```python
def multi_bubble_sort(items, comp = lambda x, y: x > y):
    for i in range(len(items):
        # 先正向
        sort = False
        for j in range(len(items)-1-i):
            if comp(items[j], items[j+1]):
                items[j], items[j+1] = items[j+1], items[j]
                sort = True
        # 如果没排完，反向
        if sort:
            sort = False
            for k in range(len(items-2-i), i, -1)
                if comp(items[k-1], items[k]):
                    items[k-1], items[k] = items[k], items[k-1]
                    sort = True
        if not sort:
            break
    return items

```

2. 其它常用算法：
**穷举法** - 又称为暴力破解法，对所有的可能性进行验证，直到找到正确答案。

举例：百钱百鸡。公鸡5元一只 母鸡3元一只 小鸡1元三只，用100元买100只鸡 问公鸡/母鸡/小鸡各多少只

```python
for x in range(20):
    for y in range(33):
        z = 100 - x - y # 小鸡的只数
        if 5*x + 3*y + z//3 == 100 and z%3 == 0:
            print(x, y, z)
``` 
 
贪婪法 - 在对问题求解时，总是做出在当前看来最好的选择，不追求最优解，快速找到满意解。

举例：假设小偷有一个背包，最多能装20公斤赃物，他闯入一户人家，发现如下表所示的物品。很显然，他不能把所有物品都装进背包，所以必须确定拿走哪些物品，留下哪些物品。

  |  名称  | 价格（美元） | 重量（kg） |
  | :----: | :----------: | :--------: |
  |  电脑  |     200      |     20     |
  | 收音机 |      20      |     4      |
  |   钟   |     175      |     10     |
  |  花瓶  |      50      |     2      |
  |   书   |      10      |     1      |
  |  油画  |      90      |     9      |

```Python
class Thing(object):
      """物品"""
  
      def __init__(self, name, price, weight):
          self.name = name
          self.price = price
          self.weight = weight
  
      @property
      def value(self):
          """价格重量比"""
          return self.price / self.weight
  
  
  def input_thing():
      """输入物品信息"""
      name_str, price_str, weight_str = input().split()
      return name_str, int(price_str), int(weight_str)
  
  
  def main():
      """主函数"""
      max_weight, num_of_things = map(int, input().split())
      all_things = []
      for _ in range(num_of_things):
          all_things.append(Thing(*input_thing()))
      all_things.sort(key=lambda x: x.value, reverse=True)
      total_weight = 0
      total_price = 0
      for thing in all_things:
          if total_weight + thing.weight <= max_weight:
              print(f'小偷拿走了{thing.name}')
              total_weight += thing.weight
              total_price += thing.price
      print(f'总价值: {total_price}美元')
  
  
  if __name__ == '__main__':
      main()
```

分治法 - 把一个复杂的问题分成两个或更多的相同或相似的子问题，再把子问题分成更小的子问题，直到可以直接求解的程度，最后将子问题的解进行合并得到原问题的解。

回溯法 - 回溯法又称为试探法，按选优条件向前搜索，当搜索到某一步发现原先选择并不优或达不到目标时，就退回一步重新选择。

动态规划 - 基本思想也是将待求解问题分解成若干个子问题，先求解并保存这些子问题的解，避免产生大量的重复运算。
        
## 函数的使用方式

 - 将函数视为“一等公民”
    - 函数可以赋值给变量
    - 函数可以作为函数的参数
    - 函数可以作为函数的返回值
  
- 高阶函数的用法
    - 例如`filter` `map`函数，注意两个函数都是返回迭代器
  
```Python
# map(function, iterable, ...)
# filter(function, iterable)
items1 = list(map(lambda x: x**2, filter(lambda x: x%2, range(1, 10)))
items2 = [x**2 for x in range(1, 10) if x%2]
```

- 闭包和作用域问题
    - `global`:声明或定义全局变量（要么直接使用现有的全局作用域的变量，要么定义一个变量放到全局作用域）。全局变量在程序的任何一个地方都可以访问这个变量，不管是在函数内部还是函数外部。
    - `nonlocal`：声明使用嵌套作用域的变量（`nonlocal`关键字只能用于嵌套函数中，并且外层函数中定义了相应的局部变量，否则会发生错误）。

## 并发编程
Python中实现并发编程的三种方案：多线程、多进程和异步I/O。并发编程的好处在于可以提升程序的执行效率以及改善用户体验；坏处在于并发的程序不容易开发和调试，同时对其他程序来说它并不友好。

- 多线程：Python中提供了`Thread`类并辅以`Lock`、`Condition`、`Event`、`Semaphore`和`Barrier`。Python中有GIL来防止多个线程同时执行本地字节码，这个锁对于CPython是必须的，因为CPython的内存管理并不是线程安全的，因为GIL的存在多线程并不能发挥CPU的多核特性。

注意，进程和线程的区别和联系？

进程 - 操作系统分配内存的基本单位 - 一个进程可以包含一个或多个线程

线程 - 操作系统分配CPU的基本单位

- 多进程：多进程可以有效的解决GIL的问题，实现多进程主要的类是Process，其他辅助的类跟threading模块中的类似，进程间共享数据可以使用管道、套接字等，在multiprocessing模块中有一个Queue类，它基于管道和锁机制提供了多个进程共享的队列。下面是官方文档上关于多进程和进程池的一个示例。

- 异步处理：从调度程序的任务队列中挑选任务，该调度程序以交叉的形式执行这些任务，我们并不能保证任务将以某种顺序去执行，因为执行顺序取决于队列中的一项任务是否愿意将 CPU 处理时间让位给另一项任务。异步任务通常通过多任务协作处理的方式来实现，由于执行时间和顺序的不确定，因此需要通过回调式编程或者future对象来获取任务执行的结果。Python 3 通过asyncio模块和await和async关键字（在 Python 3.7 中正式被列为关键字）来支持异步处理。




