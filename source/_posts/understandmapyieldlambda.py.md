---
layout: posts
title: Understandmapyieldlambda
date: 2023-04-011 22:13:02
updated: {{ date }}
tags: 
- coding
- multiprocess
categories: Teckknowledge
---



# Table of Contents
 <p>


```python
numbers = [2, 4, 6, 8, 5]
def square(x):
    return x * x

a= []

# 只会这么写，或者next
for x in numbers:
    a.append(square(x))
print(a)

"""
map(function, *iterables)
对指定序列做映射，返回值是个迭代器，可迭代对象： list，set， tuple
map 取代for循环
"""

square_num = map(square, numbers)
print(type(square_num))

print(square_num)

# 在list的时候调用了square_num，迭代5次，并且把每次迭代的return 都append进list中。
print(list(square_num))

```

    [4, 16, 36, 64, 25]
    <class 'map'>
    <map object at 0x1109a2c40>
    [4, 16, 36, 64, 25]



```python
# 类似于
def testyield(numbers):
    for x in numbers:
        yield square(x)

# my way
a = testyield(numbers)
print(f"next {next(a)}")  # list就是对每个元素next的动作
for x in a:
    print(x)
# list can call the generator list就是一个循环call 迭代器的动作， next       
a = list(testyield(numbers))
print(a)

#将传入list，改写成传入单个元素，去掉for循环，外面用map取代for
def testyield2(x):
    #yield square(x) 这里不行。。 貌似yield必须要和for循环在一起
    return square(x)
print(f"testyield2 with map")
a = map(testyield2, numbers)
# is a map object
print(a)

# is a generator
print(next(a))
# is a list gegerator
print(list(a))

def testyield4(x):
    print("under testyield444")
    a =yield square(x)

print("aa")
g = testyield4(2)
print(g)
print(f"call the testyield4 {next(g)}")
# for x in numbers:
#     g = testyield4(x)
#     print(next(g))
    
aa = [next(testyield4(x)) for x in numbers]
print(aa)


"""
return 和 list[map(func, parameters)]

yield 和list[next(func(x)) for x in parameters]
"""
```
