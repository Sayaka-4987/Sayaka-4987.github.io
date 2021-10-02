---
layout:     post   				        # 使用的布局（不需要改）
title:      Python：机器学习基础知识 		# 标题 
subtitle:   我就不该选这门课 				# 副标题
date:       2021-10-03				    # 时间
author:     YXWang 					    # 作者
header-img: img/post-bg-keybord.jpg 	# 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - 机器学习
    - Python
---

机器学习基础知识，整理自 BJTU 专业选修课《机器学习》课程课件

为了自用从 .ipynb 转成更方便的 markdown 格式，感谢 lys 大佬的指点（



# Part 1 Python Basics and Data Structures



## 1. 基本类型

python是动态类型语言，不需要声明变量的类型；

常见的基本类型有 int, float, bool, NoneType, str；


```python
a = 1
b = 0.5
c = True
d = None
e = "String"
```



 `print()` 函数默认在打印内容结尾处换行：


```python
print(a)
print(b)
print(c)
print(d)
print(e)
```




 `print()` 函数支持多个参数，可用逗号隔开，打印结果也将被空格隔开


```python
print(a, b)
print(c, d, e)
```

```c
1 0.5
True None String
```



一个变量名可以被反复赋值，而且不限类型：


```python
a = 1
print(a)

a = 2
print(a)

a = None
print(a)
```

```c
1
2
None
```



###  `type()` 函数

可以查看变量类型：


```python
print(type(a))
print(type(b))
print(type(c))
print(type(d))
print(type(e))
```

```xml
<class 'NoneType'>
<class 'float'>
<class 'bool'>
<class 'NoneType'>
<class 'str'>
```



在 python3 中，int 类型和 float 类型是没有限制大小的，**长度只受限于机器的内存**


```python
a = 10000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
print(a)
print(type(a))
```

```xml
10000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
<class 'int'>
```



### 返回 float 类型的除法

int 和 float 类型支持加减乘除、乘方、求余等操作；

python3 中整数除法结果默认为 float 类型，想要得到整数值的除法需要用 `//` ：


```python
a = 3
b = 2
c = 1.5
print(a + b)  # 3 + 2
print(a + c)  # 3 + 1.5
print(a * c)  # 3 * 1.5
print(a / b)  # 3 / 2
print(a / c)  # 3 / 1.5
print(a // b) # 3 // 2
print(a**2)   # a^2
print(a**3)   # a^3
print(a % b)  # a % b
print(b % c)  # b % c
```



与、或、非 在python中分别为 `and`, `or`, `not`


```python
print(1 < 2 and 2 < 3)
print(1 < 2 and 2 > 3)
print(1 < 2 or 2 > 3)
print(1 > 2 or 2 > 3)
print(not 1 > 2)
```




## 2. 列表（list）

list 是 python 中最简单，最基础的容器类型，它的特征是一对中括号 `[ ]`；
list 中的元素可以是同类型的，也可以是不同类型的：


```python
# list的初始化
a = []
print(a)

a = list()
print(a)

a = [1, 2, 3]
print(a)

a = [1, 2, None, True, "String"]
print(a)
```




在不考虑内存大小的情况下，list 内的空间是无限大的，我们使用 `len()` 函数，获取一个列表的长度


```python
print(len(a))      # 列表a的长度

print(len([]))     # 空列表的长度

print(len(list())) # 空列表的长度
```



使用 `list[index]` 访问第 index+1 个元素：


```python
print(a)
print(a[0])
print(a[1])
print(a[2])
print(a[3])
print(a[4])
print(a[5]) # 越界
print(1 in a) # 1是否在列表a中
print("haha" in a) # "haha"是否在列表a中
```



在 python 中，可以对列表进行切片(slice)，截取多个元素；

```python
list[start: end: step_size] 
```



需要注意的是，切片实际上是 **左闭右开** 区间，即 start 指向的元素会被选中，end指向的元素不会被选中；

步长（step_size）不指定时，默认为1，即从 start 到 end-1 每个元素都被选中，放入新列表中返回


```python
print(a)

print(a[:])    # 不指定start和end时，默认为0和len(a)，即取列表中的所有元素，并返回一个新列表

print(a[0:])   # 从第1个元素到最后一个元素

print(a[2:])   # 从第3个元素到最后一个元素

print(a[1:5])  # 从第2个元素到第5个元素(index为4)

print(a[:-1])  # 从第1个元素到倒数第2个元素

print(a[:-3])  # 从第1个元素到倒数第4个元素

print(a[::1])  # 从第1个元素到最后一个元素，每个元素都取

print(a[::2])  # 从第1个元素到最后一个元素，每隔1个元素取一次

print(a[1::2]) # 从第2个元素到最有一个元素，每隔1个元素取一次
```



