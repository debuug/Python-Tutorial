# 一、第一个Python程序

## 1. 使用终端

退出终端（cmd、shell）的 Python 交互环境：`exit()`

终端运行 Python 程序：`python fileName.py`

## 2. 使用VSCode

Windows 系统在 VSCode 运行 Python 程序，中文输出乱码，但终端输出正常。解决方案：新建系统环境变量 `PYTHONIOENCODING`，变量值为 `UTF-8`

## 3. 直接运行Python文件

在 Windows 中不能像 `.exe` 文件那样运行 `.py` 文件，但在 Mac 和 Linux 上是可以的，方法是在 `.py` 文件的第一行加上一个特殊的注释：

```python
#!/usr/bin/env python3
print('hello, world')
```

然后，通过命令给 `hello.py` 执行权限：

```shell
$ chmod a+x hello.py
```

## 4. 输入输出

`input()` 函数接收一整行的输入，并返回字符串类型。

字符串可以使用单引号括起来，也可以使用双引号括起来。

`print() `函数也可以接受多个字符串，用逗号 `,` 隔开。`print()`会依次打印每个字符串，遇到逗号 `,` 会输出一个空格。

# 二、Python基础

以 `# `开头的语句是注释。

没有规定缩进是几个空格还是 Tab。按照约定俗成的惯例，应该始终坚持使用 4 个空格的缩进。

Python 程序是大小写敏感的。

## 1. 数据类型和变量

### 1.1 字符串

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

### 1.2 布尔值

布尔值只有 `True`、`False `两种值。布尔值可以用 `and`、`or` 和 `not` 运算。

### 1.3 空值

空值是 Python 里一个特殊的值，用 `None` 表示。`None` 不能理解为 `0`，因为 `0` 是有意义的，而 `None` 是一个特殊的空值。

### 1.4 整数

Python的整数没有大小限制，而某些语言的整数根据其存储长度是有大小限制的，例如 Java 对 32 位整数的范围限制在 `-2147483648`-`2147483647`。

Python 的浮点数也没有大小限制，但是超出一定范围就直接表示为 `inf`（无限大）。

### 1.5 变量

无需声明变量类型，同一个变量可以反复赋值，而且可以是不同类型的变量。

### 1.6 常量

在 Python 中，通常用全部大写的变量名表示常量：`PI = 3.14159265359`

但事实上 `PI` 仍然是一个变量，Python 根本没有任何机制保证 `PI` 不会被改变，所以，用全部大写的变量名表示常量只是一个习惯上的用法，如果你一定要改变变量 `PI` 的值，也没人能拦住你。

在Python中，有两种除法：

- 除法 `/`：计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数；
- 地板除 `//`，两个整数的除法仍然是整数，取除法结果的整数部分。

## 2 字符串和编码

### 2.1 Unicode与UTF-8

简单来说：

- Unicode 是「字符集」

- UTF-8 是「编码规则」

其中：

- 字符集：为每一个「字符」分配一个唯一的 ID（学名为码位 / 码点 / Code Point）
- 编码规则：将「码位」转换为字节序列的规则（编码/解码 可以理解为 加密/解密 的过程）

各自使用范围：

- 在计算机内存中，统一使用 Unicode 编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。

### 2.2 Python字符串

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

### 2.3 Python格式化

#### 2.3.1 占位符

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

#### 2.3.2 format()

`format() `方法会用传入的参数依次替换字符串内的占位符 `{0}`、`{1}`……，不过这种方式写起来比 `%` 要麻烦得多：

```python
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```

## 3 使用list和tuple

### 3.1 list

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

### 3.2 tuple

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

## 4 条件判断

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

## 5 循环

### 5.1 for in循环

`for...in` 循环，依次把 list 或 tuple 中的每个元素迭代出来，看例子：

```python
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

`range(n)` 函数生成一个 list，其值为 `[0, 1, ..., n-1]`。

### 5.2 while循环

使用方式：

```python
while condition:
    execute something
```

## 6 使用dict和set

### 6.1 dict

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

### 6.2 set

set 与 dict 的区别就是 set 中只存储 key，不存储 value。set 也用 `{}` 表示。

要创建一个 set，需要提供一个 list 作为输入集合：

```python
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
```

通过 `add(key)` 方法可以添加元素到 set 中，可以重复添加，但不会有效果。

通过 `remove(key)` 方法可以删除元素。

set 的原理和 dict 一样，所以，同样不可以放入可变对象。

# 三、函数

## 1 调用函数

对于内置函数，只需知道函数的名称和参数即可调用。

内置数据类型转换函数：`int()、float()、str()、bool()`

```python
>>> int('123')
123
>>> int(12.34)
12
>>> float('12.34')
12.34
>>> str(1.23)
'1.23'
>>> str(100)
'100'
>>> bool(1)
True
>>> bool('')
False
```

函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”：

```python
>>> a = abs # 变量a指向abs函数
>>> a(-1) # 所以也可以通过a调用abs函数
1
```

`hex() `函数把一个整数转换成十六进制表示的字符串。

## 2 定义函数

### 2.1 通用定义方式

定义函数方式如下：

```python
def funcName(param1,param2...):
    # do something
    return
```

如果函数体没有 `return `语句，函数执行完毕后也会返回结果，只是结果为 `None`。`return None` 可以简写为 `return`。

### 2.2 空函数

如果想定义一个什么事也不做的空函数，可以用 `pass` 语句：

```Python
def nop():
    pass	# pass 关键字的实际功能就是作为占位符
