```python
products = []

with open('a.txt', 'r', encoding='utf-8') as file:
    for line in file:
        parts = line.strip().split()
        if len(parts) == 3:  
            product = {
                'name': parts[0],
                'price': int(parts[1]),  
                'amount': int(parts[2])  
            }
            products.append(product)

print("商品列表", products)

total_price = 0
for i in products:
    total_price += i['price'] * i['amount']

print("总价:", total_price)

# Output:
[{'name': 'apple', 'price': 10, 'amount': 3}, {'name': 'tesla', 'price': 100000, 'amount': 1}, {'name': 'mac', 'price': 3000, 'amount': 2}, {'name': 'lenovo', 'price': 30000, 'amount': 3}, {'name': 'chicken', 'price': 10, 'amount': 3}]
总价: 196060
```

本案例旨在通过 Python 读取存储在文本文件中的商品信息，并将其转换为易于操作的数据结构。具体目标包括：

1. 从文件 `a.txt` 中读取每一行的商品信息。
2. 将读取的信息构建为包含字典的列表，每个字典表示一个商品，包含名称、价格和数量。
3. 计算所有商品的总价，并输出结果

## Python理解要点：

1. with open 为Python的上下文管理器（context manager）语法，专门用来安全操作文件 —— 自动关闭文件
2. 安全性判断：如果分割后的长度等于三，才会把该内容存储进字典




## 疑点遗留
1. with open 自动关闭文件究竟有什么好处呢？
