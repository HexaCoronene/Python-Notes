# 第 3 条：了解 bytes、str 与 unicode 的区别

Python3 有两种表示字符序列的类型：bytes 和 str。

- bytes：实例包含原始 8 位值
- str：实例包含 Unicode 字符

要想把 Unicode 字符转换成二进制数据，必须使用 encode 方法。要想把二进制数据转换成 Unicode 字符，则必须使用 decode 方法。

**编写 Python 程序的时候，一定要把编码和解码操作放在界面最外围来做。**

**程序的核心部分应该使用 Unicode 字符类型，而且不要对字符编码做任何假设。**

Python 代码中经常会出现两种常见的使用情境：

- 开发者需要原始 8 位值，这些 8 位值表示以 UTF-8 格式（或其他编码形式）来编码的字符。
- 开发者需要操作没有特定编码形式的 Unicode 字符。

为解决问题，编写两个辅助函数：

- 接受 str 或 bytes，并总是返回 str 的方法：

```python
def to_str(bytes_or_str):
    if isinstance(bytes_or_str, bytes):
        value = bytes_or_str.decode('utf-8')
    else:
        value = bytes_or_str
    return value
```

- 接受 str 或 bytes，并总是返回 bytes 方法

```python
def to_bytes(bytes_or_str):
    if isinstance(bytes_or_str, str):
        value = bytes_or_str.encode('utf-8')
    else:
        value = bytes_or_str
    return value
```

Python3 中可能会出现一个问题。如果通过内置的 open 函数获取了文件句柄（file handle，其实是一种标识符或指针，也可以理解为文件描述符，用来指代开发者将要操作的文件。），那么该句柄会采用 UTF-8 编码格式来操作文件。

- 字符写入模式 'w'，打开模式 'r'

```python
with open(filename, 'w') as f:
    pass
```

- 二进制写入模式 'wb'，打开模式 'rb'

```python
with open(filename, 'wb') as f:
    pass
```