```

### 2.3 参数检查

在自定义函数中可通过内置函数 `isinstance()` 实现对参数类型检查的功能。例如，`my_abs` 函数只允许整数和浮点数类型的参数：

```Python
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```

### 2.4 返回多个值

Python 函数可通过 tuple 实现返回多个值的功能。在语法上，返回一个 tuple 可以省略括号，而多个变量可以同时接收一个 tuple，按位置赋给对应的值。所以，Python 的函数返回多值其实就是返回一个 tuple，但写起来更方便。

## 3 函数的参数

发表点个人意见：Python 定义函数的时候不声明参数类型真的太扯了，还是 C 系语言用着称心。

### 3.1 位置参数

普通的函数参数即为位置参数。例如，函数 `power(x, n)` 中的 x 和 n 均为位置参数。

### 3.2 默认参数

代码示例：

```python
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
```

其中参数 `n` 为默认参数，如果调用者没有给出 参数 `n` 的值，那么默认其值为 2。

设置默认参数时，有几点要注意：

- 必选参数在前，默认参数在后。否则 Python 的解释器会报错；

- 当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数。

对于存在多个默认参数的函数，先看代码：

```python
def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)
```

调用的时候，既可以按顺序提供默认参数，比如调用 `enroll('Bob', 'M', 7)`，意思是，除了`name`，`gender `这两个参数外，最后 1 个参数应用在参数 `age` 上，`city `参数由于没有提供，仍然使用默认值。

也可以不按顺序提供部分默认参数。当不按顺序提供部分默认参数时，需要把参数名写上。比如调用 `enroll('Adam', 'M', city='Tianjin')`，意思是，`city` 参数用传进去的值，其他默认参数继续使用默认值。

**定义默认参数要牢记一点：默认参数必须指向不变对象！**

原因请看代码：

```python
def add_end(L=[]):
    L.append('END')
    return L
    
>>> add_end()
['END']
>>> add_end()
['END', 'END']
```

按理说，每次调用 `add_end()` 都应该输出 `['END']`，默认参数是 `[]`，但是函数似乎每次都“记住了”上次添加了 `'END'` 后的 list。

解释：Python 函数在定义的时候，默认参数 `L` 的值就被计算出来了，即 `[]`，因为默认参数 `L` 也是一个变量，它指向对象 `[]`，每次调用该函数，如果改变了 `L` 的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的 `[]` 了。

要修改上面的例子，可以用 `None` 这个不变对象来实现：

```python
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```

### 3.3 可变参数

可变参数就是传入的参数个数是可变的。

**第一种实现方式**

将参数封装为 list 或 tuple。例如：

```python
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

调用方式：

```python
>>> calc([1, 2, 3])
14
>>> calc((1, 3, 5, 7))
84
```

**第二种实现方式**

定义可变参数和定义一个 list 或 tuple 参数相比，仅仅在参数前面加了一个 `*` 号。在函数内部，参数`numbers `接收到的是一个 tuple，因此，函数代码完全不变。

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

调用方式：

```python
>>> calc(1, 2)
5
>>> calc()
0
```

如果已经有一个 list 或者 tuple，要调用一个可变参数怎么办？Python 允许在 list 或 tuple 前面加一个 `*` 号，把 list 或 tuple 的元素变成可变参数传进去：

```python
>>> nums = [1, 2, 3]
>>> calc(*nums)
14
```

### 3.4 关键字参数

关键字参数允许传入 0 个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个 dict。示例：

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
```

其中，`kw` 为关键字参数。

在调用该函数时，可以只传入必选参数，也可以传入任意个数的关键字参数。例如：

```python
>>> person('Michael', 30)
name: Michael age: 30 other: {}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

关键字参数应用场景：试想你正在做一个用户注册的功能，除了用户名和年龄是必填项外，其他都是可选项，利用关键字参数来定义这个函数就能满足注册的需求。

如果已经有一个 dict，要调用一个关键字参数怎么办？Python 允许在 dict 前面加 `**` 号，把 dict 的所有 key-value 作为关键字参数传进去。例如：

```python
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

参数按值传递，函数中对 dict 的更改不会影响到函数外的 dict。

### 3.5 命名关键字参数

如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收 `city` 和 `job` 作为关键字参数。这种方式定义的函数如下：

```python
def person(name, age, *, city, job):
    print(name, age, city, job)
```

和关键字参数 `**kw` 不同，命名关键字参数需要一个特殊分隔符 `*`，`* `后面的参数被视为命名关键字参数。

调用方式如下：

```python
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer
```

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符 `*` 了：

```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```

每个命名关键字参数都需要传入，并且必须传入参数名。如果没有传入参数名或者没有给出所有命名关键字参数，调用将报错。

命名关键字参数可以有缺省值，从而简化调用：

```python
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
```

由于命名关键字参数 `city` 具有默认值，调用时，可不传入 `city` 参数。

### 3.6 参数组合

参数定义的顺序必须是：必选参数（位置参数）、默认参数、可变参数、命名关键字参数和关键字参数。

比如定义一个函数，包含上述若干种参数：

```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```

对于任意函数，都可以通过类似 `func(*args, **kw)` 的形式调用它，无论它的参数是如何定义的。

## 4 递归函数

解决递归调用栈溢出的方法是通过**尾递归**优化，事实上尾递归和循环的效果是一样的。

尾递归是指，在函数返回的时候，调用自身本身，并且，return 语句不能包含表达式。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

例如，计算 `n!` 的递归函数形式如下：

```python
def fact(n):
    if n == 1:
        return 1
    return n * fact(n - 1)
```

上面的 `fact(n)` 函数由于 `return n * fact(n - 1)` 引入了乘法表达式，所以就不是尾递归了。修改为尾递归函数如下：

```python
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
```

遗憾的是，大多数编程语言没有针对尾递归做优化，Python 解释器也没有做优化，所以，即使把上面的 `fact(n)` 函数改成尾递归方式，也会导致栈溢出。

# 四、高级特性

## 1 切片

对取指定索引范围元素的操作，用循环十分繁琐。因此，Python 提供了切片（Slice）操作符，能大大简化这种操作。

对于一个 list 变量 L，有如下常用切片访问操作：

- `L[m, n]` 取索引范围 [m, n) 内的元素
- `L[ : n]` 取索引范围 [0, n) 内的元素
- `L[m : ]` 取索引范围 [m, -1] 内的元素，-1 表示最后一个元素
- `L[ : ]` 取所有的元素，即复制 L
- `L[m : n : k]` 在索引范围 [m, n) 内每隔 k 个元素取一个元素

tuple 也是一种 list，唯一区别是 tuple 不可变。因此，tuple 也可以用切片操作，只是操作的结果仍是 tuple。

字符串 `'xxx'` 也可以看成是一种 list，每个元素就是一个字符。因此，字符串也可以用切片操作，只是操作结果仍是字符串。

## 2 迭代

迭代即遍历。Python 中 list、tuple、dict、set、str 均可通过 `for ... in ...` 进行迭代。可以通过 collections 模块的 Iterable 类型判断一个对象是否可以迭代：

```python
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str 是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list 是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```

对于 dict，默认情况下迭代的是 key，如果要迭代 value，可以用 `for value in d.values()`，如果要同时迭代 key 和 value，可以用 `for k, v in d.items()`。

如果在使用 `for ... in ...` 迭代 list 的过程中，希望访问到下标，可使用 Python 内置的 `enumerate()` 函数：

```python
for i, value in enumerate(['A', 'B', 'C']):
    print(i, value)
