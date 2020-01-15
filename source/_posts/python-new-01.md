---
date: 2019-04-01 00:00
title:  Python 3.8新特性——赋值表达式
subtitle: 介绍Python语言新的特性
cover: /images/new-01.png
categories: [Python新特性]
---
上周，Python3.8版本发布，到底带来了哪些新的特性呢？我们应该在哪些场景中使用这些特性呢？本周，我们通过几篇文章来告诉你答案。

![](https://tva1.sinaimg.cn/large/006tNbRwly1gai72bypmej30u00u9wl1.jpg)

#### 安装

首先，我们并不推荐安装最新版本到常用的开发环境中，你可以使用虚拟环境或者docker来尝鲜。

官方安装包

网址：`https://www.python.org/downloads/release/python-380/`

也可以使用docker

拉取镜像命令
`docker pull  python:3.8`

## 赋值表达式

![](https://tva1.sinaimg.cn/large/006tNbRwly1gai72nu5otj30jg0atdfq.jpg)

赋值表达式被叫做海象运算符，因为它的形状像海象。如果熟悉go语言的话，会对这个表达式会熟悉。


赋值表达式的语法是 
```
 name := expression
```

和赋值语句 `=` 作用差不多，非必不可少，但可以简化代码。


#### 官方示例

```
>>> a = False
>>> print(a)
False
>>> print(a := True)
True
```

此例子赋值之后，后续还需要使用变量。赋值表达可用于简化代码，提高可读性。

```
>>> inputs = list()
>>> while True:
...   current = input("your input:")
...   if current == "quit":
...       break
...   inputs.append(current)
...
your input:a
your input:b
your input:test
your input:quit
>>> inputs
['a', 'b', 'test']
```

使用赋值操作符时：

```
>>> inputs = list()
>>> while (current := input("your input:")) != "quit":
...   inputs.append(current)
...
your input:a
your input:b
your input:test
your input:quit
>>> inputs
['a', 'b', 'test']

```

此例子，省略了一条语句，可读性上升。


再来一个例子

最初版本
```
 a = [1,2,3,4]
 if len(a) > 3:  #计算 len(a) 一次 
    print(f"a is too long ({len(a)} elements,expected < 3)")  # 计算 len(a) 第二次
```

我们改写为:

改进版本
```
 a = [1,2,3,4]
 n = len(a) # 计算一次len(a)
 if n > 3:  # 多了变量n
    print(f"a is too long ({n} elements,expected < 3)")  # 
```

新特性重写:
重写版本
```
 a = [1,2,3,4]
 if (n:=len(a)) > 3:  # 计算一次len(a)，多了变量n，把两行改为一行
    print(f"a is too long ({n} elements,expected < 3)")  # 
```

从上面可以看到，重写版本和改进版本的不同在于：

```
 n = len(a) 
 if n > 3:
    pass
```
    
与
```
 if (n:=len(a)) > 3:
    pass
    
```

这两个版本的区别在于，`:=`和`=`是补充关系并不是替换关系，下面的例子可以看到官方的意图。

```
    x = 5
    print(f"x = {x}")
    
    #能用=解决的就用=解决
    y := 5 # SyntaxError: invalid syntax
    print(f"y = {y}")

    (z := 5)
    print(f"z = {z}")

```

由上面可以看出`:=`和`=`是互补关系，在应该使用`:=`的时候才可以使用`:=`。

Python语言的一致性，不管是专家还是新手，在同一个问题上都应该有一致的写法，然后这就形成了最pythonic的写法。


最后带来，新特性带来的最佳实践

```
# 简化 os.fork 
if pid := os.fork():
    # Parent code
else:
    # Child code

# 直接到把 socket 对象的 read buffer 读完为止
while data := sock.recv(8192):
    print("Received data:", data)

```

#### 参考
- https://docs.python.org/zh-cn/3.8/whatsnew/3.8.html
- https://www.python.org/dev/peps/pep-0572

