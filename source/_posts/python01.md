
title: "python学习笔记01"
date: 2015-12-26 20:34:05
tags: [python]
categories: [python]
---
# python学习笔记01 *[2015-12-26]*

## raw

    raw字符串表示不需要转译里面的特殊字符


## 转译字符

    屏蔽转译r'i'm a "nice" student' = 'i\'m a \"nice\" student'


## 换行

    '''内容

     内容2

     内容3

     内容4'''

     会直接输出这4行内容


## 输出中文   


    print u '中文' ## 表示输出unicode编码的各种文字【maxosx10自带的python2.7以上已经默认支持unicode】 

## 元素追加

    append追加到list队尾

## 元素插入

    insert(index_,'content')插入到index_的后边

## 元素删除队尾

    pop()删除并且返回list的最后一个元素

<!--more-->    

## 元素删除

    pop(index_)删除并且返回list的第index_个元素

## 不能更改的集合

    tuple元素创建以后，只能访问不能修改。



## if | else逻辑块控制 |for循环|while循环


** if else语句后面条件分别有: ,执行逻辑前面有4个空格**
### if else语句

```
score = 55



if score>=60:

    print 'passed'

else:

    print 'failed'

```
---

### elseif语句

```
score = 85



if score>=90:

    print 'excellent'

elif score>=80:

    print 'good'

elif score>=60:

    print 'passed'

else:

    print 'failed'

```
---


### 班里考试后，老师要统计平均成绩，已知4位同学的成绩用list表示如下：L = [75, 92, 59, 68]，请利用for循环计算出平均成绩。



        L = [75, 92, 59, 68]

        sum = 0.0

        for s in L:

            sum += s

        print sum / 4


---

### 利用while循环计算100以内奇数的和。

        sum = 0

        x = 1

        while x<100:

            sum+= x

            x+=2

            

        print sum



---

### 利用 while True 无限循环配合 break 语句，计算 1 + 2 + 4 + 8 + 16 + ... 的前20项的和。


        sum = 0

        x = 1

        n = 1

        while True:

            if n>20:

                break

            sum+=x

            x = x * 2

            n+=1

        print sum

---


### 对已有的计算 0 - 100 的while循环进行改造，通过增加 continue 语句，使得只计算奇数的和：

        sum = 0

        x = 0

        while True:

            

            x = x + 1

            if x > 100:

                break

            if x % 2 == 0:

                continue

            sum += x

        print sum

---



### 对100以内的两位数，请使用一个两重循环打印出所有十位数数字比个位数数字小的数，例如，23（2 < 3）。




        for x in [ 1,2,3,4,5,6,7,8,9]:

            for y in [ 0,1,2,3,4,5,6,7,8,9 ]:

                if x<y:

                    print (x*10+y)






## dict相当于java 的json

﻿```

        ﻿d = {

            'Adam': 95,

            'Lisa': 85,
            'Bart': 59
        }
            
            
            
            
        d = {
            'Adam': 95,
            'Lisa': 85,
            'Bart': 59
        }
        print 'Adam:',d.get('Adam')
        print 'Lisa:',d.get('Lisa')
        print 'Bart:',d.get('Bart')    

```