```

## 3 列表生成式

列表生成式用于快速简洁地生成列表，直接看代码：

```python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

循环后面还可以加上 if 判断，这样我们就可以筛选出仅偶数的平方：

```python
>>> [x * x for x in range(1, 11) if x & 1 == 0]
[4, 16, 36, 64, 100]
```

还可以使用两层循环，可以生成全排列：

```python
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

for 后面的 if 是过滤条件，不能带 else。for 前面的部分是一个表达式，它必须根据 for 后面的临时变量计算出结果，因此，如果 if 出现在 for 的前面，则必须带有 else。例如：

```python
>>> [x if x & 1 == 0 else -x for x in range(1, 11)]
[-1, 2, -3, 4, -5, 6, -7, 8, -9, 10]
```

## 4 生成器

通过列表生成式，我们可以直接创建一个列表。但是，如果需要创建一个包含 100 万个元素的列表，而实际大概率只会访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

所以，如果列表元素可以按照某种算法推算出来，这样就不必创建完整的 list，从而节省大量的空间。

在 Python 中，这种一边循环一边计算的机制，称为生成器（generator）。

**第一种实现方式**

创建一个 generator 最简单的一种方法是，把一个列表生成式的 `[]` 改成 `()`。例如：

```python
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```

如果需要依次访问 generator 的值，可以使用 `next()` 函数：

```python
>>> next(g)
0
>>> next(g)
1
>>> next(g)
4
```

generator 保存的是算法，每次调用 `next(g)`，就计算出 `g` 的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出 `StopIteration` 的错误。generator 也支持 `fo ... in ...` 迭代。

**第二种实现方式**

如果一个函数定义中包含 `yield` 关键字，那么这个函数就不再是一个普通函数，而是一个 generator。

对于 generator 的函数，在每次调用 `next()` 的时候执行，遇到 `yield` 语句返回，再次执行时从上次返回的 `yield` 语句处继续执行。

同样的，把函数改成 generator 后，基本上从来不会用 `next()` 来获取下一个返回值（获取完最后一个元素之后再执行 `next()` 或报错），而是直接使用 `for` 循环来迭代。

用 `for` 循环调用 generator 时，拿不到 generator 的 `return` 语句的返回值。如果想要拿到返回值，必须捕获 `StopIteration` 错误，返回值包含在 `StopIteration` 的 `value` 中。

```python
def powerList(n):
    if not isinstance(n, int):
        raise TypeError
    for i in range(n):
        yield i * i
    return "done"

g = powerList(3)
for i in range(10):
    try:
        print(next(g))
    except StopIteration as e:
        print('Generator return value:', e.value)
        break
```

输出：

```
0
1
4
Generator return value: done
```

**注意事项**

如果将一个生成器转化成列表，再将 `next()` 作用于生成器，将不会得到生成器中的任何元素。例如：

```python
g = (x * x for x in range(10))
L = list(g)
print(L)
while True:
    try:
        print(next(g))
    except StopIteration as e:
        print("Generator return value:", e.value)
        break
```

输出：

```
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
Generator return value: None
```

这是因为生成器表示的是一个数据流，在调用 `next()` 时才会计算下一个值。而在将生成器转化为 `list` 的过程中，所有的元素均已被计算出，因此，再调用 `next()` 只会抛出异常。

## 5 迭代器

可以直接作用于 `for` 循环的数据类型有以下几种：

- 一类是集合数据类型，如 `list`、`tuple`、`dict`、`set`、`str `等；

- 一类是 `generator`，包括生成器和带 `yield` 的 generator function。

这些可以直接作用于 `for` 循环的对象统称为可迭代对象：`Iterable`。

可以使用 `isinstance()` 判断一个对象是否是 `Iterable` 对象：

```python
>>> from collections.abc import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance((), Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False
```

注意：带 `yield` 的 generator function 也是 `Iterable` 对象，因为在 Python 中，一切皆为对象，包括函数。

可以被 `next()` 函数调用并不断返回下一个值的对象称为迭代器：`Iterator`。

可以使用 `isinstance()` 判断一个对象是否是 `Iterator` 对象：

```python
>>> from collections.abc import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance((), Iterator)
False
>>> isinstance('abc', Iterator)
False
```

**总结：**`list`、`tuple`、`dict`、`str `和 `generator` 都是 `Iterable` 对象，但只有 `generator` 是`Iterator` 对象。

把 `list`、`tuple`、`dict`、`str` 等 `Iterable` 对象转换成 `Iterator` 对象可以使用 `iter()` 函数。

```python
from collections.abc import Iterable, Iterator
g = iter("abc")
while True:
    try:
        print(next(g))
    except StopIteration as e:
        print("Generator return value:", e.value)
        break
```

输出：

```
a
b
c
Generator return value: None
```

`Iterator` 对象表示的是一个数据流，可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过 `next()` 函数实现按需计算下一个数据，所以 `Iterator` 的计算是惰性的，只有在需要返回下一个数据时它才会计算。

`Iterator` 甚至可以表示一个无限大的数据流，例如全体自然数。而使用 list 是永远不可能存储全体自然数的。

Python 的 `for `循环本质上就是通过不断调用 `next()` 函数实现的。

# 五、函数式编程

函数式编程就是一种抽象程度很高的编程范式，纯粹的函数式编程语言编写的函数没有变量。因此，任意一个函数，只要输入是确定的，输出就是确定的，这种纯函数我们称之为没有副作用。而允许使用变量的程序设计语言，由于函数内部的变量状态不确定，同样的输入，可能得到不同的输出，因此，这种函数是有副作用的。

函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数！

Python 对函数式编程提供部分支持。由于 Python 允许使用变量，因此，Python 不是纯函数式编程语言。

## 1 高阶函数

高阶函数特点如下：

1. 变量可以指向函数

2. 函数名也是变量：将函数名指向其他变量之后，便无法通过函数名调用原函数

3. 函数可以作为参数：这种函数称为高阶函数

   ```python
   def add(x, y, f):
       return f(x) + f(y)
   ```

### 1.1 map/reduce

Python 内建了 `map()` 和 `reduce()` 函数。

#### 1.1.1 map函数

`map()` 函数接收两个参数，一个是函数，一个是 `Iterable`对象，`map` 将传入的函数依次作用到序列的每个元素，并把结果作为新的 `Iterator` 返回。

```python
>>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

#### 1.1.2 reduce函数

`reduce()` 函数与 `map()` 函数接受参数类型一致，区别在于，`reduce ` 把结果继续和序列的下一个元素做累积计算，其效果就是：

```
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

从中可以总结出，`reduce(f, iterableObj)` 中的参数 `f`，需要接收两个参数，并且 `iterableObj` 中的元素类型能被函数 `f` 接受；此外，函数 `f` 需要能接受其自身的返回值类型作为参数。

```python
from functools import reduce

def add(x, y):
    return x + y

print(reduce(add, list(range(1, 101))))	# 输出：5050
```

下面展示一个 `reduce()` 更加实用的用法，将数值字符串转化为整型：

```python
from functools import reduce

DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def str2int(s):
    def fn(x, y):
        return x * 10 + y
    def char2num(s):
        return DIGITS[s]
    return reduce(fn, map(char2num, s))

print(str2int("123") + str2int("456"))
```

### 1.2 filter

`filter()` 也接收一个函数和一个序列作为参数。`filter()` 把传入的函数依次作用于每个元素，然后根据返回值是 `True` 还是 `False` 决定保留还是丢弃该元素。

例如，在一个 list 中，删掉偶数，只保留奇数，可以这么写：

```python
def is_odd(n):
    return n % 2 == 1

print(list(filter(is_odd, list(range(10)))))	# 输出：[1, 3, 5, 7, 9]
```

注意，`filter()` 函数接受的函数参数不一定必须返回 `True` 或者 `False`，因为在 Python 中，零值、空字符串、空序列、`None` 等表示 `False`，其余表示 `True`。

### 1.3 sorted

`sorted()` 是 Python 内置的排序函数，可以对 list 进行排序：

```python
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]
```

`sorted() `函数也是一个高阶函数，它还可以接收一个 `key` 函数来实现自定义的排序，`key` 指定的函数将作用于 list 的每一个元素上，并根据 `key` 函数返回的结果进行排序。例如按绝对值大小排序：

```python
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```

对字符串进行忽略大小写的升序排序：

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
['about', 'bob', 'Credit', 'Zoo']
```

