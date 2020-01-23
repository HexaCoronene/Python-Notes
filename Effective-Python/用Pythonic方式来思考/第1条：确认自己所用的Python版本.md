# 第 1 条：确认自己所用的 Python 版本

---

## 要点

1. 有两个版本的 Python 处于活跃状态，Python 2 与 Python 3。
2. 有很多种流行的Python运行时环境，例如，CPython、Jython、IronPython 以及 PyPy 等。
3. 在操作系统中运行 Python 时，请确保该 Python 的版本与你想使用的 Python 版本相符。
4. 在开发后续项目时，优先使用 Python 3。Python 2 EOL。

## Windows

当前所有代码执行于Windows平台，IDE 为 PyCharm Community，文本编辑器为 VS Code，Python 版本 3.7.5

在命令行输入py或python，会调用Python解释器

```
C:\Users\***>python
Python 3.7.5 (tags/v3.7.5:5c02a39a0b, Oct 15 2019, 00:11:34) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.

C:\Users\***>py
Python 3.7.5 (tags/v3.7.5:5c02a39a0b, Oct 15 2019, 00:11:34) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
```

按下 Ctrl+Z，并回车可退出解释器

_**不要输入python3，会打开Microsoft Store**_

:new_moon_with_face:

### 查询版本的两种方法

1. 命令行

```
C:\Users\***>python --version
Python 3.7.5
```

2. 运行时，在内置 sys 模块查询

```python
>>> import sys
>>> sys.version_info
sys.version_info(major=3, minor=7, micro=5, releaselevel='final', serial=0)
>>> sys.version
'3.7.5 (tags/v3.7.5:5c02a39a0b, Oct 15 2019, 00:11:34) [MSC v.1916 64 bit (AMD64)]'
```



先不要着急升级到 Python3.8，模块会有适配问题

比如 time 库的某函数被移除了