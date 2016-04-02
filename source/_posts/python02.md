title: "python学习笔记02"

date: 2015-12-28 20:06:30

tags: [python]

categories: [python]

---

# python学习笔记02 *[2015-12-28]*


## 编写函数
﻿

- 在Python中，定义一个函数要使用 def 语句，依次写出函数名、括号、括号中的参数和冒号:，然后，在缩进块中编写函数体，函数的返回值用 return 语句返回。

### 任务 请定义一个 square_of_sum 函数，它接受一个list，返回list中每个元素平方的和。

    def square_of_sum(L):

        sum = 0

        for i in L:

            sum += i * i

        return sum



    print square_of_sum([1, 2, 3, 4, 5])

    print square_of_sum([-5, 0, 5, 15, 25])


<!--more--> 

## 递归函数

### 任务 汉诺塔 (http://baike.baidu.com/view/191666.htm) 的移动也可以看做是递归函数。

```
我们对柱子编号为a, b, c，将所有圆盘从a移到c可以描述为：

如果a只有一个圆盘，可以直接移动到c；

如果a有N个圆盘，可以看成a有1个圆盘（底盘） + (N-1)个圆盘，首先需要把 (N-1) 个圆盘移动到 b，然后，将 a的最后一个圆盘移动到c，再将b的(N-1)个圆盘移动到c。

请编写一个函数，给定输入 n, a, b, c，打印出移动的步骤：

move(n, a, b, c)

例如，输入 move(2, 'A', 'B', 'C')，打印出：

A --> B
A --> C
B --> C

```

    def move(n, a, b, c):

        if n==1:

            print a, "-->", c

            return

        move(n-1, a, c, b)

        print a,"-->",c

        move(n-1, b , a, c)







## 定义默认参数

### 任务-请定义一个 greet() 函数，它包含一个默认参数，如果没有传入，打印 'Hello, world.'，如果传入，打印 'Hello, xxx.'




    def greet(b='world'):

        

        print 'hello, '+b+'.'



    greet()

    greet('Bart')



## 定义多个参数

     def fn(*args):
     print args
- 假设我们要计算任意个数的平均值，就可以定义一个可变参数：

    def average(*args):
        sum = 0.0
        if len(args)==0:
            return sum
        else:
            for i in args:
                sum+=i
        return sum / len(args)
        

    print average()
    print average(1, 2)
    print average(1, 2, 2, 3, 4)

## range()函数可以创建一个数列：
    range(1, 101)
    [1, 2, 3, ..., 100]

## slice对list进行切片
-    对应上面的问题，取前3个元素，用一行代码就可以完成切片：
    >>> L[0:3]

-   如果第一个索引是0，还可以省略：

    >>> L[:3]

-   也可以从索引1开始，取出2个元素出来：
    >>> L[1:3]

-   只用一个 : ，表示从头到尾：
    >>> L[:]

-   切片操作还可以指定第三个参数：
    >>> L[::2]
    ['Adam', 'Bart']第三个参数表示每N个取一个，上面的 L[::2] 会每两个元素取出一个来，也就是隔一个取一个。{第三个参数每隔N个数取第一个数}

-   字符串有个方法 upper() 可以把字符变成大写字母：
    >>> 'abc'.upper()

### 任务：请设计一个函数，它接受一个字符串，然后返回一个仅首字母变成大写的字符串。
    提示：利用切片操作简化字符串操作。
    def firstCharUpper(s):
        return s[:1].upper()+s[1:]
    print firstCharUpper('hello')
    print firstCharUpper('sunday')
    print firstCharUpper('september')

## 迭代
    集合是指包含一组元素的数据结构，我们已经介绍的包括：
    1. 有序集合：list，tuple，str和unicode；
    2. 无序集合：set
    3. 无序集合并且具有 key-value 对：dict
### 任务-请用for循环迭代数列 1-100 并打印出7的倍数。
```
//第一种实现
for i in range(1,101):
    if(i%7==0):
        print(i)
```   
```   
//第二种实现  
for i in range(1, 101)[6::7]:

    print i
```
```   
//第三种实现 ,range有步长功能，不需要切片 
for i in range(1, 101,7):

    print i
```

## 索引迭代
- Python中，迭代永远是取出元素本身，而非元素的索引。
    enum..
    使用 enumerate() 函数，我们可以在for循环中同时绑定索引index和元素name。但是，这不是 enumerate() 的特殊语法。实际上，enumerate() 函数把：
    ['Adam', 'Lisa', 'Bart', 'Paul']变成了类似：
    [(0, 'Adam'), (1, 'Lisa'), (2, 'Bart'), (3, 'Paul')]