如果需要降序排序，可以传入第三个参数 `reverse=True`：

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```

## 2 返回函数

### 2.1 函数作为返回值

实现一个可变参数的求和。通常情况下，求和的函数是这样定义的：

```python
def calc_sum(*args):
    ax = 0
    for n in args:
        ax = ax + n
    return ax
```

但是，如果不需要立刻求和，而是在后面的代码中，根据需要再计算怎么办？可以不返回求和的结果，而是返回求和的函数：

但是，如果不需要立刻求和，而是在后面的代码中，根据需要再计算怎么办？可以不返回求和的结果，而是返回求和的函数：

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```

当调用 `lazy_sum()` 时，返回的并不是求和结果，而是求和函数：

```python
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
```

调用函数 `f` 时，才真正计算求和的结果：

```python
>>> f()
25
```

在这个例子中，在函数 `lazy_sum` 中又定义了函数 `sum`，并且，内部函数 `sum` 可以引用外部函数 `lazy_sum` 的参数和局部变量，当 `lazy_sum` 返回函数 `sum` 时，相关参数和变量都保存在返回的函数中，这种称为“闭包（Closure）”的程序结构拥有极大的威力。

请再注意一点，当调用 `lazy_sum()` 时，每次调用都会返回一个新的函数，即使传入相同的参数：

```python
>>> f1 = lazy_sum(1, 3, 5, 7, 9)
>>> f2 = lazy_sum(1, 3, 5, 7, 9)
>>> f1==f2
False
```

`f1()` 和 `f2()` 的调用结果互不影响。

### 2.2 闭包

首先，做如下定义：

- 以函数作为返回值的函数称为外部函数；
- 作为返回值的函数在外部函数内部定义，称为内部函数；

内部函数引用了外部函数的参数和外部函数中定义的局部变量，当外部函数返回后，内部函数依然保持着这些变量的引用。另外，返回的函数并没有被立即执行，而是直到调用了才会被执行。

```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
```

在上面的例子中，每次循环，都创建了一个新的函数，然后，把创建的 3 个函数都返回了。

你可能认为调用 `f1()`，`f2()` 和 `f3()` 结果应该是 `1`，`4`，`9`，但实际结果是：

```python
>>> f1()
9
>>> f2()
9
>>> f3()
9
```

原因就在于返回的函数引用了变量 `i`，但它并非立刻执行。等到 3 个函数都返回时，它们所引用的变量 `i` 已经变成了 `3`，因此最终结果为 `9`。

***返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量。***

如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：

```python
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
```

再看看结果：

```python
>>> f1, f2, f3 = count()
>>> f1()
1
>>> f2()
4
>>> f3()
9
```

### 2.3 匿名函数

以 `map()` 函数为例，计算 `f(x) = x * x` 时，除了定义一个 `f(x)` 的函数外，还可以直接传入匿名函数：

```python
>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

通过对比可以看出，匿名函数 `lambda x: x * x` 实际上就是：

```python
def f(x):
    return x * x
```

关键字 `lambda` 表示匿名函数，冒号前面的 `x` 表示函数参数。

匿名函数有个限制，就是只能有一个表达式，不用写 `return`，返回值就是该表达式的结果。

此外，匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数：

```python
>>> f = lambda x: x * x
>>> f
<function <lambda> at 0x101c6ef28>
>>> f(5)
25
```

同样，也可以把匿名函数作为返回值返回，比如：

```python
def build(x, y):
    return lambda: x * x + y * y
```

### 2.4 装饰器

由于函数也是一个对象，而且函数对象可以被赋值给变量，所以，通过变量也能调用该函数。

```python
>>> def now():
...     print('2020-6-27')
...
>>> f = now
>>> f()
2020-6-27
```

函数对象有一个 `__name__` 属性，可以拿到函数的名字：

```python
>>> now.__name__
'now'
>>> f.__name__
'now'
```

现在，假设我们要增强 `now()` 函数的功能，比如，在函数调用前后自动打印日志，但又不希望修改 `now()` 函数的定义。这种在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）。

本质上，decorator 就是一个返回函数的高阶函数。所以，我们要定义一个能打印日志的 decorator，可以定义如下：

```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

