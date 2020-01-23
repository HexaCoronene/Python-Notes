# 第 2 条：遵循 PEP8 风格指南

## **[PEP 8，Python Enhancement Proposal #8](http://www.python.org/dev/peps/pep-0008)**

## 请遵守如下规则(  !  )：

### 空白

Python 中的空白（whitespace）会影响代码的含义。Python程序员使用空白的时候尤其在意，因为它们还会影响代码的清晰程度。

- 使用空格（space）表示缩进，而不要用制表符（tab）。# 但是 tab 键用起来真的舒服啊（小声bb
- 和语法相关的每一层缩进都用 4 个空格来表示。
- 每行的字符数不应超过 79。
- 对于占据多行的长表达式来说，除了首行之外的其余各行都应该在通常的缩进级别上再加 4 个空格。
- 文件中的函数与类之间应该用两个空行隔开。
- 在同一个类中，各方法之间应该用一个空行隔开。
- 在使用下标来获取列表元素、调用函数或给关键字参数赋值的时候，不要在两边添加空格。
- 为变量赋值的时候，赋值符号的左侧和右侧应该各自写上一个空格，而且只写一个。

### 命名

PEP 8 提倡采用不同的命名风格来编写 Python 代码的各个部分，以便在阅读代码时可以根据这些名称看出它们在 Python 语言中的角色。

- 函数、变量及属性应该用小写字母来拼写，各单词间以下划线相连，例如，lowercase_underscore。# 我记得不遵守这条的情况也很多啊
- 受保护的实例属性，应该以单个下划线开头，例如，\_leading_underscore。
- 私有的实例属性，应该以两个下划线开头，例如，\_\_double\_leading_underscore。
- 类与异常，应该以每个单词首字母均大写的形式来命名，例如，CapitalizedWorld。
- 模块级别的常量，应该全部采用大写字母来拼写，各单词之间以下划线相连，例如，ALL_CAPS。
- 类中的实例方法（instance method），应该把首个参数命名为 self，以表示该对象自身。
- 类方法（class method）的首个参数，应该命名为 cls，以表示该类自身。

### 表达式和语句

_The Zen of Python_ 中说：

> There should be one-- and preferably only one --obvious way to do it.

- 采用内联形式的否定词，而不要把否定词放在整个表达式的前面，例如，应该写 if a is not b 而不是 if not a is b。
- 不要通过检测长度的方法（如 if len(somelist) == 0）来判断 somelist 是否为 \[] 或 \'' 等空值。而是应该采用 if not somelist 这种写法来判断，它会假定：空值将自动评估为 False。
- 检测 somelist 是否为 [1] 或 'hi' 等非空值时，也应如此，if somelist 语句默认会把非空的值判断为 True。
- 不要编写单行的 if 语句、for 循环、while 循环及 except 复合语句，而是应该把这些语句分成多行来写，以示清晰。
- import 语句应该总是放在文件开头。
- 引入模块的时候，总是应该使用绝对名称，而不是根据当前模块的路径来使用相对名称。例如，引入 bar 包中的 foo 模块时，应该完整地写出 from bar import foo，而不应该简写为 import foo。（？）
- 如果一定要以相对名称来编写 import 语句，那就采用明确的写法，from xxx import foo。
- 文件中的那些 import 语句应该按顺序划分成三个部分，分别表示标准库模块、第三方模块以及自用模块。在每一部分之中，各 import 语句应该按模块的字母顺序来排列。