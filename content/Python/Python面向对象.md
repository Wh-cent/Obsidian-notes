# 从定义人类开始 「Beginning」
有初步的代码，设计一份人类的定义：
```python
class Human(object): 
	'''
	在代码中，这段内容似乎无法被直接调用出来；只是用来看的？
	'''
	class_attribute_mind = 'clever' # 类属性，不独属于某个实例化的对象
	def __init__(self, name, age): # 实例属性 instance attribute 
		self.name = name
		self.age = age
		
	def think(self): #实例方法 instance method
		print('thinking')

vincent = Human('vincent', 20)
print(vincent.name)

# output
vincent
```
其中，类名为`Human`，采用大驼峰命名法；`object`似乎是Python最基础的基类，所有类都是从它这里继承出去的「继承什么意思我也不知道」，如果不继承自object，似乎这里的初始化就完不成了

往下定义类属性、实例属性与实例方法，其中实例属性是通过类的内置方法定义出来的
发现，在实例属性初始化中，需要写入`self`；因为在索取实例属性时，需要有个接口。由于我已经实例化了一个单位，所以不能用`__init__`去对接里面的属性，所以我得用实例化后的对象名称「上述代码的对象名称和实例属性name只是正巧重合了」,而
```python
object_one = Human('vincent', 20)
print(object_one.name)

# output
vincent
```
自然是完全可以的

注意：
```python
object_one = Human('vincent', 20)
print(object_one.think())

# output
None
```
这是因为`think()`实例方法的操作是打印一段话，而如果要打印"打印一段话"，就会返回空，因为一段话的布尔值为空，所以，如果希望能在外部打印，实例方法可以修改为：
```python
def think(self):
	return f'{self.name} is thinking'
print(vincent.think())

# output
vincent is thinking
```


# 类「class」与对象「object / instance」
## 类的方法
类下面有「方法」，其中包括内置方法和自定义方法
**内置方法**：`__init__`以及其他。应该都是object提供的
**自定义方法**：上述代码中的`def think()`等

由于放在了同一层级，所以我认为这两种方法是等价的；就像模块分为标准库模块和自定义模块，只是在使用上略有不同【参考：[[Python面向函数#模块]]】

### 内置方法

```python
# __init__ 实例化对象
def __init__(self, name, age):
	self.name = name
	self.age = age
	
# __new__ 创建对象时，自动调用；不会显示出来
object_one = Human(……) 
```


## 类的操作

### 查看属性
```python
class Human(object): 

	class_attribute_mind = 'clever' 
	def __init__(self, name, age): 
		self.name = name
		self.age = age
		
	def think(self): 
		return f'{self.name} is thinking'

vincent = Human('vincent', 20)
print(vincent.name)
print(vincent.__dict__)                       # 查看对象属性
print(Human.__dict__['class_attribute_mind']) # 查看类属性

# output
vincent
{'name': 'vincent', 'age': 20}
clever
```
通过`__dict__`可以查看，但不能修改属性：
```python
Human.__dict__['mind'] = 'smart'

# output
TypeError: 'mappingproxy' object does not support item assignment
```

#### 关注`__dict__`性质
```python
# __dict__ 查看对象/类的所有属性【数据】
print(object_one.__dict__)
```
`__dict__`没有括号，不属于内置方法，而是内置特殊属性，存储当前对象的命名空间字典
也就是说，数据能看，但不能直接改，否则就乱套了

### 修改属性
当然，如果要修改类属性，可以这样做：
```python
Human.class_attribute_mind = 'smart' # 修改属性
print(Human.class_attribute_mind)

# output
smart
```

对象修改属性也类似：
```python
vincent.name = "vicentPerp1" # 修改属性
del vincent.age              # 删除属性
print(vincent.__dict__)

# output
{'name': 'vicentPerp1'}
```

### 操作动态方法
```python
class Human(object): 

	class_attribute_mind = 'clever' 
	def __init__(self, name, age): 
		self.name = name
		self.age = age
		
	def think(self): 
		return f'{self.name} is thinking'

vincent = Human('vincent', 20)
print(Human.think(vincent))

```


# 封装「encapsulation」