上面的 `log()` 函数，因为它是一个 decorator，所以接受一个函数作为参数，并返回一个函数。借助 Python 的 @ 语法，把 decorator 置于函数的定义处：

```python
@log
def now():
    print('2020-6-27')
```

调用 `now()` 函数，不仅会运行 `now()` 函数本身，还会在运行 `now()` 函数前打印一行日志：

```python
>>> now()
call now():
2020-6-27
```

把 `@log` 放到 `now()` 函数的定义处，相当于执行了语句：

```python
now = log(now)
```

如果 decorator 本身需要传入参数，那就需要编写一个返回 decorator 的高阶函数，写出来会更复杂。比如，要自定义 log 的文本：

```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```

这个 3 层嵌套的 decorator 用法如下：

```python
@log('execute')
def now():
    print('2020-6-27')
```

执行结果如下：

```python
>>> now()
execute now():
2020-6-27
```

和两层嵌套的 decorator 相比，3 层嵌套的效果是这样的：

```python
>>> now = log('execute')(now)
```

以上两种 decorator 的定义都没有问题，但还差最后一步。因为函数也是对象，它有 `__name__` 等属性，但经过 decorator 装饰之后的函数，它们的 `__name__` 已经从原来的 `'now'` 变成了 `'wrapper'`：

```python
>>> now.__name__
'wrapper'
```

因为返回的那个 `wrapper()` 函数名字就是 `'wrapper'`，所以，需要把原始函数的 `__name__` 等属性复制到 `wrapper()` 函数中，否则，有些依赖函数签名的代码执行就会出错。

不需要编写 `wrapper.__name__ = func.__name__` 这样的代码，Python 内置的 `functools.wraps` 就是干这个事的。所以，一个完整的 decorator 的写法如下：

```python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

或者针对带参数的 decorator：

```python
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```

### 2.5 偏函数

通过设定参数的默认值，可以降低函数调用的难度。而偏函数也可以做到这一点。举例如下：

`int()` 函数可以把字符串转换为十进制表示的整数，当仅传入字符串时，`int()` 函数默认将字符串视为十进制数。但 `int()` 函数还提供额外的 `base` 参数，默认值为 `10`；如果传入 `base` 参数，就可以将字符串视为 N 进制数，并转换为十进制表示的整数：

```python
>>> int('12345', base=8)
5349
>>> int('12345', 16)
74565
```

假设要转换大量的二进制字符串，每次都传入 `int(x, base=2)` 非常麻烦。于是，可以定义一个 `int2()` 的函数，默认把 `base=2` 传进去：

```python
def int2(x, base=2):
    return int(x, base)
```

`functools.partial` 就是帮助我们创建一个偏函数的，不需要我们自己定义 `int2()`，可以直接使用下面的代码创建一个新的函数 `int2`：

```python
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
>>> int2('1010101')
85
```

需要注意的是，上面的新 `int2` 函数，仅仅是把 `base` 参数重新设定默认值为 `2`，但也可以在函数调用时传入其他值：

```python
>>> int2('1000000', base=10)
1000000
```

所以，简单总结 `functools.partial` 的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。

创建偏函数时，实际上可以接收函数对象、`*args` 和 `**kw` 这 3 个参数，当传入：

```python
int2 = functools.partial(int, base=2)
```

实际上固定了 `int()` 函数的关键字参数 `base`，也就是：

```python
int2('10010')
```

相当于：

```python
kw = { 'base': 2 }
int('10010', **kw)
```

当传入：

```python
max2 = functools.partial(max, 10)
```

实际上会把 `10` 作为 `*args` 的一部分自动加到左边，也就是：

```python
max2(5, 6, 7)
```

相当于：

```python
args = (10, 5, 6, 7)
max(*args)
```

# 六、模块

在 Python 中，一个 `.py` 文件就称之为一个模块（Module）。

使用模块最大的好处是大大提高了代码的可维护性。当一个模块编写完毕，就可以被其他地方引用。我们在编写程序的时候，也经常引用其他模块，包括 Python 内置的模块和来自第三方的模块。

使用模块还可以避免函数名和变量名冲突。但是也要注意，尽量不要与内置函数（[Built-in Functions](https://docs.python.org/3/library/functions.html)）名字冲突。

如果不同的人编写的模块名相同怎么办？为了避免模块名冲突，Python 又引入了按目录来组织模块的方法，称为包（Package）。

现在，假设我们的 `abc` 和 `xyz` 这两个模块名字与其他模块冲突了，于是我们可以通过包来组织模块，避免冲突。方法是选择一个顶层包名，比如 `mycompany`，按照如下目录存放：

```
mycompany
├─ __init__.py
├─ abc.py
└─ xyz.py
```

引入了包以后，只要顶层的包名不与别人冲突，那所有模块都不会与别人冲突。现在，`abc.py` 模块的名字就变成了 `mycompany.abc`，类似的，`xyz.py` 的模块名变成了 `mycompany.xyz`。

请注意，每一个包目录下面都会有一个 `__init__.py` 的文件，这个文件是必须存在的，否则，Python 就把这个目录当成普通目录，而不是一个包。`__init__.py` 可以是空文件，也可以有 Python 代码，因为 `__init__.py` 本身就是一个模块，而它的模块名就是 `mycompany`。

类似的，可以有多级目录，组成多级层次的包结构。比如如下的目录结构：

```
mycompany
 ├─ web
 │  ├─ __init__.py
 │  ├─ utils.py
 │  └─ www.py
 ├─ __init__.py
 ├─ abc.py
 └─ utils.py
```

文件 `www.py` 的模块名就是 `mycompany.web.www`，两个文件 `utils.py` 的模块名分别是 `mycompany.utils` 和 `mycompany.web.utils`。

***自己创建模块时要注意命名，不能和 Python 自带的模块名称冲突。例如，系统自带了 sys 模块，自己的模块就不可命名为 sys.py，否则将无法导入系统自带的 sys 模块。***

## 1 使用模块

Python 本身就内置了很多非常有用的模块，只要安装完毕，这些模块就可以立刻使用。

### 1.1 模块的标准文件模板

以内建的 `sys` 模块为例，编写一个 `hello` 的模块：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

' This is a test module. '

__author__ = 'Mason Hoo'

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```

第 1 行注释可以让这个 `hello.py` 文件直接在 Unix/Linux/Mac 上运行；

