
#### 了解了最基本的装饰器原理后，进一步探索timer功能：
```python
import time

def func1():
	print('this is func1')
	
def timer(func1):
	def inner():
		start = time.time()
		func1()
		end = time.time()
		print(f'程序运行时间为：{end - start}')
	return inner
	
Try = timer(func1)
Try()
```
这是最基础的timer形式

#### 而在真实场景中，为了方便，有【语法糖】这个概念：
```python
import time
	
def timer(input_func):
	def inner():
		start = time.time()
		input_func()
		end = time.time()
		print(f'程序运行时间为：{end - start:.8f}')
		# 这里保留八位有效小数
	return inner

# 这就是语法糖 @timer等价于在定义func1函数之后执行func1 = timer(func1)
# @自己创建的新函数名称即可
@timer 
def func1():
	print('this is func1')

func1() # 这里不可用 Try()，在@timer下，只能用原名称 —— 这自然更清晰
```

#### 改成带参数的很简单，所以直接看携带多参数的
以下是错误代码：
```python
import time

def timer(func1):
	def inner(*args, **kwargs):
		start = time.time()
		func1(*args, **kwargs)
		end = time.time()	
		print(f'程序运行时间为：{end - start:.8f}')
	return inner

@timer
def func1(*args, **kwargs):
	print(args) # print(*args)会print(1,2,3),而不是一整个元组
	print(kwargs) # 注意：直接复制可能导致该函数体语句的缩进不一致

func1(1,2,3,name = 'vincent')
```

外部函数改为计数器也是一样的【教学案例】
```python
import time

def timer(input_func):
	def inner(*args, **kwargs):
		start = time.time()
		input_func(*args, **kwargs)
		end = time.time()	
		print(f'程序运行时间为：{end - start:.8f}')
	return inner
	
@timer
def add(x,y):
	print(f'和为：{x+y}')
	
add(1,2)
```


暂时不继续深挖，后续用到时再回过头按照教学文档补全，并进一步优化