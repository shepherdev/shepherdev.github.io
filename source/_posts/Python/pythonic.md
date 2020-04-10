---
title: 
date: 2020-3-28
author: shepherd
img: 
top: false
cover: false
coverImg: 
password:
toc: true
mathjax: false
summary: 
categories: 
  - 编程
tags:
  - Python
---

很python的东西

### 列表推导式

```python
a = [1,2,3,4,5,6,7]

b = [i**2 for i in a if i >=4]

c = {i**3 for i in a}

print(b)
print(c)

>>>[16, 25, 36, 49]
>>>{64, 1, 8, 343, 216, 27, 125}
```

### None

None不是空字符串，不是空列表，不是0，不是False

它也是一个对象，python里面一切都是对象

判空

```python
if a:
    a != None
    a != ' '
    a !=[]
    a != False
if not a:
```

虽然None和False对应的分支是一样的，但他们的意义不一样，几乎所有对象都与bool值有转化关系

### 内置方法

```python
class Test():
    def __bool__(self):
        return False
    # 调用len()
    def __len__(self):
        return 0
 # 判断类是否为False时主要看这两个内置函数
# 共存时主要是__bool__
# 在python2里面则是__nonzero__
```