第 2 行注释表示 `.py` 文件本身使用标准 UTF-8 编码；

第 4 行是一个字符串，表示模块的文档注释，任何模块代码的第一个字符串都被视为模块的文档注释；

第 6 行使用 `__author__` 变量把作者写进去，这样当你公开源代码后别人就可以瞻仰你的大名；

以上就是 Python 模块的标准文件模板，当然也可以全部删掉不写，但是，按标准办事肯定没错。

导入 `sys` 模块后，我们就有了变量 `sys` 指向该模块，利用 `sys` 这个变量，就可以访问 `sys` 模块的所有功能。

`sys` 模块有一个 `argv` 变量，用 list 存储了命令行的所有参数。`argv` 至少有一个元素，因为第一个参数永远是该 `.py` 文件的名称，例如：

- 运行 `python hello.py` 获得的 `sys.argv` 就是 `['hello.py']`；

- 运行 `python hello.py Mason` 获得的 `sys.argv` 就是 `['hello.py', 'Mason']`。

最后，注意到这两行代码：

```python
if __name__=='__main__':
    test()
```

在命令行运行 `hello` 模块文件时，Python 解释器把一个特殊变量 `__name__` 置为 `__main__`，而如果在其他地方导入该 `hello` 模块时，`if` 判断将失败。因此，这种 `if` 测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。

我们可以用命令行运行 `hello.py` 看看效果：

```shell
$ python hello.py
Hello, world!
$ python hello.py Mason
Hello, Mason!
```

如果启动 Python 交互环境，再导入 `hello` 模块：

```python
>>> import hello
>>> hello.test()
Hello, world!
```

导入时，没有打印 `Hello, word!`，因为没有执行 `test()` 函数；调用 `hello.test()` 时，才能打印出 `Hello, word!`。

### 1.2 作用域

在一个模块中，我们可能会定义很多函数和变量，但有的函数和变量我们希望给别人使用，有的函数和变量我们希望仅仅在模块内部使用。在 Python 中，是通过 `_` 前缀来实现的。

正常的函数和变量名是公开的（public），可以被直接引用，比如：`abc`，`x123`，`PI` 等；

类似 `__xxx__` 这样的变量是特殊变量，可以被直接引用，但是有特殊用途，比如上面的 `__author__`，`__name__` 就是特殊变量，`hello` 模块定义的文档注释也可以用特殊变量 `__doc__` 访问，我们自己的变量一般不要用这种变量名；

类似 `_xxx` 和 `__xxx` 这样的函数或变量就是非公开的（private），**不应该**被直接引用，比如 `_abc`，`__abc` 等；

之所以我们说，private 函数和变量“不应该”被直接引用，而不是“不能”被直接引用，是因为 Python 并没有一种方法可以完全限制访问 private 函数或变量。但是，从编程习惯上不应该引用 private 函数或变量。

## 2 安装第三方模块

在 Python 中，安装第三方模块，是通过包管理工具 pip 完成的。

