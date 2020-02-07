# 第 6 条：在单次切片操作中，不要同时指定 start、end 和 stride

Python 提供了 `somelist[start:end:stride]`写法，以实现步进式切割。

问题在于采用`stride`方式进行切片时，经常会出现不符合预期的结果。

Python 中常见的技巧，将序列反转`somelist[::-1]`。

这种技巧对字节串和 ASCII 字符有用，但是对已经编码成 UTF-8 字节串的 Unicode 字符来说，则不奏效。

```python
w = '谢谢'
x = w.encode('utf-8')
y = x[::-1]
z = y.decode('utf-8')
>>>
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xa2 in position 0: invalid start byte
```

切割列表时，如果指定了`stride`，那么代码可能会变得相当费解。在一对中括号中写 3 数字显得拥挤，从而导致代码难以阅读。这种写法使得`start`和`end`索引的含义变得模糊，尤其当`stride`为负值时。

1. 为解决这种问题，我们不应该把`stride`与`start`和`end`写在一起。
2. 如果一定要使用`stride`，那尽量采用正值，同时省略`start`和`end`索引。
3. 如果一定要配合`start`或`end`索引来使用`stride`，那么请先做步进式切片，把切割结果赋给某个变量，然后再做二次切割。（也可以先做范围切割，再做步进切割）

```python
a = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
b = a[::2] # ['a', 'c', 'e', 'g']
c = b[1:-1] # ['c', 'e']
```

以上先做步进切割，再做范围切割的办法，会多产生一份原数据的浅拷贝。

浅拷贝：

- 对于 **不可** 变类型`number`、`string`、`tuple`，浅复制仅仅是地址指向，不会开辟新空间。
- 对于 **可** 变类型`list`、`dictionary`、`set`，浅复制会开辟新的空间地址（仅仅是最顶层开辟了新的空间，里层的元素地址还是一样的），进行浅拷贝
- 浅拷贝后，改变原始对象中为可变类型的元素的值，会同时影响拷贝对象的；改变原始对象中为不可变类型的元素的值，只有原始类型受影响。（操作拷贝对象对原始对象的也是同理）

如果程序对执行时间或内存用量的要求非常严格，考虑使用`itertools`模块中的`islide`方法，该方法不允许为`start`、`end`或`stride`指定负值。（参见[第46条]()）

