

## 文件读取

### 两种存储地方：
1. 硬盘或SSD：长期保存数据，断电了也不会消失 「一般被编码，储存为字节」
2. 内存：也叫RAM( Random Access Memory 随机存取记忆) 保存程序运行时正在使用的数据

### 内存溢出风险：
不是“不能一次性读取整个文件”，而是“不能在不了解文件大小、可用内存的情况下，无限制地一次性读取”

对于以下代码，
```python
fileObject = open("first.md", "r", encoding = "utf-8")  
lines = fileObject.readlines()  
for line in lines:  
print(line, end= "")
```
是具有迷惑性的：看似是一行行读取文件内容，避开了内存溢出的风险；但实际过程为-先一行行读取内容，将内容全部储存在lines当中，此时这些内容已经全部保存在内存中，并不是进去一行，出去一行

而以下代码，
```python
with open("first.md", "r", encoding="utf-8") as fileObject:
    for line in fileObject:
        print(line, end="")
```
可以实现希望的功能

示例：通过 Python 读取存储在文本文件中的商品信息，并将其转换为易于操作的数据结构
[[Python示例·文件读取·Commodity Information Management and Total Price Calculation]]


## File.open() & File.close()

建立一个对象，将一个文件与该对象进行一个建立，然后规定查阅或修改该文件的方式，以及使用什么编码

也就是说，真正保存在硬盘里的文件，和在程序当中运行的这个file并不是同一个东西；所谓的open and close，只是所谓的程序的开始与结束，相当于是一种告示

当然具体的作用是比较复杂的，我也不理解，比如涉及到缓冲区等概念；

还涉及到文件游标，即读取是一个持续性的过程：两次file.read(2)并不会返回相同的内容，而是连续的两次不同的字节


## 文件游标、断点传续、seek()、分块读取

文件游标记录“读写进行到了哪里”；seek() 可以调整这个位置；分块读取控制“每次读取多少”。断点续传则会利用位置记录，从上次中断的地方继续传输

seek() 是调整文件位置的方法，不是专门的“断点续传函数”。 断点续传是它的一种应用场景

```python
with open("example.txt", "rb") as file:
    print(file.tell())      # 0

    data = file.read(2)

    print(data)             # b'AB'
    print(file.tell())      # 2
```
其中的file.tell()是让文件对象告诉我当前的位置