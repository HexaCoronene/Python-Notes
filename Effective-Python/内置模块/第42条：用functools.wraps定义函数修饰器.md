# 第 42 条：用 functools.wraps 定义函数修饰器

## 例如，我们要打印某个函数受到调用时所接受的参数以及该函数的返回值。

```python
def trace(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        print("{}({}, {}) -> {}".format(func.__name__, args, kwargs, result))
        return result

    return wrapper


@trace
def fibonacci(n):
    """Return the n-th Fibonacci number"""
    if n in (0, 1):
        return n
    return fibonacci(n - 2) + fibonacci(n - 1)


if __name__ == "__main__":
    fibonacci(3)
    print(fibonacci)

```

输出结果如下：

```python
fibonacci((1,), {}) -> 1
fibonacci((0,), {}) -> 0
fibonacci((1,), {}) -> 1
fibonacci((2,), {}) -> 1
fibonacci((3,), {}) -> 2
<function trace.<locals>.wrapper at 0x0000022CE07A43A8>
```

使用 @ 符号来修饰函数，其效果就等于先以该函数为参数，调用修饰器，然后把修饰器所返回的结果，赋给同一个作用域与函数同名的变量。

```python
fibonacci = trace(fibonacci)
```

你问我为什么要定义两层函数？

因为一层用来传入函数本身，一层用来传入函数接受的参数啊

修饰器所返回的值，其名称会和原来不同

这个问题，可以用内置的 `functools` 模块中名为 `wraps` 的辅助函数来解决

`wraps` 本身也是个修饰器，它可以帮助开发者编写其他修饰器

```python
import functools

def trace(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        # ...
        return result

    return wrapper
```

此时可以看到

```python
<function fibonacci at 0x000001CEF96F43A8>
```

但是他为什么要这么写？？？

```python
@functools.wraps(func)
```

我横竖睡不着，从代码缝里看出两个字：

**源码**

```python
WRAPPER_ASSIGNMENTS = ('__module__', '__name__', '__qualname__', '__doc__',
                       '__annotations__')
WRAPPER_UPDATES = ('__dict__',)
def update_wrapper(wrapper,
                   wrapped,
                   assigned = WRAPPER_ASSIGNMENTS,
                   updated = WRAPPER_UPDATES):
    """Update a wrapper function to look like the wrapped function

       wrapper is the function to be updated
       wrapped is the original function
       assigned is a tuple naming the attributes assigned directly
       from the wrapped function to the wrapper function (defaults to
       functools.WRAPPER_ASSIGNMENTS)
       updated is a tuple naming the attributes of the wrapper that
       are updated with the corresponding attribute from the wrapped
       function (defaults to functools.WRAPPER_UPDATES)
    """
    for attr in assigned:
        try:
            value = getattr(wrapped, attr)
        except AttributeError:
            pass
        else:
            setattr(wrapper, attr, value)
    for attr in updated:
        getattr(wrapper, attr).update(getattr(wrapped, attr, {}))
    # Issue #17482: set __wrapped__ last so we don't inadvertently copy it
    # from the wrapped function when updating __dict__
    wrapper.__wrapped__ = wrapped
    # Return the wrapper so this can be used as a decorator via partial()
    return wrapper

def wraps(wrapped,
          assigned = WRAPPER_ASSIGNMENTS,
          updated = WRAPPER_UPDATES):
    """Decorator factory to apply update_wrapper() to a wrapper function

       Returns a decorator that invokes update_wrapper() with the decorated
       function as the wrapper argument and the arguments to wraps() as the
       remaining arguments. Default arguments are as for update_wrapper().
       This is a convenience function to simplify applying partial() to
       update_wrapper().
    """
    return partial(update_wrapper, wrapped=wrapped,
                   assigned=assigned, updated=updated)
```

线索指向了偏函数（禁止套娃）

但是可知，`functools.wraps(func)` 返回一个（函数？）

`partial` 是一个定义了 `__call__` 方法的类

```python
def trace(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print("Now in function wrapper: ", wrapper)
        result = func(*args, **kwargs)
        print("{}({}, {}) -> {}".format(func.__name__, args, kwargs, result))
        return result
	print("Now in function trace: ", wrapper)
	return wrapper

@trace
def fibonacci(n):
    # ...

if __name__ == "__main__":
    fibonacci(3)
    print("Now in __main__: ", fibonacci)
    
```

总之暂时不要搞这么复杂，先看上面这样写的结果

```python
Now in function trace:  <function fibonacci at 0x000001FBC1E743A8>
Now in function wrapper:  <function fibonacci at 0x000001FBC1E743A8>
Now in function wrapper:  <function fibonacci at 0x000001FBC1E743A8>
fibonacci((1,), {}) -> 1
Now in function wrapper:  <function fibonacci at 0x000001FBC1E743A8>
Now in function wrapper:  <function fibonacci at 0x000001FBC1E743A8>
fibonacci((0,), {}) -> 0
Now in function wrapper:  <function fibonacci at 0x000001FBC1E743A8>
fibonacci((1,), {}) -> 1
fibonacci((2,), {}) -> 1
fibonacci((3,), {}) -> 2
Now in __main__:  <function fibonacci at 0x000001FBC1E743A8>
```

是否可以这么理解：`trace` 仅执行一遍，用于替换 `fibonacci` 的行为；`@functools.wraps` 在 `wrapper` 执行前已执行，此时 `wrapper` 的签名已被替换；再然后就执行 `wrapper` 代码，只是 `func` 运行的是 `fibonacci`