### 列表生成式

格式为 **[*expr* for *element* in *list*]** ；

其中 *expr* 为表达式(可以是对 *element* 的操作也可以和 *element* 无关)，*element* 为循环中的每一个元素，*list* 为列表(实际上任何可迭代对象都可以)，比如说要生成从0到9每个数字的平方，可以用：

```python
result = [i**2 for i in range(10)]

print([i**2 for i in range(10)]) # 1到9每个数字的平方

print([type(x) for x in a])      # a中每个元素的类型
```



### 取元素时候 index 也可以为负数  

尝试取列表a中的倒数第3个元素，将其存入b中


```python
a = [1, 2, None, True, "String"]

# 尝试取列表a中的倒数第3个元素，将其存入b中

# ------start code------

b = a[-3]

# ------end code------

print(b)
```



列表是一种类型，是 python 内置的类型，该类型有很多常用的方法(method)

比如：添加元素，删除元素，对列表进行排序，查找一个值第一次出现的位置：


```python
# 在列表的末尾插入一个元素

print('before append an element:')

print(a)

a.append("new element")

print('after append an element:')

print(a)
```


```python
# 在列表的任意位置插入一个元素

print('before insert an element:')

print(a)

a.insert(1, "new element2") # 在下标(index)为1的地方插入一个字符串"new element2"

print('after insert an element:')

print(a)
```


```python
# 删除第二个元素，并返回

print("before delete the second element:")

print(a)

b = a.pop(1) # 删除第二个元素，index为1，并返回，赋值给b

print("after delete the second element:")

print(a)

print("the second element in list a before delete:", b)
```


```python
# 删除第一次出现的一个元素

a = [1, 2, 3, 1, 2]

print(a)

a.remove(1) # 删除从左到右第一个出现的1

print(a)

a.remove(1) # 删除从左到右第一个出现的1

print(a)
```


```python
# 对列表进行原地排序

a = [5, 3, 1, 4, 7, 3, 9, 0]

a.sort() # 对a进行从小打大排序

print(a)

a.sort(reverse = True) # 对a进行从大到小排序

print(a)
```


```python
# 查找第一次出现的一个值

a = [1, 2, 3, 4, 1, 2, 3, 4]

print(a.index(2)) # 打印出从左到右，2这个元素第一次出现的index值
```



### `help()` 函数

查看对应文档：


```python
help(list) # 使用help函数，查看list的文档
```



### 魔术方法（magic methods）

方法名前后有两个下划线的，称为魔术方法，属于 python 的高级技巧  

list里的其他方法：  

1. `a.clear()` 清空列表  
2. `a.copy()` 复制当前列表并返回  
3. `a.count(b)` 返回列表 a 中指定元素 b 的出现次数  
4. `a.extend(b)`  b 是一个列表，将 b 中的所有元素添加到 a 中  
5. `a.reverse()` 将 a 中的元素原地前后转置  




```python
a = [1, 2, 3, 4, 5]

# 在下方使用clear方法

# ------start code------

a.clear()

# ------end code------

print(a)
```


```python
a = [1, 2, 3, 4, 5]

# 在下方使用copy方法，复制给b

# ------start code------

b = a.copy()

# ------end code------

print(b)
```


```python
a = [1, 2, 3, 4, 5, 1, 3, 6, 2, 4, 1, 4, 2, 6, 8, 2]

# 在下方使用count方法，数出列表a内2的个数

# ------start code------

number = a.count(2)

# ------end code------

print(number)
```


```python
a = [1, 2, 3, 4, 5]

b = [6, 2, 4, 1, 6, 0]

# 在下方使用extend方法，将b中的元素扩展到a中

# ------start code------

a.extend(b)

# ------end code------

print(a)

print(b)
```


```python
a = [1, 2, 3, 4, 5, 1, 3, 6, 2, 4, 1, 4, 2, 6, 8, 2]

# 在下方使用reverse方法，将a中的元素转置

# ------start code------

a.reverse()

# ------end code------

print(a)
```



### `max()` 和 `min()` 函数