一般来说，第三方库都会在 Python 官方的 [pypi.python.org](https://pypi.python.org/) 网站注册。要安装一个第三方库，必须先知道该库的名称，可以在官网或者 pypi 上搜索，比如 Pillow（处理图像的工具库）的名称叫 [Pillow](https://pypi.python.org/pypi/Pillow/)，因此，安装 Pillow 的命令就是：

```shell
pip install Pillow
```

### 2.1 安装常用模块

在使用 Python 时，我们经常需要用到很多第三方库。用 pip 一个一个安装费时费力，还需要考虑兼容性。推荐直接使用 [Anaconda](https://www.anaconda.com/)，这是一个基于 Python 的数据处理和科学计算平台，它已经内置了许多非常有用的第三方库。装上 Anaconda，就相当于把数十个第三方模块自动安装好了，非常简单易用。

Anaconda 会把系统 Path 中的 Python 指向自带的 Python，并且，Anaconda 安装的第三方模块会安装在 Anaconda 自己的路径下，不影响系统已安装的 Python 目录。

安装好 Anaconda 后，在命令行窗口中输入 `python`，可以看到 Anaconda 的信息：

```shell
┌────────────────────────────────────────────────────────┐
│Command Prompt - python                           - □ x │
├────────────────────────────────────────────────────────┤
│Microsoft Windows [Version 10.0.0]                      │
│(c) 2015 Microsoft Corporation. All rights reserved.    │
│                                                        │
│C:\> python                                             │
│Python 3.6.3 |Anaconda, Inc.| ... on win32              │
│Type "help", ... for more information.                  │
│>>>                                                     │
│                                                        │
│                                                        │
│                                                        │
│                                                        │
└────────────────────────────────────────────────────────┘
```

### 2.2 模块搜索路径

当我们试图 `import` 一个模块时，默认情况下，Python 解释器会搜索当前目录、所有已安装的内置模块和第三方模块，搜索路径存放在 `sys` 模块的 `path` 变量中：

```python
>>> import sys
>>> sys.path
['', 'C:\\Softwares\\Python\\Python38\\python38.zip', 'C:\\Softwares\\Python\\Python38\\DLLs', 'C:\\Softwares\\Python\\Python38\\lib', 'C:\\Softwares\\Python\\Python38', 'C:\\Users\\Admin\\AppData\\Roaming\\Python\\Python38\\site-packages', 'C:\\Softwares\\Python\\Python38\\lib\\site-packages']
>>>
```

如果我们要添加自己的搜索目录，有两种方法：

- 一是直接修改 `sys.path`，添加要搜索的目录：

  ```python
  >>> import sys
  >>> sys.path.append('D:\\Python_Modules\\my_py_scripts')
  ```

  这种方法是在运行时修改，运行结束后失效。

- 二是设置环境变量 `PYTHONPATH`，该环境变量的内容会被自动添加到模块搜索路径中。设置方式与设置 Path 环境变量类似。注意只需要添加你自己的搜索路径，Python 自己本身的搜索路径不受影响。

# 七、面向对象编程

OOP（Object Oriented Programming）把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。

在 Python 中，所有数据类型都可以视为对象，当然也可以自定义对象。

## 1 类和实例

在 Python中，定义类是通过 `class` 关键字，以 Student 类为例：

```python
class Student(object):
    pass
```

`class `后面紧接着是类名，即 `Student`，类名通常是大写开头的单词；紧接着是 `(object)`，表示该类是从哪个类继承下来的，通常，如果没有合适的继承类，就使用 `object` 类，这是所有类最终都会继承的类。

定义好了 `Student` 类，就可以根据 `Student `类创建出 `Student` 的实例，创建实例是通过类名+()实现的。此外，可以自由地给一个实例变量绑定属性（并不是在类中定义的），比如，给实例 `stu` 绑定一个 `name` 属性：

```python
>>> stu = Student()
>>> stu.name = "Mason Hoo"
>>> print(stu.name)
'Mason Hoo'
```

由于类可以起到模板的作用，因此，可以在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。通过定义一个特殊的 `__init__` 方法，在创建实例的时候，就把 `name`，`score` 等属性绑上去：

```python
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score
```

***注意：特殊方法 `__init__` 前后分别有两个下划线！！！***

`__init__` 方法的第一个参数永远是 `self`，表示创建的实例本身。因此，在 `__init__` 方法内部，就可以把各种属性绑定到 `self`。

有了 `__init__` 方法，在创建实例的时候，就不能传入空的参数了，必须传入与 `__init__` 方法匹配的参数，但 `self` 不需要传，Python 解释器自己会把实例变量传进去：

```python
>>> stu = Student('Mason Hoo', 99)
>>> stu.name
'Mason Hoo'
>>> stu.score
99
```

要定义一个方法，除了第一个参数是 `self` 外，其他和普通函数一样。要调用一个方法，只需要在实例变量上直接调用，除了 `self` 不用传递，其他参数正常传入：

```python
class Student(object): 
    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))

stu = Student('Mason Hoo', 99)
stu.print_score()	# 输出：Mason Hoo: 99
```

## 2 访问限制

如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线 `__`，在 Python 中，实例的变量名如果以 `__` 开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问。所以，我们把上一节中的 Student 类改一改：

```python
class Student(object):

    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
```

改完后，对于外部代码来说，没什么变动，但是已经无法从外部访问类属性了：

```python
>>> stu = Student('Mason Hoo', 99)
>>> stu.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
```

如果外部代码要获取和更改 `name` 和 `score` 怎么办？可以给 Student 类增加 `get_name`、`get_score`、`set_name`、`set_score` 这样的方法。

需要注意的是，在 Python 中，变量名类似 `__xxx__` 的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量。特殊变量是可以直接访问的，不是 private 变量。所以，不能用 `__name__`、`__score__ `这样的变量名。

以一个下划线开头的实例变量名，比如`_name`，这样的实例变量外部是可以访问的。但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”。

双下划线开头的实例变量是不是一定不能从外部访问呢？其实也不是。不能直接访问 `__name` 是因为 Python 解释器对外把 `__name` 变量改成了 `_Student__name`，所以，仍然可以通过 `_Student__name` 来访问 `__name` 变量：

```python
>>> stu._Student__name
'Mason Hoo'
```

但是强烈建议你不要这么干，因为不同版本的 Python 解释器可能会把 `__name` 改成不同的变量名。

最后注意下面的这种错误写法：

```python
>>> stu = Student('Mason Hoo', 99)
>>> stu.print_score()
'Mason Hoo: 99'
>>> stu.__name = 'Way Hoo' # 设置__name变量！
>>> stu.__name
'Way Hoo'
```

表面上看，外部代码“成功”地设置了 `__name` 变量，但实际上这个 `__name` 变量和 class 内部的 `__name` 变量不是一个变量！内部的 `__name` 变量已经被 Python 解释器自动改成了 `_Student__name`，而外部代码给 `stu` 新增了一个 `__name` 变量。不信试试：

```python
>>> stu.print_score()
'Mason Hoo: 99'
```

## 3 继承和多态

在继承关系中，如果一个实例的数据类型是某个子类，那它的数据类型也可以被看做是父类。但是，反过来就不行。

## 4 获取对象信息

### 4.1 type()

使用 `type()` 函数可以判断对象的类型。基本类型都可以用 `type()` 判断：

```python
>>> type(123)
<class 'int'>
>>> type('str')
<class 'str'>
>>> type(None)
<type(None) 'NoneType'>
```

如果一个变量指向函数或者类，也可以用 `type()` 判断：

```
>>> type(abs)
<class 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>
```

`type() `函数返回对应的 Class 类型：

```python
>>> type(123)==type(456)
True
>>> type(123)==int
True
>>> type('abc')==type('123')
True
>>> type('abc')==str
True
>>> type('abc')==type(123)
False
```

判断基本数据类型可以直接写 `int`，`str` 等，但如果要判断一个对象是否是函数怎么办？可以使用 `types` 模块中定义的常量：

```python
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```

### 4.2 isinstance()

`isinstance() `函数也可以判断 Class 类型，常规使用方式是：`isinstance(obj, Class)`。例如：

```python
>>> isinstance('a', str)
True
>>> isinstance(123, int)
True
>>> isinstance(b'a', bytes)
True
```

还可以判断一个变量是否是某些类型中的一种，比如下面的代码就可以判断是否是 list 或者 tuple：

```python
>>> isinstance([1, 2, 3], (list, tuple))
True
>>> isinstance((1, 2, 3), (list, tuple))
True
```

### 4.3 dir()

`dir()` 函数可以获得一个对象的所有属性和方法，它返回一个包含字符串的 list。比如，获得一个 str 对象的所有属性和方法：

```python
>>> dir('ABC')
['__add__', '__class__',..., '__len__', 'capitalize', 'casefold',..., 'zfill']
```

类似 `__xxx__` 的属性和方法在 Python 中都是有特殊用途的，比如 `__len__()` 方法返回长度。在 Python 中，如果你调用 `len()` 函数试图获取一个对象的长度，实际上，在 `len()` 函数内部，它自动去调用该对象的 `__len__()` 方法，所以，下面的代码是等价的：

```python
>>> len('ABC')
3
>>> 'ABC'.__len__()
3
```

我们自己写的类，如果也想用 `len(myObj)` 的话，就自己写一个 `__len__()` 方法：

```python
>>> class MyDog(object):
...     def __len__(self):
...         return 100
...
>>> dog = MyDog()
>>> len(dog)
100
```

仅仅把属性和方法列出来是不够的，配合 `getattr()`、`setattr() `以及 `hasattr()`，我们可以直接操作一个对象的状态：

```python
>>> class MyObject(object):
...     def __init__(self):
...         self.x = 9
...     def power(self):
...         return self.x * self.x
...
>>> obj = MyObject()
```

紧接着，可以测试该对象的属性：

```python
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
```

如果试图获取不存在的属性，会抛出 AttributeError 的错误：

```python
>>> getattr(obj, 'z') # 获取属性'z'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'MyObject' object has no attribute 'z'
```

`getattr()` 可以传入一个 default 参数，如果属性不存在，就返回默认值：

```python
>>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
404
```

也可以获得对象的方法：

```python
>>> hasattr(obj, 'power') # 有属性'power'吗？
True
>>> fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量fn
>>> fn # fn指向obj.power
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn() # 调用fn()与调用obj.power()是一样的
81
```

## 5 实例属性和类属性

给实例绑定属性的方法是通过实例变量，或者通过 `self` 变量：

```python
class Student(object):
    def __init__(self, name):
        self.name = name

s = Student('Bob')
s.score = 90
```

但是，如果类本身需要绑定一个属性呢？可以直接在 class 中定义属性，这种属性是类属性，归类所有，但类的所有实例都可以访问到。

```python
>>> class Student(object):
...     name = 'Student'
...
>>> s = Student() # 创建实例s
>>> print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
Student
>>> print(Student.name) # 打印类的name属性
Student
>>> s.name = 'Michael' # 给实例绑定name属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
Michael
>>> print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
Student
>>> del s.name # 如果删除实例的name属性
>>> print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
Student
```

# 八、面向对象高级编程

## 1 \__slots__

### 1.1 为类和实例绑定属性与方法

正常情况下，当我们定义了一个 class，创建了一个 class 的实例后，我们可以给该实例绑定任何属性和方法，这就是动态语言的灵活性。先定义 class：

```python
class Student(object):
    pass
```

然后，尝试给实例绑定一个属性：

```python
>>> s = Student()
>>> s.name = 'Michael' # 动态给实例绑定一个属性
>>> print(s.name)
'Michael'
```

还可以尝试给实例绑定一个方法：

```python
>>> def set_age(self, age): # 定义一个函数作为实例方法
...     self.age = age
...
>>> from types import MethodType
>>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
>>> s.set_age(25) # 调用实例方法
>>> s.age # 测试结果
25
```

但是，给一个实例绑定的方法，对另一个实例是不起作用的：

```python
>>> s2 = Student() # 创建新的实例
>>> s2.set_age(25) # 尝试调用方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'
```

为了给所有实例都绑定方法，可以给 class 绑定方法：

```python
>>> def set_score(self, score):
...     self.score = score
...
>>> Student.set_score = set_score
```

给 class 绑定方法后，所有实例均可调用：

```python
>>> s.set_score(100)
>>> s.score
100
>>> s2.set_score(99)
>>> s2.score
99
```

### 1.2 使用\__slots__

如果我们想要限制实例属性的绑定怎么办？比如，只允许对 Student 实例添加 `name` 和 `age` 属性。

为了达到限制的目的，Python 允许在定义 class 的时候，定义一个特殊的 `__slots__` 变量，来限制该 class 实例能添加的属性：

```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```

然后，尝试给 `Student` 实例绑定属性：

```python
>>> s = Student() # 创建新的实例
>>> s.name = 'Michael' # 绑定属性'name'
>>> s.age = 25 # 绑定属性'age'
>>> s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```

使用 `__slots__` 要注意，`__slots__` 定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：

```python
>>> class GraduateStudent(Student):
...     pass
...
>>> g = GraduateStudent()
>>> g.score = 9999
```

除非在子类中也定义 `__slots__`，这样，子类实例允许定义的属性就是自身的 `__slots__` 加上父类的 `__slots__`。

## 2 @property

在绑定属性时，如果我们直接把属性暴露出去，虽然写起来很简单，但是，没办法检查参数，导致可以把成绩随便改：

```python
s = Student()
s.score = 9999
```

这显然不合逻辑。为了限制 score 的范围，可以通过一个 `set_score()` 方法来设置成绩，再通过一个 `get_score()` 来获取成绩。这样，在 `set_score()` 方法里，就可以检查参数：

```python
class Student(object):

    def get_score(self):
         return self._score

    def set_score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

现在，对任意的Student实例进行操作，就不能随心所欲地设置score了：

```python
>>> s = Student()
>>> s.set_score(60) # ok!
>>> s.get_score()
60
>>> s.set_score(9999)
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

但是，上面的调用方法又略显复杂，没有直接用属性这么直接简单。

有没有既能检查参数，又可以用类似属性这样简单的方式来访问类的变量呢？对于追求完美的 Python 程序员来说，这是必须要做到的！

还记得装饰器（decorator）可以给函数动态加上功能吗？对于类的方法，装饰器一样起作用。Python 内置的 `@property` 装饰器就是负责把一个方法变成属性调用的：

```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

`@property `的实现比较复杂，我们先考察如何使用。把一个 getter 方法变成属性，只需要加上 `@property` 就可以了，此时，`@property` 本身又创建了另一个装饰器 `@score.setter`，负责把一个 setter 方法变成属性赋值，于是，我们就拥有一个可控的属性操作：

```python
>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.set_score(60)
>>> s.score # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

还可以定义只读属性，只定义 getter 方法，不定义 setter 方法就是一个只读属性：

```python
import datetime

class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return datetime.datetime.now().year - self._birth
```

上面的 `birth` 是可读写属性，而 `age` 就是一个*只读*属性，因为 `age` 可以根据 `birth` 和当前时间计算出来。

## 3 多重继承

在设计类的继承关系时，通常，主线都是单一继承下来的，例如，`Dog` 和 `Bird` 类继承自 `Animal`。但是，如果需要“混入”额外的功能，通过多重继承就可以实现，例如，`Dog` 类可以实现 `Runnable` 功能，则 `Dog` 类再继承 `Runnable` 类，同理 `Bird` 类再继承 `Flyable` 类。这种设计通常称之为 MixIn。

为了更好地看出继承关系，我们把 `Runnable` 和 `Flyable` 改为 `RunnableMixIn` 和 `FlyableMixIn`。

继承关系如下：

```python
class Animal(object):
    pass

class RunnableMixIn(object):
    def run(self):
        print('Running...')

class FlyableMixIn(object):
    def fly(self):
        print('Flying...')

class Dog(Animal, RunnableMixIn):
    pass

class Bird(Animal, FlyableMixIn):
    pass
```

## 4 定制类

待续......