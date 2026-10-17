
## 变量：

有两种分类方式，
1.强类型、弱类型
2.动态类型、静态类型



## 循环


```python
while True:
	print('vincent')
```
死循环，极高风险

一般都会在内部设置一个`break`条件
```python
count = 0
while True:
	print('vincent')
	count += 1
	if count == 10:
		print(f'循环次数为：{count}次')
		break
```


# 一类东西
`__class__` / `__iter__` / `__next__`
我很好奇这类东西到底是什么？



# print