`max()` 用来求一个可迭代对象中的最大值，如 max(a) ；`min() `用来求一个可迭代对象中的最小值，如 min(a) ：


```python
a = [1, 2, 3, 4, 5, 1, 3, 6, 2, 4, 1, 4, 2, 6, 8, 2]

# 在下方使用max函数，将a中的最大值存到max_value中，使用min函数，将最小值存到min_value中

# ------start code------

max_value = a.max()

min_value = a.min()

# ------end code------

print(max_value)

print(min_value)
```



## 3. 元组（tuple）

元组是一种不可变的类型，它和 list 类似，但是 list 是可变的，tuple不可变，标志是 `()` ：


```python
# 初始化一个tuple

a = ()

print(a)

print(type(a))

```


```python
a = tuple()

print(a)
```


```python
# 初始化一个有元素的tuple

a = (1, 2, 3)

print(a)
```


```python
# 初始化一个仅有一个元素的tuple

a = (1, )

print(a)
```


```python
# 初始化一个元素的tuple时，如果不加逗号， python 会将小括号理解成数值运算里的括号

a = (1)

print(a)

print(type(a))

```



和 list 类似，a[index] 来访问元组 a 中的元素


```python
a = (1, 2, 3)

print(a[0])

print(a[1])

print(a[2])
```



同样，tuple 也是可以用切片取多个元素的


```python
a = (1, 2, 3, 4, 5, 6, 7, 8, 9)

print(a[::2])   # 取index为偶数的元素

print(a[1::2])  # 取index为奇数的元素

print(a[:])     # 取所有的元素

print(a[:-5])   # 取第一个元素到倒数第六个元素
```



由于元组不可变，没有 list 那样的 sort 和 reverse 方法了



### `sorted()` 函数

列表的 `sort()` 方法可以就地排序，但是调用这个方法后，并没有返回值；

python 还提供了一个 `sorted()` 函数，可以将一个可迭代对象内的元素排序后放到一个新 list 中返回


```python
a = [5, 2, 3, 1, 9, 4, 8]

b = sorted(a)

print(b)

c = sorted(a, reverse = True)

print(c)
```



请尝试对下面的元组a调用sorted函数，将a按从小到大排序的结果存入b中，将a按从大到小排序的结果存入c中


```python
# test1

a = (5, 2, 3, 1, 9, 4, 8)

# ------ start code ------

b = sorted(a)
c = sorted(a, reverse=True)

# ------ end code ------

print(b)
print(c)
```



### `sum()` 函数 

python 中还内置了一个sum函数，用于求一个容器内元素的和：


```python
help(sum)
```


```python
a = (1.1, 2.2, 3.3, 4.4)

b = sum(a)

print(b)
```



但是如果要使用 sum 函数对某一个可迭代对象求和，这个可迭代对象里必须都是数值型的类型，比如 int 和 float，如果出现了其他类型，则会报错


```python
a = (1, "list", 2, "tuple")

print(sum(a))
```



请使用**切片(slice)**将元组a中index为偶数的元素取出，并将他们的和存入summation中


```python
a = (1, "list", 2, "tuple", 3, "dict", 4, "set")

# ------ start code ------

summation = a[::2]

# ------ end code ------

print(summation)
```



### `len()` 函数

 python 自身并没有求一个容器平均值的函数，不过 python 提供了**求和**与**求长度（元素个数）**的函数，请使用这两个函数求下面元组a中元素的平均值


```python
a = (1, 2, 3, 4, 5, 6)

# ------ start code ------

average = a.sum()/len(a)

# ------ end code ------

print(average)
```



### 强制类型转换

 python 与其他语言一样，提供了强制类型转换的**函数**，常见的有int, float, str, list, tuple


```python
# 将a转换为浮点型

a = "3.14"
b = float(a)
print(b, type(b))
```


```python
# 将a转换为整型

a = "3"
b = int(a)
print(b, type(b))
```


```python
# 将a转换为整型

a = "1"
b = int(a)
print(b, type(b))
```


```python
# 将a转换为整型

a = 3.99
b = int(a)
print(b, type(b))
```



需要注意的是，int这个函数，可以将float（小数）转换为整数，但是它会向着趋近于0的方向取整，所以即便是3.99，也会被转换为3



请将下面的字符串a，转换为整型(int)，存入b中


```python
a = "-3.14"

# ------ start code ------

b = int(float(a))

# ------ end code ------

print(b)
```



同样的，list与tuple也可作为强制类型转换的函数