### 任务 zip()函数可以把两个 list 变成一个 list：
>>> zip([10, 20, 30], ['A', 'B', 'C'])
[(10, 'A'), (20, 'B'), (30, 'C')]在迭代 ['Adam', 'Lisa', 'Bart', 'Paul'] 时，如果我们想打印出名次 - 名字（名次从1开始)，请考虑如何在迭代中打印出来。

```
    L = ['Adam', 'Lisa', 'Bart', 'Paul']
    for index, name in zip(range(1,6),L):
        print index, '-', name
    
```
 
 
## 迭代dict的value  

```
     ① d.values
     d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59 }

     for v in d.values():
        print v 
     
     ② itervalues
     d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59 }

    -- <dictionary-valueiterator object at 0x106adbb50>
    for v in d.itervalues():
        print v
```


## 迭代dict的key和value

- item会把dict迭代成N个List[tuple{}],item有iteritems

### 任务-请根据dict打印内容：
    d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59, 'Paul': 74 }
    打印出 name : score，最后再打印出平均分 average : score。


    d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59, 'Paul': 74 }

    sum = 0.0
    for k, v in d.items():
        sum = sum + v
        print k,":",v
    print 'average', ':', sum/len(d)


## 生成列表-列表生成式
    任务
    请利用列表生成式生成列表 [1x2, 3x4, 5x6, 7x8, ..., 99x100]
    提示：range(1, 100, 2) 可以生成list [1, 3, 5, 7, 9,...]

```
print [x *(x+1) for x in range(1,101,2)]

```


## 复杂表达式

- 字符串可以通过 % 进行格式化，用指定的参数替代 %s。字符串的join()方法可以把一个 list 拼接成一个字符串。

### 在生成的表格中，对于没有及格的同学，请把分数标记为红色。
-提示：红色可以用 <td style="color:red"> 实现。
```
    d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59 }
    def generate_tr(name, score):
        if score>60:
        return '<tr><td>%s</td><td>%s</td></tr>' % (name, score)
        else:
         return '<tr><td>%s</td><td style="color:red">%s</td></tr>' % (name, score)    

    tds = [generate_tr for name, score in d.iteritems()]
    print '<table border="1">'
    print '<tr><th>Name</th><th>Score</th><tr>'
    print '\n'.join(tds)
    print '</table>'

    --解释
    #先把'<tr><td>%s</td><td>%s</td></tr>' % (name, score)部分用gtr表示，
    则此代码可以表示为[gtr for name, score in d.iteritems()]
    我们先看gtr部分，字符串可以通过 % 进行格式化，用指定的参数替代 %s，所以gtr表示一个字符串。举个例子，如果name=Mike，score=90，那么gtr='<tr><td>Mike</td><td>90</td></tr>'。
    再看整个这代码，这是个利用for循环迭代dict d的复杂表达式，取出的name，score将会一一带入gtr，构成一个元素为字符串的list。#

```

## 条件过滤
    列表生成式的 for 循环后面还可以加上 if 判断。例如：
    >>> [x * x for x in range(1, 11)]
    [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]如果我们只想要偶数的平方，不改动 range()的情况下，可以加上 if 来筛选：
    >>> [x * x for x in range(1, 11) if x % 2 == 0]
    [4, 16, 36, 64, 100]有了 if 条件，只有 if 判断为 True 的时候，才把循环的当前元素添加到列表中。


### 任务 请编写一个函数，它接受一个 list，然后把list中的所有字符串变成大写后返回，非字符串元素将被忽略。


        提示：
        1. isinstance(x, str) 可以判断变量 x 是否是字符串；
        2. 字符串的 upper() 方法可以返回大写的字母。

        def toUppers(L):
            return [s.upper() for s in L if isinstance(s, str)]

        print toUppers(['Hello', 'world', 101])

## 多层表达式
- for循环可以嵌套，因此，在列表生成式中，也可以用多层 for 循环来生成列表。

- 对于字符串 'ABC' 和 '123'，可以使用两层循环，生成全排列：

    >>> [m + n for m in 'ABC' for n in '123']
    ['A1', 'A2', 'A3', 'B1', 'B2', 'B3', 'C1', 'C2', 'C3']
    翻译成循环代码就像下面这样：

    L = []
    for m in 'ABC':
        for n in '123':
            L.append(m + n)

### 任务利用 3 层for循环的列表生成式，找出对称的 3 位数。例如，121 就是对称数，因为从右到左倒过来还是 121。

    print [a*100+b*10+c for a in range(1,10) for b in range(0,10) for c in range(0,10) if a==c]



