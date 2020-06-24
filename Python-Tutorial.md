# 第一个Python程序

## 一、使用终端

退出终端（cmd、shell）的 Python 交互环境：`exit()`

终端运行 Python 程序：`python fileName.py`

## 二、使用VSCode

Windows 系统在 VSCode 运行 Python 程序，中文输出乱码，但终端输出正常。解决方案：新建系统环境变量 `PYTHONIOENCODING`，变量值为 `UTF-8`

## 三、直接运行Python文件

在 Windows 中不能像 `.exe` 文件那样运行 `.py` 文件，但在 Mac 和 Linux 上是可以的，方法是在 `.py` 文件的第一行加上一个特殊的注释：

```python
#!/usr/bin/env python3
print('hello, world')
```

然后，通过命令给 `hello.py` 执行权限：

```shell
$ chmod a+x hello.py
```

## 四、输入输出

`input()` 函数接收一整行的输入，并返回字符串类型。

字符串可以使用单引号括起来，也可以使用双引号括起来。

`print() `函数也可以接受多个字符串，用逗号 `,` 隔开。`print()`会依次打印每个字符串，遇到逗号 `,` 会输出一个空格。

# Python基础

以 `# `开头的语句是注释。

没有规定缩进是几个空格还是 Tab。按照约定俗成的惯例，应该始终坚持使用 4 个空格的缩进。

Python 程序是大小写敏感的。

## 一、数据类型和变量

### 1. 字符串

如果字符串里面有很多字符都需要转义，就需要加很多 `\`，为了简化，Python 还允许用 `r''` 表示 `''` 内部的字符串默认不转义。

```python
>>> print('\\\t\\')
\       \
>>> print(r'\\\t\\')
\\\t\\
```

如果字符串内部有很多换行，用 `\n` 写在一行里不好阅读，为了简化，Python 允许用 `'''...'''` 的格式表示多行内容。注意：下面代码是在交互式命令行内输入，`... `是提示符，不是代码的一部分。

```python
>>> print('''line1
... line2
... line3''')
line1
line2
line3
```

### 2. 布尔值

布尔值只有 `True`、`False `两种值。布尔值可以用 `and`、`or` 和 `not` 运算。

### 3. 空值

空值是 Python 里一个特殊的值，用 `None` 表示。`None` 不能理解为 `0`，因为 `0` 是有意义的，而 `None` 是一个特殊的空值。

### 4. 整数

Python的整数没有大小限制，而某些语言的整数根据其存储长度是有大小限制的，例如 Java 对 32 位整数的范围限制在 `-2147483648`-`2147483647`。

Python 的浮点数也没有大小限制，但是超出一定范围就直接表示为 `inf`（无限大）。

### 5. 变量

无需声明变量类型，同一个变量可以反复赋值，而且可以是不同类型的变量。

### 6. 常量

在 Python 中，通常用全部大写的变量名表示常量：`PI = 3.14159265359`

但事实上 `PI` 仍然是一个变量，Python 根本没有任何机制保证 `PI` 不会被改变，所以，用全部大写的变量名表示常量只是一个习惯上的用法，如果你一定要改变变量 `PI` 的值，也没人能拦住你。

在Python中，有两种除法：

- 除法 `/`：计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数；
- 地板除 `//`，两个整数的除法仍然是整数，取除法结果的整数部分。

## 二、字符串和编码

### 1. Unicode与UTF-8

简单来说：

- Unicode 是「字符集」

- UTF-8 是「编码规则」

其中：

- 字符集：为每一个「字符」分配一个唯一的 ID（学名为码位 / 码点 / Code Point）
- 编码规则：将「码位」转换为字节序列的规则（编码/解码 可以理解为 加密/解密 的过程）

各自使用范围：

- 在计算机内存中，统一使用 Unicode 编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。

### 2. Python字符串

在最新的 Python 3 版本中，字符串是以 Unicode 编码的，也就是说，Python 的字符串支持多语言。

对于单个字符的编码，Python 提供了 `ord()` 函数获取字符的整数表示，`chr() `函数把编码转换为对应的字符：

```python 
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

如果知道字符的整数编码，还可以用十六进制这么写 `str`：

```python
>>> '\u4e2d\u6587'
'中文'
```

由于 Python 的字符串类型是 `str`，在内存中以 Unicode 表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把 `str` 变为以字节为单位的 `bytes`。

Python 对 `bytes` 类型的数据用带 `b` 前缀的单引号或双引号表示：

```python
x = b'ABC'
```

要注意区分 `'ABC'` 和 `b'ABC'`，前者是 `str`，后者虽然内容显示得和前者一样，但 `bytes` 的每个字符都只占用一个字节。

以 Unicode 表示的 `str` 通过 `encode()` 方法可以编码为指定的 `bytes`，例如：

```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

纯英文的 `str` 可以用 `ASCII` 编码为 `bytes`，内容是一样的，含有中文的 `str` 可以用 `UTF-8` 编码为 `bytes`。含有中文的 `str` 无法用 `ASCII` 编码，因为中文编码的范围超过了 `ASCII` 编码的范围，Python 会报错。

