# 第 7 条：用列表推导来取代 map 和 filter

Python 提供了精炼的列表推导（list comprehension）写法，可以根据一份列表来制作另外一份。

```python
a = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
squares = [x**2 for x in a]
# squares: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

对于简单的情况来说，列表推导比内置的 map 函数更清晰。如果要使用 map 函数，那就要创建 lambda 函数，会使代码看起来有些乱。

```python
squares = map(lambda x: x**2, a)
```

列表推导可以直接过滤原列表中的元素。

```python
even_squares = [x**2 for x in a if x % 2 == 0]
```

把内置的 filter 函数与 map 结合起来，也能达到同样的效果，但是代码会写得难懂。

```python
alt = map(lambda x: x**2, filter(lambda x: x % 2 == 0, a))
```

字典与集合也有类似的推导机制。

```python
# dic
chile_ranks = {'ghost': 1, 'habanero': 2, 'cayenne': 3}
rank_dict = {rank: name for name, rank in chile_ranks.items()}
# rank_dict: {1: 'ghost', 2: 'habanero', 3: 'cayenne'}

# set
chile_len_set = {len(name) for name in rank_dict.values()}
# chile_len_set: {8, 5, 7}
```

