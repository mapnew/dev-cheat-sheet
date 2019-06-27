## 字典和集合 dictionary and set

**字典**：一系列由键值配对组成的元素的集合

> 在python 3.7开始，字典被确定为有序，成为语言特性。在3.6的版本中只是作为实现的细节，没有作为正式的语言特性。在3.6之前是无序的，长度大小可变，元素可以删减和改变。

**集合**：一系列无序的、唯一的元素的组合。集合相比字典没有键和值的配对。

```python
# 字典
d1 = {'name': 'jason', 'age': 20, 'gender': 'male'}
d2 = dict({'name': 'jason', 'age': 20, 'gender': 'male'})
d3 = dict([('name', 'jason'), ('age', 20), ('gender', 'male')])
d4 = dict(name='jason', age=20, gender='male') 
d1 == d2 == d3 ==d4
True

# 集合
s1 = {1, 2, 3}
s2 = set([1, 2, 3])
s1 == s2
True
```

- 创建字典、集合

  ```python
  # 字典
  >>> dic2 = dict({'city': 'shenzhen', 'university': 'shen da'})
  >>> dic3 = dict([('city', 'shenzhen'), ('university', 'shen da')])
  >>> dic4 = dict(city = 'shenzhen', university='shen da')
  >>> dic1 = {'city': 'shenzhen', 'university': 'shen da'}
  >>> dic2
  {'city': 'shenzhen', 'university': 'shen da'}
  >>> dic3
  {'city': 'shenzhen', 'university': 'shen da'}
  >>> dic4
  {'university': 'shen da', 'city': 'shenzhen'}
  >>> dic1 == dic2 == dic3 == dic4
  True
  
  # 集合
  >>> s1 = {1, 2, 'hello'}
  >>> s2 = set([1, 2, 'hello'])
  >>> s1
  {'hello', 1, 2}
  >>> s2
  {'hello', 1, 2}
  >>> s1 == s2
  True
  ```

  

- 元素类型：对于字典和集合，其键和值均可以是混合类型的。

  ```python
  s = {1, 'hello', 5.0}
  ```

- 访问字典元素

  - 以键为索引，返回值。如果不存在，抛出异常。

  ```python
  >>> dic1
  {'city': 'shenzhen', 'university': 'shen da'}
  >>> dic1['university']
  'shen da'
  >>> dic1['location']
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  KeyError: 'location'
  ```

  - 调用get(key, default)来访问，如果键不存在，可以设置返回默认值。

  ```python
  >>> dic1.get('city')
  'shenzhen'
  >>> dic1.get('location')
  >>> dic1.get('location',null)
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  NameError: name 'null' is not defined
  >>> dic1.get('location', 'null')
  'null'
  ```

- 访问集合元素

  > 集合不支持索引：因为集合本质是一个哈希表，和列表不一样，所以无法用index来访问。

  ```python
  >>> s
  {'hello', 1, 5.0}
  >>> s[0]
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: 'set' object does not support indexing
  ```

  要判断一个元素是否在集合/字典中，可以用 value in dictionary/set 来判断

  ```python
  >>> s
  {'hello', 1, 5.0}
  >>> 1 in s
  True
  >>> 5.0 in s
  True
  >>> 3 in s
  False
  >>> 'hello' in s
  True
  
  >>> dic1
  {'city': 'shenzhen', 'university': 'shen da'}
  >>> 'city' in dic1
  True
  >>> 'shenzhen' in dic1
  False
  >>> 'location' in dic1
  False
  ```

- 增加、删除、更新字典/集合的元素

  ```python
  # 字典
  >>> dic1
  {'city': 'shenzhen', 'university': 'shen da'}
  >>> dic1['type'] = 'comprehensive' # 添加
  >>> dic1
  {'city': 'shenzhen', 'university': 'shen da', 'type': 'comprehensive'}
  >>> dic1['university'] = 'Shenzhen University' # 更新
  >>> dic1
  {'city': 'shenzhen', 'university': 'Shenzhen University', 'type': 'comprehensive'}
  >>> dic1.pop('type') # 删除
  'comprehensive'
  >>> dic1
  {'city': 'shenzhen', 'university': 'Shenzhen University'}
  
  # 集合
  >>> s
  {'hello', 1, 5.0}
  >>> s.add(3) # 添加
  >>> s
  {'hello', 1, 3, 5.0}
  >>> s.remove(1) # 删除
  >>> s
  {'hello', 3, 5.0}
  >>> s.pop() # 删除集合中最后一个元素
  'hello'
  >>> s
  {3, 5.0}
  >>> s.add(7)
  >>> s
  {3, 5.0, 7}
  >>> s.pop()
  3
  >>> s
  {5.0, 7}
  >>> s.pop(7)
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: pop() takes no arguments (1 given)
  
  ```

  > 因为集合本身是无序的，而集合的pop()操作是删除集合中最后一个元素，所以我们无法知道会删除哪一个元素，因此这个操作谨慎使用

- 字典和集合的排序

  > 实际场景中常需要排序，根据键、值进行升序或降序的排序
  >
  > 降序是？

  ```python
  # 字典
  >>> d = {'c': 1, 'b': 3, 'a':2}
  >>> d_sorted_by_key = sorted(d.items(), key=lambda x: x[0]) # 根据字典键的升序排序
  >>> d_sorted_by_value = sorted(d.items(), key=lambda x: x[1]) # 根据字典值的升序排序
  >>> d_sorted_by_key
  [('a', 2), ('b', 3), ('c', 1)]
  >>> d_sorted_by_value
  [('c', 1), ('a', 2), ('b', 3)]
  
  # 集合
  >>> s = (2, 1, 4, 2.3)
  >>> sorted(s)
  [1, 2, 2.3, 4]
  >>> s
  (2, 1, 4, 2.3)
  ```

- 字典和集合的性能

  ```python
  def find_product_price(products, product_id):
      for id, price in products:
          if id == product_id:
              return price
      return None 
       
  products = [
      (143121312, 100), 
      (432314553, 30),
      (32421912367, 150) 
  ]
  
  print('The price of product 432314553 is {}'.format(find_product_price(products, 432314553)))
  
  # 输出
  The price of product 432314553 is 30
  
  ```

  

  