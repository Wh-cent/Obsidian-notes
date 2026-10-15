

## 函数编程与函数式编程的差异

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


## Python 函数参数

这些概念不是互相排斥的分类，而是从不同角度描述函数如何接收数据

### 1. 形参「parameter」与实参「argument」：谁来接收，传递什么

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

### 2. 位置与关键字：如何对应

- **位置实参**：按照顺序对应形参。
- **关键字实参**：通过形参名称指定对应关系。

```python
def greet(name, site):
    print(name, site)

greet("Vincent", "学习平台")             # 按位置
greet(site="学习平台", name="Vincent")   # 按名称
```

调用中的 `site="学习平台"` 表示“把这个值传给形参 `site`”，不会创建外部变量 `site`。

### 3. 默认参数：没传值时怎么办

默认参数就是**带默认值的形参**。调用时没有提供值，才使用默认值。

```python
def greet(name, site="学习平台"):
    print(name, site)

greet("Vincent")                # site 使用默认值
greet("Vincent", "课程网站")     # 按位置提供新值
greet("Vincent", site="论坛")   # 按关键字提供新值
```
正如 `print("", end = "")`，这就是在修改默认参数

### 4. 动态参数：数量不固定怎么办

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



## 命名空间（Namespace）与作用域（Scope）
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


## 闭包

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


## Python装饰器
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
func1('zhangsan')

```
以上是错误写法，问题在于，
