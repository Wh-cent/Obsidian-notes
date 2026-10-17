
# Python内置数据结构

### 序列容器：有序，支持索引、切片for遍历

1. str 字符串：不可变，内部元素只能是字符
2. list 列表：可变，元素可以是任意对象
3. tuple 元组：不可变，元素可以是任意对象

### 非序列容器：不支持数字索引、切片

4. set 集合：可变，无序，元素必须可哈希，不能放list这类可变对象
5. dict 字典：可变；在Python3.7后保留插入顺序；key必须可哈希，value无限制

对于这些类型，有增删（改）查集中操作，外加其他操作等设计  
具体的不展开，因为太多了，暂时就是不理解

字典是一种映射关系，所以有映射关系的似乎可以看作一种字典


### 注意`dict`的保存形式
```python
a = 'num1'
b = 'num2'
dict = {a:'1',b:'2'} # 注意a与b的赋值，字典的keyword需要定义
print(dict)
```
同时，记住`list`用方括号；`tuple`用圆括号；`dict`和`set`都用花括号


# Python字符串 str

### 字符串常用方法：

```python
text = "hello world!"

text.replace("hello", "Hello") # 替换，返回 "Hello Python"
text.split() # 按空白拆分成列表，返回 ["Hello", "Python"]
text.strip() # 删除字符串首尾空白（空格 \n \t），中间不变
```

注意：这些用法必须为`text = text.xxx` ，此类函数本身并不会自己运行，必须赋值给另一个对象 —— 或许是因为这些操作都「修改类」，不是「创造类」？



## f-string
「formatted string literal 格式化字符串字面值」

- 基础作用：为了能够更好看的输出一串中间需要夹带自变量等的字符串
- 基础结构：f"普通文字{Python表达式}普通文字"

还有冒号型写法，这里与{格式化}有点关系：
```python
num = int(input("Please input an integer: "))
print(f"This is the output: {num:o}")
```
冒号前面：记录要显示什么 、 冒号后面：记录如何显示

疑惑：该板块属于什么大板块的知识呢？



## 字符串 str：end = "" 与 +"/n" 的区别

### 一句话总结：它们作用的位置不同

1. end = " " 控制 print ( ) 打印结束后追加什么
2. + "\n" 是在字符串本身后面添加一个换行符「这个本来就是一个字符，可以加在其他字符后面」

### print 的默认设置
print本身默认在内容后追加换行  
即默认：print("你好") 相当于 print("你好", end="/n")

在讨论函数形式参数/位置参数时，对print有更深的理解：print(line, end="") 也是同样的机制：end="" 明确把空字符串传给 print() 的 end 参数「该参数为默认参数，若不提供该参数的值，就使用默认值」

即：print本身也是函数，只不过是Python已经写好的，print有其需要输入的参数和已经被写好的参数


### 文件读取上的特殊应用
文件读取中经常会运用到end加上空，是因为在读取中，其读取内容本身就已经携带了换行字符，若再使用print的默认设置，则会再换行一次，即叠加了两个换行字符。在终端就会显示输出一行，空一行，再输出一行




# 数据结构操作


## pop()

选取，并自动将其移出原数据结构
```python
item = list.pop() # 默认删去最后一个元素
item = list.pop(0)
item = dict.pop('key name')
item = set.pop()  # 随意删除一个
```
tuple与str没有pop操作

