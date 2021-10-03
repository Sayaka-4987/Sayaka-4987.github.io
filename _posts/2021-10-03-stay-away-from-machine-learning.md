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

机器学习基础知识，整理自 BJTU 专业选修课《机器学习》课程课件，感谢 lys 大佬的指点（

为了自用从 `.ipynb` 转成更方便的 `markdown` 格式……这个自动导出问题还挺多的，注释行必须多加个换行才能在博客正常显示



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

# 在下标(index)为1的地方插入一个字符串"new element2"

a.insert(1, "new element2") 
print('after insert an element:')
print(a)
```


```python
# 删除第二个元素，并返回

print("before delete the second element:")
print(a)

# 删除第二个元素，index为1，并返回，赋值给b

b = a.pop(1) 
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

一旦元组被生成，里面的元素就不可变了，不允许添加、删除以及修改里面的元素


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



但也有可变元组，原因请联想 Java 中的 String 类型；


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
a = (1, )
print(a)
print(type(a))
```


```python
b = (1)
print(b)
print(type(b))
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









# Part 2 Function and Control Sequence



## 1. 函数

python中的函数很简单，用 `def` 定义一个函数，函数体有四个空格作为缩进，在函数名后面加一对儿括号就可以调用该函数


```python
def test():
    print("test")
```


```python
test()
```




参数直接写在括号里面就行，不需要声明它的类型，返回值用 `return` 返回


```python
def sum_even(lista):
    '''
    求一个列表中偶数元素之和
    '''
    s = 0
    for i in lista:
        if i % 2 == 0:
            s += i
    return s
```


```python
sum_even([1,2,3,4,5,6,7,8,9,10])
```



python 中的函数可以返回多个值，这种时候返回的是一个 tuple


```python
def sum_odd_even(lista):
    '''
    求一个列表中奇数之和和偶数之和
    '''
    s_odd = 0
    s_even = 0
    for i in lista:
        if i % 2 == 0:
            s_even += i
        else:
            s_odd += i
    return s_odd, s_even
```


```python
sum_odd_even([1,2,3,4,5,6,7,8,9,10])
```



在python中定义函数的时候，由于不需要定义形参的类型，所以很多时候函数的参数是很灵活的，像上面的函数，我们不光可以传入list，还可以传入tuple和set，或是其他的可迭代类型


```python
sum_odd_even((1,2,3,4,5,6,7,8,9,10)) # 传入一个tuple
```


```python
sum_odd_even({1,2,3,4,5,6,7,8,9,10}) # 传入一个set
```


```python
sum_odd_even({1:"1", 2:"2"}) # 传入一个dict
```



需要注意的是，如果传入的参数是一个可变类型的变量，如果函数的功能对传入的这个可变类型的变量进行了修改，那么原来传入的这个值就会被改变


```python
def modify(lista):    
    '''    清空lista    '''  
    
    lista.clear()    
    print('clear list!')
```


```python
a = [1,2,3,4]
modify(a)
```

```python
clear list!
```



可以看到，此时的列表已经变成空的了，但是如果我们传入的是一个元组呢


```python
modify((1,2,3))
```



可以看到，元组是没有clear这个方法的，所以程序报了错。可以看到，**元组往往比列表更安全！**



### 默认参数

python中的函数在定义时，可以选择默认参数


```python
def test(num = 0):    '''    打印num这个变量，如果没有num传入，num默认为0    ''' 
    print(num)
```


```python
test()
```


```python
test(100)
```



需要注意一点，如果定义的这个函数有很多参数，那么 **默认参数一定要写在非默认参数的后面**


```python
def test(num = 0, n):    
    print(num + n)
```



上面的定义是有问题的，默认参数后面跟了非默认参数，应该改成下面的样子


```python
def test(n, num = 0):    
    print(n + num)
```


```python
test(1)
```



### 匿名函数

和其他大多数语言一样，Python 也提供了匿名函数的功能，语法如下：

```python
lambda param1, param2, ... : f(param1, param2, ...)
```



比如说定义一个 a+b 的函数：

```python
foo = lambda a, b : a + b
print(foo(2,3))
```



定义一个 sin(x) + cos(x) 的函数：

```python
import math
sin_cos = lambda x: math.sin(x) + math.cos(x)
print(sin_cos(1.5))
```


```python
print((lambda x : x**2+x)(10))  # x^2+x

foo = lambda f, x: f(x) + f(2*x)
print(foo(10))
```



## 2. 判断

python 中的 `if else` 没有花括号，使用 4 个空格作为缩进，具有相同缩进的代码块为一个整体。


```python
if 2 > 1:    
    print("2 > 1")    
    print("haha")
```


```python
if 2 < 1:    
    print("2 > 1")    
    print("haha")
else:    
    print("2 < 1")    
    print("heihei")
```

使用 `if elif else` 可以实现多种条件的判断，无需使用 `if else` 嵌套


```python
x = 1
if x<-1:    
    y = 2-x
elif 1 <= x <= 1/2:    
    y = -3 * x
else:    
    y = x - 2print(y)
```



## 3. 循环

python 中的循环有 for 和 while 



### `while` 循环

while 与其他语言的 while 差不多，只要 while 后面的条件满足，就一直执行下去，和 if 语句类似，具有相同缩进的代码块会被同时执行


```python
index = 0
s = 0

while index <= 100:    
    s += index
    index += 1
    print(s)
```



### `for` 循环

python中的for与c中的for有很大区别，python中的for循环的形式是：

```python
for i in iterable: 
```


其中，每循环一次，i 都会分别表示可迭代对象 iterable 中的每一项


```python
for i in [1,2,3,4,5,6,7]:    
    print(i)
```


```python
for i in [[1,2,3], [4,5,6], [7,8,9]]:    
    print(i)
```


```python
for i in [[1,2,3], [4,5,6], [7,8,9]]:    
    for j in i:        
        print(j, end = ' ')    
    print()
```



### `range()` 函数

python 中提供了一个 range 函数，这个函数接受 3 个参数，这三个参数与 list 的切片接受的参数差不多

1. 第一个参数代表起始值（不指定的时候默认为0）；
2. 第二个参数是必须传入的参数，代表终点，但是该值不会出现在迭代器中，迭代器中的最后一个数为它前面的一个数；
3. 第三个参数为步长，可以缺省，默认为1；


```python
range(5)
```


```python
for i in range(5):
    print(i)
```


```python
range(2, 5)
```


```python
for i in range(2, 5):
    print(i)
```


```python
range(1, 11, 2)
```


```python
for i in range(1, 11, 2):
    print(i)
```



例：使用 while 循环和 for 循环遍历下面的列表 numbers，并打印出奇数的个数


