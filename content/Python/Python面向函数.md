# Python内置函数
目前，Python共69个内置函数，详见[Python官方文档](https://docs.python.org/zh-cn/3.9/library/functions.html)

## isinstance()
```python
from collections.abc import Iterable

list = [1,2,3]
print(isinstance(list,Iterable)) 

# Output
True
```
语法：`isinstance(object, classinfo)`
`Iterable`类型需要先导入，注意该类型只能大写，`Iterator`迭代器判断同理
可见下文：[[Python面向函数#迭代器【Iterator】与生成器【Generator】]]



# 函数编程与函数式编程的差异

**函数编程：用函数来组织代码。  
函数式编程：一种强调“根据输入算出结果，尽量不改动外部数据”的编程风格。**

比如给成绩列表增加一个分数：

```python
# 直接修改原列表
def add_score(scores):
    scores.append(100)
```

```python
# 返回新列表，原列表保持不变，更符合函数式风格
def add_score(scores):
    return scores + [100]
```

两段都使用了函数，但第二段更体现函数式编程的思路。

你现在学的**定义函数、传参、返回值**，属于函数基础；“函数式编程”是在这个基础上，进一步讨论**怎样设计和组合函数**。

另外，“函数编程”不是严格统一的术语，有些课程也会拿它作为“函数式编程”的简称


# Python 函数参数

这些概念不是互相排斥的分类，而是从不同角度描述函数如何接收数据

## 1. 形参「parameter」与实参「argument」：谁来接收，传递什么

- **形参**：定义函数时，用来接收数据的名称，相当于一个暂时的标签？
- **实参**：调用函数时，实际提供的值或表达式，真正的作用对象

```python
def greet(name):   # name 是形参
    print(name)

greet("Vincent")   # "Vincent" 是实参
```

实参也可以用变量提供，变量名不需要与形参名一致：

```python
username = "Vincent"
greet(username)    # 把 username 的值交给 name
```

## 2. 位置与关键字：如何对应

- **位置实参**：按照顺序对应形参。
- **关键字实参**：通过形参名称指定对应关系。

```python
def greet(name, site):
    print(name, site)

greet("Vincent", "学习平台")             # 按位置
greet(site="学习平台", name="Vincent")   # 按名称
```

调用中的 `site="学习平台"` 表示“把这个值传给形参 `site`”，不会创建外部变量 `site`。

## 3. 默认参数：没传值时怎么办

默认参数就是**带默认值的形参**。调用时没有提供值，才使用默认值。

```python
def greet(name, site="学习平台"):
    print(name, site)

greet("Vincent")                # site 使用默认值
greet("Vincent", "课程网站")     # 按位置提供新值
greet("Vincent", site="论坛")   # 按关键字提供新值
```
正如 `print("", end = "")`，这就是在修改默认参数

## 4. 动态参数：数量不固定怎么办

也叫可变数量参数：
- `*args`：把额外的位置实参收集成**元组**。
- `**kwargs`：把未匹配明确形参的关键字实参收集成**字典**。

```python
def show(name, *args, **kwargs):
    print(name)
    print(args)
    print(kwargs)

show("Vincent", "Python", "Linux", city="南京")
```
注意定义函数也要冒号引出下文

输出：
```text
Vincent
('Python', 'Linux')
{'city': '南京'}
```

`args`、`kwargs` 是惯用名称，可以更换；起特殊作用的是定义中的 `*` 和 `**`。没有额外实参时，分别得到空元组和空字典



# 命名空间（Namespace）与作用域（Scope）
 **LEGB**：Local（局部）、Enclosing（外层函数）、Global（模块全局）、Built-in（内置）
 
可以把命名空间理解成一个以“名字”为键、以“对象”为值的字典
如，可以把
```python
site = "学习平台"
count = 3
```
看作是
```python
{
    "site": "学习平台",
    "count": 3
}
```
名字对应的是**对象**：`site` 指向字符串对象，`count` 指向整数对象
类比形参与实参，对象就是那个实参

文档说明，作用域规定了变量的可访问性或可见性，我认为这是有道理的


# 闭包「closure」

AI给的解释是，闭包就是一个由几件事组合后产生的能力：函数里面定义函数、返回里面的函数、里面的函数使用外层变量「作用域链？？」

以服务器告警为例
```python
def is_alert(value, threshold)
	return value >= threshold
	
is_alert(95,90) # True
is_alert(85,90) # False
```
如果 CPU 告警线一直是 `90`，每次都传 `90` 有些重复。于是改进为，
```python
def make_checker(threshold)
	def is_alert(value)
		return value >= threshold
	return is_alert
	
check_cpu = make_checker(90)
print(check_cpu(85))
print(check_cpu(95))
```
相当于，外层函数返回另一个函数，该函数带着外层函数输入进来的值，继续接受新的输入，从而得到里面函数的返回值

注意：`check_cpu = make_checker(90)`的`make_checker(90)`只是把返回的并未立刻执行的is_alert赋值给了check_cpu，它需要再括号一次才会执行其本身


# Python装饰器「Decorator」
装饰器，是一种功能，但本质上也是一个函数；可以在不修改函数代码的情况下，动态的修改函数；它接受一个函数作为参数，并返回一个参数「闭包也返回函数」

从最简单的代码开始
```python
def func1(a):
	print('a')
	
# 我们希望装饰该函数，使其在主功能上下额外 print 一些内容
# 借用闭包的写法

def func2():
	def inner():
		print('python')
		func1()
		print('Typora')
	return inner
	
func1 = func2()
func1('zhangsan') # 这里我设计的已经是带参数的，并不很基础

```
以上是错误写法，问题在于，
1. 当把func2（）赋值给func1，func1就指向inner这个函数，但inner这个函数是不接受参数的，而我却在最后给func1传递了str，所以直接报错【TypeError】
2. 装饰器的本意：不修改原函数，在原函数执行的基础上，额外添加功能；即：原函数是要运行的。但该inner内部的func1直接指向外部被修改的func1，而不是原本的func1，即inner内部会再运行inner，导致不断调用自己，最终递归错误【RecursionError】


因此，装饰器需要先把原func1函数作为参数传进去，而不是直接在inner内部指向func1
```python
def func1(a):
	print(a)

def func2(func):
	def inner(a):
		print('python')
		func(a)
		print('Typora')
	return inner
	
func1 = func2(func1) # 此处把携带原func1的inner传给新的对象func1
func1('vincent')
```
注意：后面的`func1赋值`不是修改`func1函数体`内部逻辑，我们完全可以改为：
```python
def func1(a):
	print(a)

def func2(func):
	def inner(a):
		print('python')
		func(a)
		print('Typora')
	return inner
	
Try = func2(func1) 
Try('vincent')
```

更多纠错放在常规示例中：
[[Python示例·装饰器·测试函数执行时间]]

## wraps装饰器
上述逻辑依旧会修改func1的定义【func1该形参不再对应func1函数对象，而指向wrap出来的my_decorator() 】，所以进一步需要wraps：
```python
from functools import wraps
import time

def func1():
	print('this is func1')
	
def timer(input_func):
	@wraps(input_func) # 注意：wraps不同于timer，是需要输入参数的
	def inner():
		start = time.time()
		input_func()
		end = time.time()
		print(f'程序运行时间为：{end - start}')
	return inner
	
Try = timer(func1)
Try()
print(Try.__name__) # 查看函数的名称

# Output:
this is func1
程序运行时间为：1.6927719116210938e-05
func1
```

外部`timer`为`decorator`装饰器，内部`inner`为`wrapper`装饰函数


## 多个装饰器装饰一个函数
洋葱结构

```python
@decorator_a
@decorator_b
def func1():
	xxx
	
# 相当于：
func1 = decorator_a(decorator_b(func1))
```

A 的调用前操作
    B 的调用前操作
        原函数
    B 的调用后操作
A 的调用后操作


## 更多的拓展内容，等后续用到时再重新增删改查……
```
我关心的“在函数中间插入代码”，确实有更深入的技术，但那属于另一层能力。

例如，可以先改变函数的代码结构，再返回修改后的函数。涉及的技术包括 Abstract Syntax Tree（抽象语法树，简称 AST）：把源代码解析成一种可以由程序分析、修改的结构。某些工具也会修改更底层的字节码

装饰器可以作为触发这种修改的入口，但真正完成插入的是代码改写技术。这些方案有源码获取、作用域、调试和 Python 版本兼容等限制
```


# 函数注册「registration」
把函数装进列表里，用`list[i]()`直接调用，或者进行其他操作
```python
tasks = []

def register(func):
    tasks.append(func)  # 保存函数，不执行
    return func        # 保留原函数名的正常使用

@register
def check_disk():
    print('执行磁盘检查')

@register
def check_network():
    print('执行网络检查')

for task in tasks:
    task()
    
# Output
执行磁盘检查
执行网络检查
```



# 迭代器「Iterator」与生成器「Generator」

## 迭代器
对于迭代器，有以下代码：
```python
list = [1,2,3,'a','b','c']
list_iter = list.__iter__() # 初始化一个迭代器【类似一个大函数体】
item = list_iter.__next__()
print(item)

# 初始化迭代器还可以用`list_iter = iter(list)`，写起来更短
```
该代码有潜在报错风险：**StopIteration**

```python
list = [1,2,3,'a','b','c']
list_iter = list.__iter__() # 初始化一个迭代器【类似一个大函数体】

while True: # 注意：while循环体使用大写 True，小写 true 会报错`未定义`
	try:
 		print(list_iter.__next__())
	except StopIteration:
		print('迭代完成')
		break
```
此代码就稍微完善

如何判断一个对象是迭代器呢？可以参考[[Python面向函数#isinstance()]]；代码懒得写了

## 生成器
只要函数里有了`yield`，它就会变成生成器
最简单的例子是:
```python
def output_func(number): # 先定义函数，内部yield还未开始执行
	for i in range(1,number+1)
		yield i # 该处yield选择返回i的值
		
for num in output_func(10) # 当外部开始向generator索要数据时，开始执行
	print(num) # num拿了output_func的i值
```
如果不做成生成器，直接返回值，那就是简单的：
```python
def output_func(number):
	for i in range(1,number+1):
		print(i)
output_func(10)
```
差别在于：函数内部的返回值从`print`改成了`yield`；于是，只有当外部索要时，内部才会让for循环向下执行，像是有`__next()__`的存在

我们简单的进行类别判定，有：
```python
def output_func(number):
	for i in range(1,number+1):
		yield i
num1 = output_func(10) # 初始化一个生成器
print(num1.__class__)

# output
<class 'generator'>
```
说明：只把output_func赋值给num1，num1接收到的是一整个生成器，而不是里面的一个yield

再稍微修改一下，就可以得到：
```python
def output_func(number):
	for i in range(1,number+1):
		yield i
num1 = output_func(10).__next__() # 符合装饰器的直觉，即要再运行一次
print(num1.__class__)

# output
<class 'int'>
```

那如何一个个拿函数里的yield值呢？
```python
def output_func(number):
	for i in range(1,number+1):
		yield i

num1 = output_func(10) # 不可再写__next__()，因为会让num1指向一个int值
print(num1.__next__())
print(num1.__next__())
print(num1.__next__())
print(num1.__next__()) # 用for循环打印也可以，记得控制数量与异常处理
```

而send这玩意怎么用呢？
```python
def generator():
	print('vincent')
	transit = yield 10086
	print(f'We tansit it:{tansit}')
	yield 'passion' # 不知为何末尾必须加一个yield，否则有StopIteration异常

g = generator() # g 保存生成器对象,还未开始执行
ret = g.__next__() # __next__() 不仅是向下一个，也是初始的启动
print(ret,ret.__class__)
ret = g.send('wenhong')
print(ret,ret.__class__)

# output
vincent
10086 <class 'int'>
We tansit it:wenhong
passion <class 'str'>
```
确实有点复杂，还没搞懂，先暂缓吧


# 模块和包「module」「package」
## 模块
模块分三类：自定义模块、标准库模块、第三方模块
其中，标准库模块就是Python自带的模块，无需pip，包含两个子类
1. 内置模块 built-in module —— C语言编写，编译进Python解释器，无独立.py源码
2. 纯Python源码编写，存放在Lib文件夹


### os 模块（Operating System 操作系统）
作用：Python与操作系统交互，管理文件/目录
```python
import os
#以下列出我接触到的相关函数

os.getcwd() # 获取当前工作目录
os.listdir() # 列出当前目录文件
os.remove()
os.rename() 
```
属标准库「Standard Library」模块，Python源码实现，但不属于内置模块

### sys 模块（System 系统）
作用：查看和影响"当前这个 Python 程序的运行情况"。所谓运行情况，说起来也复杂，反正我不理解；似乎是与Python解释器交互的接口？
```python
import sys
# 以下列出我接触到的相关函数

print(sys.path) # 规定了需要某些非源码实现的模块去哪里找，
```
属标准库模块下的内置模块

如`sys.path`：
```python
import sys
sys.path.append(
	'/Users/vincent/TraeCode CN Projects/package'
)
import module # module为自定义模块
```
此时就能找到module模块，并执行内部的代码；但如果直接导入
```python
import module
```
则会显示：**ModuleNotFoundError**: No module named 'module'




## 包
包，只是一种信息的组织形式，意在提供更加清晰的结构
**pip**：Python的包安装管理器「Package Installer for Python」用于导入第三方模块与包

包含`__init__.py` 文件的目录还是写一写比较好，这样兼容性更好
似乎在导入包之后，该文件里的内容会被首先执行，具体我不了解，以后再说


## 绝对导入和相对导入
### 绝对导入：
使用`sys.path`为起始点开始导入，详见[[Python面向函数#sys 模块（System 系统）]]

### **相对导入**：
参照当前所在文件的文件夹为起始开始查找