反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是 `bytes`。要把 `bytes` 变为 `str`，就需要用 `decode()` 方法：

```python
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

要计算 `str` 包含多少个字符，可以用 `len()` 函数：

```python
>>> len('ABC')
3
>>> len('中文')
2
```

`len() `函数计算的是 `str` 的字符数，如果换成 `bytes`，`len() `函数就计算字节数：

```python
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```

由于 Python 源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为 UTF-8 编码。当 Python 解释器读取源代码时，为了让它按 UTF-8 编码读取，我们通常在文件开头写上这两行：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

第一行注释是为了告诉 Linux/OS X 系统，这是一个 Python 可执行程序，Windows 系统会忽略这个注释；

第二行注释是为了告诉 Python 解释器，按照 UTF-8 编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

### 3. Python格式化

#### 占位符

在 Python 中，采用的格式化方式和 C 语言是一致的，用 `%` 实现，举例如下：

```python
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```

如果只有一个格式化占位符 `%?`，括号可以省略。

| 占位符 |   替换内容   |
| :----: | :----------: |
|   %d   |     整数     |
|   %f   |    浮点数    |
|   %s   |    字符串    |
|   %x   | 十六进制整数 |

有些时候，字符串里面的 `%` 是一个普通字符怎么办？这个时候就需要转义，用 `%%` 来表示一个 `%`。

#### format()

`format() `方法会用传入的参数依次替换字符串内的占位符 `{0}`、`{1}`……，不过这种方式写起来比 `%` 要麻烦得多：

```python
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```

## 三、使用list和tuple

### list

list 是一种有序的集合，可以随时添加和删除其中的元素。使用 `[]` 表示 list，支持索引访问，当索引超出了范围时，Python 会报一个 `IndexError` 错误；用 `len()` 函数可以获得 list 元素的个数。

如果要取最后一个元素，除了计算索引位置外，还可以用 `-1` 做索引，直接获取最后一个元素。以此类推，可以获取倒数第 2 个、倒数第 3 个......需要注意的是，使用 `-n` 形式获取倒数第 n 个元素也可能会产生数组越界。

```python
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
>>> len(classmates)
3
>>> classmates[0]
'Michael'
>>> classmates[-1]
'Tracy'
```

要删除 list 末尾的元素，用 `pop()` 方法，该方法会返回删除的元素。要删除指定位置的元素，用 `pop(i)` 方法，其中 `i` 是索引位置。

list 里面的元素的数据类型也可以不同，比如：

```python
>>> L = ['Apple', 123, True]
```

list 元素也可以是另一个 list，比如：

```python
>>> s = ['python', 'java', ['asp', 'php'], 'scheme']
>>> len(s)
4
```

要从 `s` 中获取 "php"，可以写成 `s[2][1]`。

### tuple

元组 tuple 使用 `()` 表示，在定义的时候，tuple的元素就必须被确定下来，一旦初始化就不能修改。tuple 所谓的“不变”是说，tuple 的每个元素，指向永远不变。如果 tuple 的某一个元素指向的是 list，那么 list 中的元素是可以变的。

tuple 也可以像 list 那样使用索引访问，但不能修改。

如果要定义一个空的 tuple，可以写成 `()`：

```python
>>> t = ()
>>> t
()
```

只有 1 个元素的 tuple 定义时必须加一个逗号 `,`，来消除歧义，否则会被视为整数 1：

```python
>>> t = (1,)
>>> t
(1,)
```

## 四、条件判断

`if `语句的完整形式就是：

```python
if <condition1>:
    <execute1>
elif <condition2>:
    <execute2>
elif <condition3>:
    <execute3>
else:
    <execute4>
```

其中的 `<condition>` 也可以是数值、字符串、list、tuple等，其值非空非零，就判断为 `True`，否则为 `False`。

## 五、循环

### for in循环

`for...in` 循环，依次把 list 或 tuple 中的每个元素迭代出来，看例子：

```python
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

`range(n)` 函数生成一个 list，其值为 `[0, 1, ..., n-1]`。

### while循环

使用方式：

```python
while condition:
    execute something
```

## 六、使用dict和set

### dict

dict 全称 dictionary，在其他语言中也称为 map，使用键-值（key-value）存储，具有极快的查找速度。

dict 使用 `{}` 表示，用 Python 写一个 dict 如下：

```python
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
```

要避免 key 不存在的错误，有两种办法：一是通过 `in` 判断 key 是否存在；二是通过 dict 提供的 `get()` 方法，如果 key 不存在，可以返回 `None`，或者自己指定的 value。

```python
>>> 'Thomas' in d
False
>>> d.get('Thomas')
>>> d.get('Thomas', -1)
-1
```

注意：返回 `None` 的时候 Python 的交互环境不显示结果。

要删除一个 key，用 `pop(key)` 方法，对应的 value 会从 dict 中删除并返回。

正确使用 dict 需要牢记的第一条就是 dict 的 key 必须是**不可变对象**。在 Python 中，字符串、整数等都是不可变的，因此，可以放心地作为 key；而 list 是可变的，就不能作为 key。