```python
import time
import random

numbers = [random.randint(-100, 100) for i in range(random.randint(20, 100))]
print('data built!')

# ------ start code ------

# for loop

for i in numbers:    
    if i % 2== 1:        
        print(i)
        
# while loop

i = 0
while True:    
    if i >= len(numbers):        
        break
    elif numbers[i] % 2 == 1:        
        print(numbers[i])
        
# ------ end code ------
```



## 4. 文件 IO

python中的文件读写很简单，打开文件用open函数，写文件使用文件对象的write方法，不论是读还是写，首先都要先打开一个文件，以下是一种写文件的方式，三种读文件的方法：


```python
f = open('test', 'w') # 打开一个叫做的test的文件，w表示写入(write)

f.write("test") # 写入字符串"test"

f.write("file") # 写入字符串"file"

f.write('\n') # 写入换行符\n

for i in range(10):
    f.write(str(i) + '\n') # 写入1到10
    
f.close() # 关闭文件
```


```python
f = open('test', 'r') # 使用r读文件(read)

text = f.read() # 使用read方法读文件

f.close()
print(text)
```


```python
f = open('test', 'r') # 使用r读文件(read)

text = f.readline() # 使用readline方法读文件的一行

f.close()
print(text)
```


```python
f = open('test', 'r') # 使用r读文件(read)

text = f.readlines() # 使用readlines方法读文件的每一行，放到列表中，并返回

f.close()
print(text)
```



### `try...finally` 异常处理

需要注意的是，如果在读取文件的过程中，或是在写文件的过程中出现bug，那么 `f.close()` 很有可能就不会被执行，这样会造成系统资源的浪费，而且可以打开的文件数量是有限的，所以一般我们使用 try...finally 来处理这个问题：


```python
try:    
    f = open('test', 'r')    
    text = f.readlines()    
    1 / 0
finally:    
    if f:        
        f.close()
```



用上面的这种写法，虽然会报错，但是文件确实是被关闭了，可以使用 `f.closed` 检查


```python
f.closed
```

可以看到 f.closed 为 True，这说明文件已经被关闭了；

但是上面的这种方法很不简洁，python提供了一个非常简单的写法：


```python
with open('test', 'r') as f:    
    test = f.read()    
    print(test)    
    print("Is the file closed?", f.closed)    
    1 / 0
```


```python
print("Is the file closed?", f.closed)
```



### 其他文件模式

可以看到，使用 `with open as f`，同样可以做到读写文件，而且这样写起来很简洁，只要把打开文件后要执行的语句，使用 4 个空格作为缩进写在一起即可。

值得注意的是，我们只使用了两种模式，r 和 w，事实上还有 rb 和 wb，分别是读取二进制文件和写入二进制文件，当我们写程序，下载一些东西的时候，比如图片格式，常使用 wb 保存一个图片；

python 还提供了一个模式 “a”，这个模式是追加用的，如果用a模式打开文件，那么可以直接用 write 方法写入新的信息，这些信息会被追加到文件的尾部。以上这几种是很常用的模式。

可以使用 help(open) 查看 open 的文档



例：尝试使用**追加**模式，在 test 这个文件后面加入任何你想加的字符串，然后打印整个文件的内容


```python
# ------ start code ------

with open('test', 'w+') as f:    
    f.write('fooooo')
with open('test', 'r') as f:    
    print(f.read()) 
# ------ end code ------
```



# Part 3 Numpy for Numerical Calculation

### 遇事不决查文档

