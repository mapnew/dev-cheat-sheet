# 列表和元组 list and tuple

## 1. 列表和元组的共同点

- 都是有序的集合
- 可以放置任意数据类型

>  语言对比：C/C++、Java都要求集合的数据类型一致，而python的列表和元组不要求。

```python
l = [1, 2, 'hello', 'world'] # 列表中同时含有 int 和 string 类型的元素
l
[1, 2, 'hello', 'world']

tup = ('jason', 22) # 元组中同时含有 int 和 string 类型的元素
tup
('jason', 22)
```



## 2. 列表和元组的区别

- 列表是动态的，可变的：长度大小不固定，可以随意增加、删除和修改元素

- 元组是静态的，不可变的：长度大小固定，无法增加、删除和修改元素

```python
  # 元组不支持对元素的修改
  >>> tup[2]='ad'
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: 'tuple' object does not support item assignment
  
  # 元组不支持对元素的增加,只能通过开辟新内存的方式来得到想要的元组
  >>> tup = tup + (2,)
  >>> tup
  (1, 2, 'we', 2)
  
  # 列表支持对元素的添加，但不是创建新的列表
  >>> l
  [3, 4, 'ad', 'he']
  >>> l.append(0)
  >>> l
  [3, 4, 'ad', 'he', 0]
```

- python的列表和元组支持负数索引，-1表示最后一个元素，以此类推

  > C/C++ 不支持负数索引

  ```python
  >>> l[-2]
  'he'
  >>> tup
  (1, 2, 'we', 2)
  >>> tup[-2]
  'we'
  ```

- python的列表和元组都支持切片操作，如 list[1:3]表示索引为从1到2的子列表

  ```python
  >>> l
  [3, 4, 'ad', 'he', 0]
  >>> l[1:3] # 返回索引为从1到2的子列表
  [4, 'ad']
  >>> l
  [3, 4, 'ad', 'he', 0]
  
  >>> tup
  (1, 2, 'we', 2)
  >>> tup[1:3]
  (2, 'we')
  >>> tup
  (1, 2, 'we', 2)
  ```

- python 的列表和元组都支持嵌套

  ```python
  >>> l = [[1, 2], [3, 4]]
  >>> l
  [[1, 2], [3, 4]]
  
  >>> tup = ((1, 2), (3, 4))
  >>> tup
  ((1, 2), (3, 4))
  ```

- python 的元组和列表可以相互转换, 调用tuple()转换为元组，调用list()转换为列表

  ```python
  >>> tup
  ((1, 2), (3, 4))
  >>> list(tup) # 调用list()转换为列表
  [(1, 2), (3, 4)]
  >>> tup
  ((1, 2), (3, 4))
  
  >>> l
  [[1, 2], [3, 4]]
  >>> tuple(l) # 调用tuple()转换为元组
  ([1, 2], [3, 4])
  ```

- python 的列表和元组常用的内置函数

  - `count(item) `表示统计列表 / 元组中 item 出现的次数
  - `index(item)` 表示返回列表中该item第一次出现的索引
  - `list.reverse()` 原地倒转列表
  - `list.sort() `地排序
  - `reversed()`、`sorted() `返回对列表/元组进行倒转或排序的新的列表/元组

  ```python
  >>> l = [8, 1, 2, 3, 7]
  >>> l
  [8, 1, 2, 3, 7]
  >>> l.append(3)
  >>> l
  [8, 1, 2, 3, 7, 3]
  >>> l.count(3)
  2
  >>> l.index(7)
  4
  >>> l.index(3)
  3
  >>> l.reverse()
  >>> l
  [3, 7, 3, 2, 1, 8]
  >>> l.sort()
  >>> l
  [1, 2, 3, 3, 7, 8]
  
  ```

- 列表和元组存储方式的差异

  - 列表over-allocating：为了减少每次增加和删减操作在空间分配上的开销，Python每次分配空间时都会额外多分配一些, 保证在增加和删减的时间复杂度均为O(1)
  - 元组的存储空间固定

  ```python
  >>> l = []
  >>> l.__sizeof__()
  40
  >>> l = [1]
  >>> l.__sizeof__()
  48
  >>> l.append(2)
  >>> l
  [1, 2]
  >>> l.__sizeof__()
  80
  >>> l.append(3)
  >>> l.__sizeof__() 
  80 // 由于之前分配了空间，所以加入元素 3，列表空间不变
  >>> l.append(4)
  >>> l.__sizeof__()
  80
  >>> l.append(5)
  >>> l.__sizeof__()
  80
  >>> l.append(6)
  >>> l.__sizeof__()
  112 // 加入元素 6 之后，列表的空间不足，所以又额外分配了可以存储 4 个元素的空间
  >>> l = []
  >>> l.__sizeof__() // 空列表的存储空间为 40 字节
  40
  >>> l.append(1)
  >>> l.__sizeof__()
  72 // 加入了元素 1 之后，列表为其分配了可以存储 4 个元素的空间 (72 - 40)/8 = 4
  ```

- 列表和元组的性能

  - 元组更为轻量，性能略优于列表。初始化一个相同元素的列表和元组分别所需的时间，元组比列表快了五倍左右。

    ```shell
    root@JD:~# python3 -m timeit 'x=(1,2,3,4,5,6)'
    100000000 loops, best of 3: 0.0124 usec per loop
    
    root@JD:~# python3 -m timeit 'x=[1,2,3,4,5,6]'
    10000000 loops, best of 3: 0.0672 usec per loop
    ```

- 列表和元组的使用场景

  - 如果存储的数据和数量不变，那么选用元组更合适
  - 如果存储的数据和数量是可变的，那么选用列表更合适

- tuple 源码： 

  - https://github.com/python/cpython/blob/3.7/Include/tupleobject.h
  - https://github.com/python/cpython/blob/master/Objects/tupleobject.c

- list 源码：

  - https://github.com/python/cpython/blob/3.7/Include/listobject.h
  - https://github.com/python/cpython/blob/master/Objects/listobject.c



思考题：

创建空列表，哪个效率高？

```
# 创建空列表
# option A
empty_list = list()

# option B
empty_list = []
```

区别主要在于list()是一个function call，Python的function call会创建stack，并且进行一系列参数检查的操作，比较expensive，反观[]是一个内置的C函数，可以直接被调用，因此效率高。

```
root@JD:~# python3 -m timeit 'empty_list = list()'
10000000 loops, best of 3: 0.0858 usec per loop
root@JD:~# python3 -m timeit 'empty_list = []'
10000000 loops, best of 3: 0.0233 usec per loop
```

使用timeit会自动关掉垃圾回收机制，让程序的运行更加独立，时间计算更加准确。