```python
a = (1, 2, 3)

b = list(a)

print(b, type(b))
```


```python
a = [1, 2, 3]

b = tuple(a)

print(b, type(b))
```



### tuple 的不可变

什么是可变，什么是不可变，tuple的不可变体现在哪里？


```python
a = [1, 2, 3, 4]

a[0] = 5

print(a)
```


```python
a = (1, 2, 3, 4)

a[0] = 5

print(a)
```



一旦元组被生成，里面的元素就不可变了，不允许添加、删除以及修改里面的元素, **但是**


```python
a = [1, 2, 3]

b = (a, 1, 2)

print(b)
```


```python
a.append(4)

print(a)
```


```python
# 打印b试试

# ------ start code ------

print(b)

# ------ end code ------
```

原因请联想 Java 中的 String 类型；



## 4. 字符串

 python 中的字符串很强大，内置了多种方法，字符串的特征是 `" "` , `' '` , `''' '''` , `""" """`  


```python
a = 'Tom'

b = "Tom"

print(a == b)
```



单引号和双引号没有什么区别，三个引号用于多行的字符串


```python
a = '''TomandJack'''

print(a)
```


```python
a = 'TomandJack'
```



### `+` 拼接

```python
print('foo ' + ' bar')
```



### `str.strip()` 函数：去除空格

```python
print('  ver  bo  se '.strip())
```



###  `"%d %f %s"%(int1, float1, string1)` 格式化输出

```python
print('int: %d, float: %f, string: %s'%(1, 3.14, 'my god'))
```



### `str.upper(), str.lower()` 函数：大小写转换


```python
print('Big Small'.upper(), 'Big Small'.lower())
```



### `len()` 函数

可以查看字符串的长度


```python
len("Tom")
```



### `startswith()`, `endswith()` 函数

字符串有两个常用方法，一个是 startswith，另一个是 endswith，前者用于判断一个字符串是否以某一个字符串为开头，后者用于判断是否以某一字符串为结尾：


```python
print(help(str.startswith))
```


```python
print(help(str.endswith))
```


```python
a = "Tom and Jarry"print(a.startswith('Tom'))print(a.endswith("Jarry"))
```



### `split()` 函数

`split()` 用于分割字符串，返回一个 list：


```python
print(help(str.split)) 

a = "Tom and Jarry"

print(a.split(' '))
```



### `replace()` 函数

`replace()` 用于替换字符串中的子字符串，将替换后的字符串返回，不会影响原字符串


```python
a = "Tom and Jarry"

b = a.replace('Tom', 'Jack')

print(b)

print(a)
```



### `strip()` 函数

这个方法常用于读文件，或是清理爬虫抓取的内容，很多时候我们读的文件，结尾可能有很多没有用的换行符或空格，如果不处理的话容易引发bug，`strip()` 可以清除字符串首尾两侧的空格或换行符等


```python
a = " Tom and Jarry    \n"

b = a.strip()

b
```



在此需要提一句，在 Jupyter notebook 的交互式环境下，即使不用 print 也可以打印一个变量的值，直接输入变量名即可；但是这种方法与print函数打印出的内容不同，print 函数会自动解析换行符等字符，但是直接使用变量名显示变量值的方法却不能：


```python
a = " Tom and Jarry    \n"

a
```


```python
a = " Tom and Jarry    \n"

print(a)
```



### `join() `函数

这个方法可以将一个全是字符串的可迭代对象，如列表，将里面的每个元素之间，插入你想要插入的元素，返回一个新的字符串


```python
print(help(str.join))
```


```python
a = ['Tom', 'Jack', "Jarry"]

print('\n'.join(a))
```


```python
a = ['Tom', 'Jack', "Jarry"]

print(', '.join(a))
```



## 5. 字典（dict）

python 中的 dict(字典) 是非常强大的数据结构，由键值对组成，一个字典中键只允许有一个。类似其他语言中的map，标志是 `{ }`


```python
# 初始化

a = {}

print(a)

type(a)
```


```python
# 初始化

a = dict()

print(a)

type(a)
```


```python
# 初始化

a = {'Name': 'David'}

print(a)

type(a)
```



使用 `[]` 访问字典中的元素


```python
print(a['Name'])
```



也可以用 `[]` 赋值：


```python
a['Id'] = 123
```


```python
print(a)
```


```python
a['Name'] = 'Jack'print(a)
```



可以用 `in` 判断一个元素是否是该字典中的一个键