[Numpy 文档](https://www.numpy.org/devdocs/reference/index.html) 

## numpy 基础

numpy 是第三方库，不是 python 自带的，需要自己安装，不过 anaconda 已经集成了numpy；

windows下安装很麻烦，需要安装 vs2015，或者使用 https://www.lfd.uci.edu/~gohlke/pythonlibs/ 这里提供的编译好的安装包进行安装；

linux/OSX 下保证有 gcc 和 g++ 即可；



### 导入外部包

语法： `import package as alias` ，其中 package 为包名，alias 为别名，不写 `as alias` 就是不取别名

如果是从包里导入一个子模块的话： `from package import sub_module`


```python
# 导入 numpy，简写为 np

import numpy as np
```

numpy 主要提供的是矩阵运算的功能，底层是 c 语言编写的，而且用了很多优化的策略，所以性能很强



### 创建矩阵


```python
# 创建一个一维向量

v = np.array([1, 2, 3, 4])
print(v)
print('type:', type(v))
print("v的维度:", v.shape)
```




可以看到维度为(4, )，v.shape 是一个 tuple，(4, )表示这是一个长度为4的一维向量


```python
# 创建一个二维矩阵

m = np.array([[1, 2],
              [3, 4]])
print(m)
print(type(m))
print(m.shape)
```



可以看到 m 的维度是 (2,2)，也就是一个 $2 \times 2$ 的矩阵


```python
m2 = np.array([[1], 
               [2],
               [3],
               [4]])
print(m2.shape)
```

    (4, 1)



### `reshape()` 函数

可以看到，m2 的维度是 (4, 1)，说明这是一个 4 行 1 列的矩阵，而v的维度是 (4, )，说明v是一个长度为4的一维向量，注意两者的区别；


```python
# 将m2变成一个1行4列的矩阵

m3 = m2.reshape(1, 4) 
print(m3)
print(m3.shape)
```

```python
[[1 2 3 4]](1, 4)
```


使用 reshape 可以对 array 的维度进行变换，会返回一个新的 array，不会影响该 array 原来的维度

在二维的 array 中，reshape 只要指定一个维度即可，另一个维度可以设置为 -1，让它自动计算；

比如，我们已经知道了 m3 是一个 1 行 4 列的矩阵，我们想将其变成一个1列的矩阵，有多少行让它自己计算，那么就使用 reshape(-1, 1)：


```python
m3.reshape(-1, 1)
```



### 取元素

在机器学习任务中，我们一般把行作为样本，也就是一行表示一个样本，列作为特征，也就是一列表示一个特征。假设我们有数据集 data，data 是一个 4行3列的数据集。


```python
data = np.random.uniform(0, 1, size = (4, 3))
print(data)
```

```python
[[0.3890509  0.07852005 0.10866473] [0.68119759 0.73162121 0.84347843] [0.92551516 0.79543677 0.49450627] [0.49077995 0.66073402 0.81769004]]
```



我们想取出第2行第2列的值：


```python
# 取第二行第二列

data[1, 1]  # 下标从0开始
```



这里使用切片的方法，这个切片和python的list的切片类似，忘了的可以回前面看一下。

切片中的第一个数字表示多少行，第二个数字表示多少列，下标从0开始，所以第二行第二列的值即为data[2, 2]：


```python
# 取第二行，第三列

data[1, 2]
```



在机器学习任务中，我们经常会取一个样本，或取一个特征，也就是在矩阵中，取一行或一列


```python
# 取出第一行

row_0 = data[0, :]
print(row_0)
print(row_0.shape)
```

    [0.3890509  0.07852005 0.10866473](3,)



使用切片的方法，切片中，第一个元素，0表示第一行，冒号 `:` 表示从第一个元素取到最后一个元素，这样就能把第0行，所有列的元素都取到。


```python
# 取出第一行

row_0 = data[0, 0: 3]
print(row_0)
print(row_0.shape)
```

    [0.3890509  0.07852005 0.10866473](3,)



上面这种方法和前面只用冒号得到的值一样


```python
# 取第一列

col_0 = data[:, 0]
print(col_0)
print(col_0.shape)
```

    [0.3890509  0.68119759 0.92551516 0.49077995](4,)



可以看到，不论是取行，还是取列，如果只取一行或一列，得到的值就会变成一个一维的 array。很多时候我们还想让他继续保持原来的维度，这时候就需要 reshape 一下


```python
# 取第一行，但还要保持二维

row_0_ = data[0, :].reshape(1, -1)
print(row_0_)
print(row_0_.shape)
```

    [[0.3890509  0.07852005 0.10866473]](1, 3)



```python
# 取第一列，但还要保持二维

col_0_ = data[0, :].reshape(-1, 1)
print(col_0_)
print(col_0_.shape)
```

    [[0.3890509 ] [0.07852005] [0.10866473]](3, 1)



### 加减法


```python
a = np.array([1,2])b = np.array([3,4])print(a, b)
```

    [1 2] [3 4]


a 和 b 都是一个长度为2的一维向量，可以直接相加减


```python
a + b
```


```python
a - b
```


```python
b - a
```


```python
c = np.array([[1, 2], [3, 4]])
d = np.array([[1, 1], [1, 1]])
print('c:', c)
print('-'*30)
print('d:', d)
```



c 和 d 是同维度的二维矩阵，也是可以直接相加减


```python
c + d
```


```python
c - d
```



### 数乘


```python
A = np.array([[1, 2, 3], [4, 5, 6]])
print(A)
```

```python
3 * A      # = array([[ 3,  6,  9], [12, 15, 18]])
```



同理，还有数加


```python
3 + A     # = array([[4, 5, 6], [7, 8, 9]])
```



### `np.dot()` 函数：矩阵乘法

我们知道，矩阵 $A \in \mathbb{R}^{m \times n}$ 和矩阵 $B \in \mathbb{R}^{n \times o}$ 是可以直接相乘的，因为前一个矩阵的列数等于后一个矩阵的行数：


```python
A = np.array([[1, 2, 3], [4, 5, 6]])
print(A)
```

```python
B = np.array([[1, 2],
              [3, 4],
              [5, 6]])
print(B)
```

```python
np.dot(A, B)    # array([[22, 28], [49, 64]])
```



### `.T` 矩阵转置

同理，我们将矩阵 $A$ 转置的话，就会变成一个3行2列的矩阵，将 $B$ 转置，变成2行3列，这两个矩阵也是可以相乘的


```python
A.T  # A的转置
```


```python
B.T  # B的转置
```


```python
np.dot(A.T, B.T)   # array([[ 9, 19, 29],       [12, 26, 40],       [15, 33, 51]])
```



### `*` 矩阵对应元素相乘

很多时候我们需要一种对应元素相乘的方法，$A \in \mathbb{R}^{m \times n}$，$B \in \mathbb{R}^{m \times n}$，定义一种乘法$\odot$，$$[A \odot B]_{ij} = [A]_{ij} * [B]_{ij} $$ 

这种乘法得到的积称为哈达玛积（Hadamard product），也称为 element-wise product，通俗来说就是两个维度相同的矩阵，他们的对应元素相乘：


```python
A = np.array([[1, 2, 3], [4, 5, 6]])
print(A)
```

```python
B = np.array([[1, 2, 3], [1, 2, 3]])
print(B)
```

```python
A * B   # = array([[ 1,  4,  9], [ 4, 10, 18]])
```

使用 `*` 直接将两个array相乘，即为两个矩阵的 hadamard product



### `sum()` 函数：求矩阵内元素之和


```python
A = np.array([[1, 2, 3], [4, 5, 6]])
print(A)
```

```python
A.sum()
```


```python
np.sum(A)
```



求一个矩阵每行的和：


```python
A.sum(axis = 1)
```


```python
np.sum(A, axis = 1)
```



求一个矩阵每列之和：


```python
A.sum(axis = 0)
```


```python
np.sum(A, axis = 0)
```



### `max()` 函数：求矩阵中最大的元素


```python
A.max()
```



求每行的最大元素：


```python
A.max(axis = 1)
```



求每列的最大元素：


```python
A.max(axis = 0)
```



### `min()` 函数：求最小值

### `mean()` 函数：求均值

### `std()` 函数：求标准差

### `np.exp()` 函数：对矩阵做指数运算

### `np.log` , `np.log2` , `np.log10` 函数：对矩阵做对数运算

例题：请你求出矩阵A的最小值，均值，标准差，每行的最小值，每列的最小值，每行的均值，每列的均值，每行的标准差，每列的标准差，看一看 np.exp(A) 和 np.log(A) 的效果


```python
## YOUR CODE HERE

np.min(A)
np.mean(A)
np.std(A)
np.min(A, axis=1)
np.min(A, axis=0)
np.mean(A, axis=1)
np.mean(A, axis=0)
np.std(A, axis=1)
np.std(A, axis=0)
np.exp(A)
np.log(A)
```



除此以外，np.exp，np.log，np.log2，np.log10等操作是可以对 python 中的 int 和 float 类型做运算的


```python
print(np.exp(1))
print(np.exp(2))
print(np.log2(2))
```




### `np.argmax()` 取出一个一维向量中最大值的下标

这是一个非常重要的操作，为什么这么说呢，在机器学习中，很多问题都是分类问题，也就是给你一张图片，上面有个数字，问你这个数字是几？很多算法会输出这个模型认为这个数字是0, 1, ..., 9的概率，也就是一个长度为10的一维向量。

这时候我们要取出概率最大的那个数所在的下标，这个下标就代表这个数字是几了。

比如，我们得到模型当前的输出了，输出为 output


```python
output = np.array([ 0.33847987,  0.7492099 ,  0.20938843,  0.53851897,  0.638118  ,        0.52182376,  0.98172993,  0.12160851,  0.5551554 ,  0.86638236])
```

可以看到，10个数，分别表示模型认为这个数字是0, 1, ..., 9的概率，我们要找数最大的那个的下标是几，使用 np.argmax 即可获取到最大值的下标，也就是0.9817对应的下标：


```python
np.argmax(output)
```



### `np.argmin()` 找最小值对应的下标

使用np.argmin找出output最小值的下标


```python
print(np.argmin(output))
```




同样，`argmax` 和 `argmin` 也支持 axis 这个参数


```python
A = np.array([[ 0.33847987,  0.7492099 ],       [ 0.20938843,  0.53851897],       [ 0.638118  ,  0.52182376],       [ 0.98172993,  0.12160851],       [ 0.5551554 ,  0.86638236]])
```



求矩阵每行最大值对应的下标：


```python
np.argmax(A, axis = 1)
```



求这个矩阵，每列最小值对应的下标


```python
np.argmin(A, axis=0)
```





### 广播（broadcast）

这是基础部分的最后一个概念，什么是广播呢，可以理解为复制，假设有两个矩阵$A \in \mathbb{R}^{1 \times 3}$，$B \in \mathbb{R}^{3 \times 3}$


```python
A = np.array([[1, 2, 3]])
print(A.shape)
```

```python
B = np.random.uniform(size = (3, 3))
print(B.shape)
```



在数学领域中，A+B是不能做的，因为两个矩阵的维度不一致，在 numpy 中，有一种机制叫做广播，让这两个维度不同的矩阵可以进行加减乘：每次操作的时候，numpy 将矩阵A复制了两份，变成了一个3行3列的矩阵，每行都是之前A的那一行，3行的值一样，然后再与B做操作，这样就得到了四种操作的值。


```python
A + B
```


```python
A - B
```


```python
B - A
```


```python
A * B
```



那么如果A是一个3行1列的矩阵，能和B做这四种操作吗？


```python
A = np.array([[1],
              [2],
              [3]])
print(A.shape)
## YOUR CODE HERE

A + B
```



广播的具体原理就是，如果两个矩阵都是二维的，那么只要有一个维度两个矩阵的值相等，那就会自动扩充另一维度，如果扩充，通过复制的方法，将当前的数据，沿着那个待扩充的维度不断的复制。

以上就是numpy的基础操作



## 综合练习题

完成一个很简单的机器学习预测任务！我们现在有一个特别简单的模型，它是 $$ \hat{y} = \frac{1}{1 + \exp^{-xw + b}}$$ 

我们现在有一个样本x，它有10个特征，这样的话它就是一个长度为10的一维向量。

在机器学习任务中，模型是对单个样本进行计算的，也就是说，我们有10个特征，那么参数W，有10个数，b有一个数。这样W乘以样本x，就是一个长度为10的向量，与另一个长度为10的向量进行乘积，得到一个长度为1的数，然后加上b，得到长度为1的这个数。在这个模型中，还需要对它取相反数，然后做指数运算，加1，取倒数，就得到了 $\hat{y}$ ，请你求出 $\hat{y}$ 是多少：


```python
x = np.array([ 0.78803502,  0.85090948,  0.17827904,  0.26081458,  0.61807529,        0.06409987,  0.70153396,  0.10446683,  0.52234655,  0.80166488])
```


```python
W = np.array([[ 0.02256906],
              [ 0.77558206],
              [ 0.29299293],
              [ 0.18633   ],
              [ 0.11959697],
              [ 0.20485966],
              [ 0.55220315],
              [ 0.1510716 ],
              [ 0.66596428],
              [ 0.29461207]])
```


```python
b = 0.39147861519281024
```



你的任务是求出 $\hat{y}$ 


```python
## YOUR CODE HERE
1/(np.exp(-np.sum(x*W.reshape(-1))+b)+1)
```

答案是0.81173994



但是在实际问题中，我们往往有上千个样本，X是我们的输入，是一个1000行，10列的矩阵，表示我们有1000个样本，10个特征，那么如何使用这个模型，对这1000个样本，求出他们的1000个值呢？

1. 使用for循环，将刚才对单个样本做预测的方法调用1000次即可
   这种方法理论上没有问题，但是在python中，for循环的速度要比c和java的for慢很多，计算的时间会很长
2. 直接使用矩阵进行运算
   $X \in \mathbb{R}^{1000 \times 10}$，$W \in \mathbb{R}^{10 \times 1}$
   $$X \times W \in \mathbb{R}^{1000 \times 1}$$
   可以看到，矩阵X乘以矩阵W得到一个1000×1的矩阵，这恰好就是将W，分别与矩阵X的每一行相乘，得到了1000个数，然后我们直接加上b，这样利用 numpy 的广播机制，b就会复制999次，将b与每一行相加，得到的还是这1000个值，然后取指数，加1，求倒数即可。所以，第二种方法中，只要将前面计算单个样本中的x换成X即可。而且速度更快



请你在下方完成这两种方法，计算出1000个输出值


```python
from time import time
```


```python
X = np.random.uniform(size = (1000, 10))
```


```python
# 使用for循环的计算方法

start_time = time()

# YOUR CODE HERE

[1/(np.exp(-np.sum(x*W.reshape(-1))+b)+1) for x in np.transpose(X)]

end_time = time()
print('time:', end_time - start_time)
```

```python
# 使用矩阵运算的计算方法

start_time = time()

# YOUR CODE HERE

1/(np.exp(-np.matmul(X, W) + b) + 1)

end_time = time()
print('time:', end_time - start_time)
```



# Part 4 Pandas for Data Analysis

## 遇事不决查文档

[Pandas文档](https://pandas.pydata.org/pandas-docs/version/0.24/index.html)

pandas 是一个非常好用的容器工具，构建在 numpy 之上，anaconda 已经集成。

pandas 在管理结构化数据上非常方便，底层是 numpy，所以性能很强劲，而且在处理时间序列数据时非常方便。

jupyter 对 pandas 的支持很好，可以直接显示 pandas 的数据结构，如 DataFrame


```python
import pandas as pd
```


```python
data = pd.read_csv('test_data.csv')
```

使用 `pd.read_csv` 即可读取 csv 文件，返回一个 pd.DataFrame



## `head()` 函数

使用 `data.head()` 就可以看到这个 dataframe 的前5行，可以使用 data.head(10) 看前10行

仔细观察的话，可以发现这个 dataframe，最上面一行是列名，左侧第一列是行的序号。列名倒是列名，但其实第一列不是行的序号，它可以是任意值，比如"a","b","c",这样的字符串，只不过在这个数据集中，它恰好为0，1，2...。我们称最左侧的这一列为索引(index)，可以通过它对行进行存取。一会儿在后面会说存取的问题。


```python
data.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>...</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>60</td>
      <td>RL</td>
      <td>65.0</td>
      <td>8450</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>208500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>20</td>
      <td>RL</td>
      <td>80.0</td>
      <td>9600</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>5</td>
      <td>2007</td>
      <td>WD</td>
      <td>Normal</td>
      <td>181500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>60</td>
      <td>RL</td>
      <td>68.0</td>
      <td>11250</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>9</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>70</td>
      <td>RL</td>
      <td>60.0</td>
      <td>9550</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2006</td>
      <td>WD</td>
      <td>Abnorml</td>
      <td>140000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>60</td>
      <td>RL</td>
      <td>84.0</td>
      <td>14260</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>12</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>250000</td>
    </tr>
  </tbody>
</table>



## `info()` 函数

`data.info()` 可以看到整个数据的全貌，1460 not-null 就表示有1460个非缺失值，object 表示这列是字符串类型的，int64 表示这列是 int 类型，float64 表示这列是 float 类型


```python
data.info()
```

```python
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1460 entries, 0 to 1459
Data columns (total 81 columns):
 #   Column         Non-Null Count  Dtype  
---  ------         --------------  -----  
 0   Id             1460 non-null   int64  
 1   MSSubClass     1460 non-null   int64  
 2   MSZoning       1460 non-null   object 
 3   LotFrontage    1201 non-null   float64
 4   LotArea        1460 non-null   int64  
 5   Street         1460 non-null   object 
 6   Alley          91 non-null     object 
 7   LotShape       1460 non-null   object 
 8   LandContour    1460 non-null   object 
 9   Utilities      1460 non-null   object 
 10  LotConfig      1460 non-null   object 
 11  LandSlope      1460 non-null   object 
 12  Neighborhood   1460 non-null   object 
 13  Condition1     1460 non-null   object 
 14  Condition2     1460 non-null   object 
 15  BldgType       1460 non-null   object 
 16  HouseStyle     1460 non-null   object 
 17  OverallQual    1460 non-null   int64  
 18  OverallCond    1460 non-null   int64  
 19  YearBuilt      1460 non-null   int64  
 20  YearRemodAdd   1460 non-null   int64  
 21  RoofStyle      1460 non-null   object 
 22  RoofMatl       1460 non-null   object 
 23  Exterior1st    1460 non-null   object 
 24  Exterior2nd    1460 non-null   object 
 25  MasVnrType     1452 non-null   object 
 26  MasVnrArea     1452 non-null   float64
 27  ExterQual      1460 non-null   object 
 28  ExterCond      1460 non-null   object 
 29  Foundation     1460 non-null   object 
 30  BsmtQual       1423 non-null   object 
 31  BsmtCond       1423 non-null   object 
 32  BsmtExposure   1422 non-null   object 
 33  BsmtFinType1   1423 non-null   object 
 34  BsmtFinSF1     1460 non-null   int64  
 35  BsmtFinType2   1422 non-null   object 
 36  BsmtFinSF2     1460 non-null   int64  
 37  BsmtUnfSF      1460 non-null   int64  
 38  TotalBsmtSF    1460 non-null   int64  
 39  Heating        1460 non-null   object 
 40  HeatingQC      1460 non-null   object 
 41  CentralAir     1460 non-null   object 
 42  Electrical     1459 non-null   object 
 43  1stFlrSF       1460 non-null   int64  
 44  2ndFlrSF       1460 non-null   int64  
 45  LowQualFinSF   1460 non-null   int64  
 46  GrLivArea      1460 non-null   int64  
 47  BsmtFullBath   1460 non-null   int64  
 48  BsmtHalfBath   1460 non-null   int64  
 49  FullBath       1460 non-null   int64  
 50  HalfBath       1460 non-null   int64  
 51  BedroomAbvGr   1460 non-null   int64  
 52  KitchenAbvGr   1460 non-null   int64  
 53  KitchenQual    1460 non-null   object 
 54  TotRmsAbvGrd   1460 non-null   int64  
 55  Functional     1460 non-null   object 
 56  Fireplaces     1460 non-null   int64  
 57  FireplaceQu    770 non-null    object 
 58  GarageType     1379 non-null   object 
 59  GarageYrBlt    1379 non-null   float64
 60  GarageFinish   1379 non-null   object 
 61  GarageCars     1460 non-null   int64  
 62  GarageArea     1460 non-null   int64  
 63  GarageQual     1379 non-null   object 
 64  GarageCond     1379 non-null   object 
 65  PavedDrive     1460 non-null   object 
 66  WoodDeckSF     1460 non-null   int64  
 67  OpenPorchSF    1460 non-null   int64  
 68  EnclosedPorch  1460 non-null   int64  
 69  3SsnPorch      1460 non-null   int64  
 70  ScreenPorch    1460 non-null   int64  
 71  PoolArea       1460 non-null   int64  
 72  PoolQC         7 non-null      object 
 73  Fence          281 non-null    object 
 74  MiscFeature    54 non-null     object 
 75  MiscVal        1460 non-null   int64  
 76  MoSold         1460 non-null   int64  
 77  YrSold         1460 non-null   int64  
 78  SaleType       1460 non-null   object 
 79  SaleCondition  1460 non-null   object 
 80  SalePrice      1460 non-null   int64  
dtypes: float64(3), int64(35), object(43)
memory usage: 924.0+ KB
```



使用 data[列明]取一列，返回的是 pd.Series，可以这样理解，Series 组成 DataFrame，DataFrame 中的行和列都是 Series，DataFrame 是二维的，Series 是一维的


```python
data['SalePrice'].head(10)  # Series也是支持head的
```


```python
0    208500
1    181500
2    223500
3    140000
4    250000
5    143000
6    307000
7    200000
8    129900
9    118000
Name: SalePrice, dtype: int64
```



## 取多列数据

还有一种取多列的方式，将要取的列，写到一个list中


```python
columns = ['LotFrontage', 'SalePrice']data[columns].head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LotFrontage</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>65.0</td>
      <td>208500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>80.0</td>
      <td>181500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>68.0</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>60.0</td>
      <td>140000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>84.0</td>
      <td>250000</td>
    </tr>
  </tbody>
</table>



### `.iloc` 截取行

第一行就用data.iloc[0]，第i行就用data.iloc[i-1]


```python
data.iloc[0]
```


```python
Id                    1
MSSubClass           60
MSZoning             RL
LotFrontage        65.0
LotArea            8450
                  ...  
MoSold                2
YrSold             2008
SaleType             WD
SaleCondition    Normal
SalePrice        208500
Name: 0, Length: 81, dtype: object
```



返回的也是一个Series，Series类似字典，可以用左侧的index取到右面的值，比如


```python
data.iloc[0]['SalePrice']
```


```python
data.iloc[0]['SaleType']
```



同样的道理，也可以先取列，再取行


```python
data['SalePrice'].iloc[0]
```



刚才不是说左侧的0，1，2是索引吗，所以 dataframe 也是可以通过索引来取值的；

用data.loc[index]来取行，可以看到，第一行的索引为0，所以可以通过 data.loc[0] 取到第0行。假设第一行的索引为"a"，那我们就可以用data.loc["a"] 取到第一行。


```python
data.loc[0]
```


```python
Id                    1
MSSubClass           60
MSZoning             RL
LotFrontage        65.0
LotArea            8450
                  ...  
MoSold                2
YrSold             2008
SaleType             WD
SaleCondition    Normal
SalePrice        208500
Name: 0, Length: 81, dtype: object
```



## `drop()` 函数

使用 `data.drop(列名, axis = 1, inplace = False)` 可以丢掉一列，inplace 表示是否在原地操作，如果为True，就会直接删除data中的这列，不会有返回值，如果为False，就会返回一个删除到该列的新的DataFrame，不改变原来的 DataFrame：


```python
data.drop('SalePrice', axis = 1, inplace = False).head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>...</th>
      <th>ScreenPorch</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>60</td>
      <td>RL</td>
      <td>65.0</td>
      <td>8450</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>20</td>
      <td>RL</td>
      <td>80.0</td>
      <td>9600</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>5</td>
      <td>2007</td>
      <td>WD</td>
      <td>Normal</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>60</td>
      <td>RL</td>
      <td>68.0</td>
      <td>11250</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>9</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>70</td>
      <td>RL</td>
      <td>60.0</td>
      <td>9550</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2006</td>
      <td>WD</td>
      <td>Abnorml</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>60</td>
      <td>RL</td>
      <td>84.0</td>
      <td>14260</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>12</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 80 columns</p>

如果drop的axis = 0，就可以删除行



## `describe()` 函数

使用data.describe()可以看到关于这个dataframe的描述，count表示有多少个非缺失值，mean表示均值，std标准差，min最小值，25%是第一四分位数，50%第二四分位数，75%第三四分位数，max表示最大值


```python
data.describe()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>MSSubClass</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>OverallQual</th>
      <th>OverallCond</th>
      <th>YearBuilt</th>
      <th>YearRemodAdd</th>
      <th>MasVnrArea</th>
      <th>BsmtFinSF1</th>
      <th>...</th>
      <th>WoodDeckSF</th>
      <th>OpenPorchSF</th>
      <th>EnclosedPorch</th>
      <th>3SsnPorch</th>
      <th>ScreenPorch</th>
      <th>PoolArea</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1201.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1452.000000</td>
      <td>1460.000000</td>
      <td>...</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>730.500000</td>
      <td>56.897260</td>
      <td>70.049958</td>
      <td>10516.828082</td>
      <td>6.099315</td>
      <td>5.575342</td>
      <td>1971.267808</td>
      <td>1984.865753</td>
      <td>103.685262</td>
      <td>443.639726</td>
      <td>...</td>
      <td>94.244521</td>
      <td>46.660274</td>
      <td>21.954110</td>
      <td>3.409589</td>
      <td>15.060959</td>
      <td>2.758904</td>
      <td>43.489041</td>
      <td>6.321918</td>
      <td>2007.815753</td>
      <td>180921.195890</td>
    </tr>
    <tr>
      <th>std</th>
      <td>421.610009</td>
      <td>42.300571</td>
      <td>24.284752</td>
      <td>9981.264932</td>
      <td>1.382997</td>
      <td>1.112799</td>
      <td>30.202904</td>
      <td>20.645407</td>
      <td>181.066207</td>
      <td>456.098091</td>
      <td>...</td>
      <td>125.338794</td>
      <td>66.256028</td>
      <td>61.119149</td>
      <td>29.317331</td>
      <td>55.757415</td>
      <td>40.177307</td>
      <td>496.123024</td>
      <td>2.703626</td>
      <td>1.328095</td>
      <td>79442.502883</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>20.000000</td>
      <td>21.000000</td>
      <td>1300.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1872.000000</td>
      <td>1950.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>2006.000000</td>
      <td>34900.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>365.750000</td>
      <td>20.000000</td>
      <td>59.000000</td>
      <td>7553.500000</td>
      <td>5.000000</td>
      <td>5.000000</td>
      <td>1954.000000</td>
      <td>1967.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>5.000000</td>
      <td>2007.000000</td>
      <td>129975.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>730.500000</td>
      <td>50.000000</td>
      <td>69.000000</td>
      <td>9478.500000</td>
      <td>6.000000</td>
      <td>5.000000</td>
      <td>1973.000000</td>
      <td>1994.000000</td>
      <td>0.000000</td>
      <td>383.500000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>25.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>6.000000</td>
      <td>2008.000000</td>
      <td>163000.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1095.250000</td>
      <td>70.000000</td>
      <td>80.000000</td>
      <td>11601.500000</td>
      <td>7.000000</td>
      <td>6.000000</td>
      <td>2000.000000</td>
      <td>2004.000000</td>
      <td>166.000000</td>
      <td>712.250000</td>
      <td>...</td>
      <td>168.000000</td>
      <td>68.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>8.000000</td>
      <td>2009.000000</td>
      <td>214000.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1460.000000</td>
      <td>190.000000</td>
      <td>313.000000</td>
      <td>215245.000000</td>
      <td>10.000000</td>
      <td>9.000000</td>
      <td>2010.000000</td>
      <td>2010.000000</td>
      <td>1600.000000</td>
      <td>5644.000000</td>
      <td>...</td>
      <td>857.000000</td>
      <td>547.000000</td>
      <td>552.000000</td>
      <td>508.000000</td>
      <td>480.000000</td>
      <td>738.000000</td>
      <td>15500.000000</td>
      <td>12.000000</td>
      <td>2010.000000</td>
      <td>755000.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 38 columns</p>



## `sum()` , `mean()` , `std()` , `max()` , `min()` 函数

和 numpy 类似，pandas 也支持这些方法

例：请你输出 data.sum(), data.mean(), data.std(), data.max(), data.min() 的结果


```python
# YOUR CODE HERE

data.sum()
data.mean()
data.std()
data.max()
data.min()
```


```python
Id                     1
MSSubClass            20
MSZoning         C (all)
LotFrontage         21.0
LotArea             1300
                  ...   
MoSold                 1
YrSold              2006
SaleType             COD
SaleCondition    Abnorml
SalePrice          34900
Length: 65, dtype: object
```



## 输出到文件中

使用 data.to_csv("文件名") 就可以将当前的dataframe保存为.csv文件，同理还有to_excel, to_json, to_dict等方法


```python
# 用.values转化为numpy

print(data.iloc[:5,:5].values)
```

```python
[[1 60 'RL' 65.0 8450]
 [2 20 'RL' 80.0 9600]
 [3 60 'RL' 68.0 11250]
 [4 70 'RL' 60.0 9550]
 [5 60 'RL' 84.0 14260]]
```


```python
# YOUR CODE HERE

data.to_csv('foo.csv')
```



# Part 5 Matplotlib for Visualization

[Matplotlib文档](https://matplotlib.org/3.1.1/index.html)

Matplotlib 是一个画图的库，和numpy，pandas可以无缝衔接，anaconda已经集成；

jupyter对matplotlib的支持很好，可以直接在jupyter中显示画出来的图：


```python
import matplotlib.pyplot as plt
%matplotlib inline
```

这样图就能在jupyter中直接显示；



**在 python 文件中就不要加了，会报错**



## `plt.figure()` 的 figsize 参数


```python
import numpy as np
```


```python
a = np.array([1, 2, 3, 4, 5, 6])
b = np.array([1, 2, 3, 4, 5, 6])
c = np.array([6, 5, 4, 3, 2, 1])
```


```python
plt.figure(figsize = (6, 6))
plt.plot(a, b, 'o')
```

plt.figure(figsize) 中，figsize 指定图的大小



尝试改变 figsize 为 (4, 4), (10, 6) 试试：


```python
# YOUR CODE HERE

plt.figure(figsize=(4,4))
plt.plot(a, b, 'o')

plt.figure(figsize=(10,6))
plt.plot(a, b, 'o')
```



## `plt.plot()` 的字符串参数

尝试将 `o` 改成 `o-` , `.` , `.-` 


```python
# o- 是比较大的点和线图

plt.plot(a, b, 'o-')
```


```python
# . 是散点图

plt.plot(a, b, '.')
```


```python
# .- 是点线图

plt.plot(a, b, '.-')
plt.show()
```



## `plt.plot()` 的颜色参数


```python
plt.figure(figsize = (8, 4))
plt.plot(a, b, '-', color = 'red')
```



尝试将color换成'blue', 'yellow', 'green'试试


```python
# blue

plt.figure(figsize = (8, 4))
plt.plot(a, b, '-', color = 'blue')
```


```python
# yellow

plt.figure(figsize = (8, 4))
plt.plot(a, b, '-', color = 'yellow')
```


```python
# green

plt.figure(figsize = (8, 4))
plt.plot(a, b, '-', color = 'green')
```



## `plt.xlabel()` , `plt.ylabel()` 增加 x,y 标签

## `plt.title()` 增加标题

使用 xlabel, ylabel, title 增加 x 标签，y 标签，标题：


```python
plt.figure(figsize = (8, 4))
plt.plot(a, b, '-', color = 'red')
plt.xlabel("x")
plt.ylabel("y")
plt.title("title")
```



## `plt.plot()` 对曲线增加 label 参数

还可以对曲线增加 label，然后显示图例，在 plot 函数中，加入 label 关键字，在最后加入 `plt.legend()` 即可显示出图例：


```python
plt.figure(figsize = (8, 4))
plt.plot(a, b, '-', color = 'red', label = 'training')
plt.plot(a, c, '-', color = 'blue', label = 'testing')
plt.xlabel("x")
plt.ylabel("y")
plt.title("title")
plt.legend()
```



`plt.legend()` 接受一个 loc 参数，值可以为：best , upper right , upper left , lower left , lower right , right , center left , center right , lower center , upper center , center  


```python
plt.figure(figsize = (8, 4))
plt.plot(a, b, '-', color = 'red', label = 'training')
plt.plot(a, c, '-', color = 'blue', label = 'testing')
plt.xlabel("x")
plt.ylabel("y")
plt.title("title")
plt.legend(loc = 'center')
```



## `plt.subplot()` 函数生成子图

matplotlib 可以在一张图中画多个子图，`plt.subplot` 里面有三个数：

1. 第一个数表示这个大图里面画多少行；
2. 第二个数表示这个大图里面画多少列；
3. 第三个数表示当前画的是第几个子图；

比如下面我们要画两个子图，在同一行，所以就是1行，2列，subplot前两个数字就是12。


```python
plt.figure(figsize = (10, 4))

plt.subplot(121)
plt.plot(a, b, '-', color = 'red', label = 'training')
plt.xlabel("x")
plt.ylabel("y")
plt.legend(loc = 'right')

plt.subplot(122)
plt.plot(a, c, '-', color = 'blue', label = 'testing')
plt.xlabel("x")
plt.ylabel("y")
plt.legend(loc = 'right')

plt.suptitle("title")
```



# Part 6 Scikit-learn for Machine Learning

[Scikit-learn文档](https://scikit-learn.org/stable/documentation.html)

scikit-learn 是python中最有名的机器学习库，构建在numpy, scipy, matplotlib之上，使用起来很简单，速度还很快，而且代码开源，可以在github上学习它的源码，学习各种机器学习算法的实现过程。

sklearn中有几个部分

1. clustering 包含了常用的聚类算法
2. datasets 包含了常见的开源数据集
3. ensemble 包含了常用的集成学习算法
4. linear_model 包含了常用的线性模型
5. metrics 包含了常用的指标
6. naive_bayes 包含了朴素贝叶斯相关的算法
7. neural_network 包含了神经网络相关的模型
8. svm 包含了支持向量机相关的算法
9. tree 包含了树模型相关的算法



我们的课程主要会涉及以上几个部分的使用

例：使用 sklearn 完成分类任务


```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

from matplotlib.colors import ListedColormap

import warnings
warnings.filterwarnings('ignore')
```



在这里引入我们需要使用的模型


```python
# 引入数据集

from sklearn.datasets import make_moons
```


```python
# 引入数据预处理工具

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
```


```python
# 引入模型

from sklearn.neural_network import MLPClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC
from sklearn.tree import DecisionTreeClassifier
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis
```


```python
# 引入评价指标

from sklearn.metrics import accuracy_score
from sklearn.metrics import precision_score
from sklearn.metrics import recall_score
from sklearn.metrics import f1_score
```



## 生成数据集

我们生成一个只有两个特征的数据集，所以可以在二维的平面上绘制出来

这个make_moons函数是sklearn.datasets中的函数，可以生成两个半月形的数据点


```python
X, y = make_moons(n_samples = 100, noise = 0.3, random_state=0)
```

X是数据，包含100个样本，两个特征，y是标记，两类


```python
y
```


```python
X.shape
```



对数据进行可视化，横轴是第一个特征，纵轴是第二个特征，颜色表示类别


```python
plt.figure(figsize = (8, 8))
cm_bright = ListedColormap(['#FF0000', '#0000FF'])
plt.scatter(X[:, 0], X[:, 1], c = y, cmap = cm_bright, edgecolors = 'k')
```

   


## 数据预处理

对数据进行标准化处理以及训练集和测试集的划分，随机选取了60%的样本作为训练，40%作为测试，所以我们会使用60个点进行训练，40个点进行测试


```python
X = StandardScaler().fit_transform(X)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=.4, random_state=42)
```

X_train表示用来训练的特征数据，y_train是其对应的标记

X_test表示用来测试的特征数据，y_test是其对应的标记



接下来对数据集进行可视化，不透明的样本为训练样本，半透明的为测试样本


```python
cm = plt.cm.RdBu
cm_bright = ListedColormap(['#FF0000', '#0000FF'])
plt.figure(figsize = (8, 8))
plt.title("Input data")

# Plot the training points
plt.scatter(X_train[:, 0], X_train[:, 1], c=y_train, cmap=cm_bright, edgecolors='k')

# and testing points
plt.scatter(X_test[:, 0], X_test[:, 1], c=y_test, cmap=cm_bright, alpha=0.3, edgecolors='k')
    
```



### 对数几率回归

对数几率回归又称逻辑回归，英文是Logistic regression，简称lr，是一个分类算法，常用于二分类任务，也可以处理多分类任务。在scikit-learn中，对数几率回归模型是LogisticRegression这个类，我们创建一个实例后，使用fit方法训练，需要的参数就是训练集的数据和标记。


```python
lr = LogisticRegression()
lr.fit(X_train, y_train)
```


```python
LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True,
          intercept_scaling=1, max_iter=100, multi_class='warn',
          n_jobs=None, penalty='l2', random_state=None, solver='warn',
          tol=0.0001, verbose=0, warm_start=False)
```



这样模型就训练完成了，接下来是预测


```python
prediction = lr.predict(X_test)   # 预测
```

可以看到，prediction就是模型对那40个测试样本的预测结果，而y_test就是他们的真值


```python
y_test
```



接下来是用各项评价指标来评价模型的预测能力，使用模型的预测值和真值就可以计算出模型的各项指标


```python
# 测试集精度

acc = accuracy_score(y_test, prediction)

# 测试集查准率

precision = precision_score(y_test, prediction)

# 测试集查全率

recall = recall_score(y_test, prediction)

# 测试集f1值

f1 = f1_score(y_test, prediction)

print('对数几率回归在该测试集上的四项指标：')
print('精度:', acc)
print('查准率:', precision)
print('查全率:', recall)
print('f1值:', f1)
```

```python
对数几率回归在该测试集上的四项指标：
精度: 0.875
查准率: 0.9
查全率: 0.8571428571428571
f1值: 0.8780487804878048
```


然后我们可以使用同样的方法，使用其他的模型来实验



### 计算混淆矩阵


```python
from sklearn.metrics import confusion_matrix
```


```python
confusion_matrix(y_test, prediction)
```



### 线性判别分析

请你尝试使用其他的模型，如线性判别分析 Linear Discriminant Analysis，完成模型的训练以及四项指标的计算


```python
model = LinearDiscriminantAnalysis()

# 模型的训练

model.fit(X_train, y_train)

# 模型的预测

prediction = model.predict(X_test)

# 计算四项指标

confusion_matrix(y_test, prediction)
```



## 可视化

接下来，我们做一些模型可视化的处理，直观的对比各个模型


```python
h = 0.02
x_min, x_max = X[:, 0].min() - .5, X[:, 0].max() + .5
y_min, y_max = X[:, 1].min() - .5, X[:, 1].max() + .5
xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
                     np.arange(y_min, y_max, h))
```



编写绘制模型预测结果的函数，这个函数会在图的右下角给出当前模型的预测精度，还会画出模型的决策边界


```python
def plot_model(model, title):
    model.fit(X_train, y_train)
    score_train = model.score(X_train, y_train)
    score_test = model.score(X_test, y_test)
    
    if hasattr(model, "decision_function"):
        Z = model.decision_function(np.c_[xx.ravel(), yy.ravel()])
    else:
        Z = model.predict_proba(np.c_[xx.ravel(), yy.ravel()])[:, 1]
        
    Z = Z.reshape(xx.shape)
    
    plt.figure(figsize = (14, 6))
    
    # 绘制训练集的子图
    
    plt.subplot(121)
    
    # 绘制决策边界
    
    plt.contourf(xx, yy, Z, cmap = cm, alpha=.8)

    # 绘制训练集的样本
    
    plt.scatter(X_train[:, 0], X_train[:, 1], c=y_train, cmap=cm_bright, edgecolors='k')
    
    # 设置图的上下左右界
    
    plt.xlim(xx.min(), xx.max())
    plt.ylim(yy.min(), yy.max())
    
    # 设置子图标题
    
    plt.title("training set")
    
    # 图的右下角写出在当前数据集中的精度
    
    plt.text(xx.max() - .3, yy.min() + .3, ('%.3f' % score_train).lstrip('0'), size=15, horizontalalignment='right')
    
    plt.subplot(122)
    plt.contourf(xx, yy, Z, cmap = cm, alpha=.8)
    
    plt.scatter(X_test[:, 0], X_test[:, 1], c=y_test, cmap=cm_bright, edgecolors='k', alpha=0.6)
    
    plt.xlim(xx.min(), xx.max())
    plt.ylim(yy.min(), yy.max())
    
    plt.title("testing set")

    plt.text(xx.max() - .3, yy.min() + .3, ('%.3f' % score_test).lstrip('0'), size=15, horizontalalignment='right')

    plt.suptitle(title)
```



## 对数几率回归

可以看到在右侧的测试集中，蓝色有三个点被模型判定为红色，红色有两个点被模型判定为蓝色，共5个样本被错误判断，所以精度是35 / 40，也就是87.5%


```python
plot_model(LogisticRegression(), 'Logistic Regression')
```





## 线性判别分析


```python
plot_model(LinearDiscriminantAnalysis(), 'Linear Discriminant Analysis')
```





## 决策树


```python
plot_model(DecisionTreeClassifier(max_depth = 5, random_state = 42), 'Decision Tree')
```





## 多层感知机


```python
plot_model(MLPClassifier(hidden_layer_sizes=(100, 100, 100), alpha=1), 'Multi-Layer Perception')
```





## SVM


```python
plot_model(SVC(kernel = "linear", C = 0.025, probability = True), 'Linear SVM')
```

















