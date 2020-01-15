---
date: 2019-04-02 00:00
title:  Python 3.8新特性——仅限位置形参
subtitle: 介绍Python语言新的特性
cover: /images/new-02.png
categories: [Python新特性]
---
## 仅限位置形参

Positional-only parameters官方翻译为仅限位置形参，也可以理解为只接受位置参数。意思就是，它只是一个位置参数，不接受关键字传参。

语法： 

```
def funx(a,b,/): # / 指明，前面的a,b参数是仅限位置形参
    pass
```

函数形参语法`/` 用来指明某些函数形参必须使用仅限位置而非关键字参数

其实，Python内置的很多C函数接口都是这种形式，比如

```
>>> import builtins
>>> help(__builtins__.divmod)
Help on built-in function divmod in module builtins:

divmod(x, y, /)
    Return the tuple (x//y, x%y).  Invariant: div*y + mod == x.

```

很多函数后面都有 `/`来表明，左边的这些参数只接受位置参数。

```
>>> divmod(1,2)
(0, 1)
>>> divmod(x=1,y=2) 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: divmod() takes no keyword arguments
>>>

```

指定关键字的参数会报语法错误，它的用途就是强制使用者用位置参数来传参。


#### 官方例子

```
>>> def f(a,b,/,**kwargs):
...   print(a,b,kwargs)
...
>>> f(10,20,a=1,b=2,c=3)
10 20 {'a': 1, 'b': 2, 'c': 3}
```

由于在 `/` 左侧的形参不会被公开为可用关键字

这里的a,b 为仅限位置参数，最后a,b会被赋值了两次。
位置参数赋值一次，关键字参数赋值一次，关键字参数以kwargs字典的形式存在，需要通过 `kwargs['a'],kwargs['b']`访问。



现在我们来看一下，添加了仅限位置形参之后的函数参数形式

```
def name(positional_only_parameters, /, positional_or_keyword_parameters,
         *, keyword_only_parameters):
```

包括了仅限位置形参， `/`, 位置形参或者关键字参数 ,`*`，仅限关键字参数。

![](https://tva1.sinaimg.cn/large/006tNbRwly1gai73g9r3ej30so09mq3k.jpg)


最后，我们可以定义以下形式的函数

```
def name(p1, p2, /, p_or_kw, *, kw):
def name(p1, p2=None, /, p_or_kw=None, *, kw):
def name(p1, p2=None, /, *, kw):
def name(p1, p2=None, /):
def name(p1, p2, /, p_or_kw):
def name(p1, p2, /):

```


#### 参考
- https://docs.python.org/zh-cn/3.8/whatsnew/3.8.html
- https://docs.python.org/zh-cn/3/howto/clinic.html
- https://www.python.org/dev/peps/pep-0570