```python
'Id' in a
```


```python
'Name' in a
```


```python
'Age' in a
```



### `.keys()` 和 `.values()`

获取所有的键和所有的值：


```python
a.keys()
```


```python
a.values()
```


```python
for i in a.keys():    
    print(i)
```


```python
for i in a.values():    
    print(i)
```



### `.items()` 获得所有的键值对


```python
for i in a.items():    
    print(i)
```

可以看到.items()返回的是一个可迭代的对象，里面每个元素都是一个元组。



在 python 中，给多个变量同时赋值有个小技巧


```python
x, y = 1, 2
```



这实际上是利用了元组的特性，上面的 1,2 组成了一个只有两个元素的元组，可以对两个变量分别赋值，可以利用这个特性交换两个元素：


```python
print(x, y)

x, y = y, x

print(x, y)
```



如果我们要用for循环遍历一个这样的列表[(1, 2), [3, 4], (5, 6), (7, 8)]，也可以使用两个变量分别表示里面的两个元素


```python
t = [(1, 2), [3, 4], (5, 6), (7, 8)]

for x, y in t:
    print('x:', x, ',', 'y:', y)
```



所以我们可以使用这个方法对字典的items方法的返回值进行遍历，即一个变量为键，另一个变量为值


```python
for x, y in a.items():
    print('key:%s, value:%s'%(x, y))
```



上面的打印用了一个小技巧，使用%s占位，在字符串结束后，加上%()，括号里面填写要替换%s的元素


```python
'%s and %s'%("Tom", "Jarry")
```



如果我们要使用for循环对一个字典遍历，会出现什么情况？


```python
for i in a:    
    print(i)
```



可以看到，遍历的是字典的键，所以遍历字典其实还有一种方式，就是遍历它的键，然后用中括号 `[]` 访问值：


```python
for i in a:
    print('key: %s, value: %s'%(i, a[i]))
```



但是不推荐这种方法，如果我们需要同时遍历键和值的话，推荐使用.items()，因为使用上面的这个方法的话 python 会计算a[i]的值，也就是它会去计算i对应的值是多少，如果数据量大的话，这个计算时间是不容忽视的。

python 字典背后的结构是Hash table.



## 6. 集合（set）

 python 中还有一个比较常用的结构，set（集合）


```python
# 初始化

a = set()
print(a)
print(type(a))
```


```python
# 初始化

a = {1,2,3}
print(a)
print(type(a))
```


```python
# 初始化

a = {1, }
print(a)
print(type(a))
```


```python
# 初始化

a = {1}
print(a)
print(type(a))
```



### `set()` 函数

将一个可迭代的对象转换为集合


```python
# 初始化

a = [1,2,3]
print(set(a))
```



讲到这里就会提到tuple(元组)的初始化，一般使用括号初始化一个元组，但如果元组中只有一个元素，此时必须写上逗号


```python
a = (1, )print(a)print(type(a))
```


```python
b = (1)print(b)print(type(b))
```



### `add()` 函数

添加元素:


```python
a = {1,2,3}
a.add(4)
print(a)
```


```python
a.add(1)
print(a)
```



可以看到，当往集合内添加一个已经存在的元素时，不会报错。


```python
a.remove(1)
print(a)
```

列表和元组的 `remove()` , `len()` , `sum()` , `max()` , `min()` , `sorted()` 函数依然可用；



### 求交集、并集、差集、对称差集


```python
# 交集

a = {1, 2, 3}
b = {1, 2, 4}
print(a.intersection(b))
print(a & b)
```


```python
# 并集

a = {1, 2, 3}
b = {1, 2, 4}
print(a.union(b))
print(a | b)
```


```python
# 差集

a = {1, 2, 3}
b = {1, 2, 4}
print(a.difference(b))
print(a - b)

print(b.difference(a))
print(b - a)
```


```python
# 对称差集

a = {1, 2, 3}
b = {1, 2, 4}
print(a.symmetric_difference(b))
print(a ^ b)
```

还有一些常用的方法，比如 clear，update 等，可以通过 help(set) 查看文档



例题：给定一个列表，请检查出a里面不重复元素的个数


```python
import random
temp = [random.randint(0, 1000) for i in range(1000)]
a = [random.choice(temp) for i in range(1000)]
```


```python
# ------ start code ------

print([([0]+a)[i] - (a+[0])[i] for i in range(len(a))].count(0))

# ------ end code ------
```























