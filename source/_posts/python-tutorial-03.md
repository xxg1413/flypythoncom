---
date: 2019-02-03 00:00
title:  Python正则15分钟入门
subtitle: Python入门教程
cover: /images/tutorial3.png
categories: [Python入门]
---

flypython群里有同学问我，如何从大量格式不确定的word文档抽取姓名、电话号码、邮箱等信息存入excel表格。通过之前我们的文章，他已经学会读取和写入文档和表格，但就是无法处理格式不确定的文档。**这里介绍的正则方法，可以帮助他解决这个问题。**



## 目标

15分钟内让你真正明白正则表达式是什么，并且让你可以在自己的python程序里正确使用它。


你将学会：

1. 极简python使用正则的方法
2. 如果利用python高效的匹配字符串
3. 如何利用python正则进行文本判断、过滤、信息提取


## 0.极简正则入门

假设程序从word或者excel读取了一串字符串，字符串中有一部分是电话号码，现在需要完整提取这个电话号码。



```PYTHON
import re
reg=re.compile("[0-9]+")
a=reg.findall("我的电话是3555487")
print(a)

```
输出：

![](https://raw.githubusercontent.com/jcjview/jcjview.github.io/master/img/20191016101032.png)





解释：
`"[0-9]+"`是正则表达式，意思是匹配0-9的数字，`"+"`
表示可以匹配1次-多次，`reg.findall`表示从后面的字符串里找到所有的匹配值。

## 1.字符集


字符集，又叫元字符，就是用一些特殊符号表示特定种类的字符或位置。

#### 匹配字符
| 代码  |说明 |
| :---: | --- |
| `.` | 匹配除换行符以外的任意一个字符 |
|`\d` |匹配数字|
| `\w` | 匹配字母或数字或下划线或汉字 |
| `\s` | 匹配任意的空白符 |
| `^` | 匹配字符串的开始 |
| `$` | 匹配字符串的结束 |


举例

```PYTHON
import re
reg=re.compile("我.")
a=reg.findall("我的电话是3555487")
print(a)

```
输出：
![](http://jcjview.github.io/img/re201910161010321.png)


#### 重复匹配
| 代码  |说明 |
| :---: | --- |
| `*` |重复0次-无数次 |
| `+` |重复1次-无数次 |
| `?` |重复0次-1次 |
| `{m}` |重复m次 |
| `{m,n}` |重复m-n次 |

举例

```PYTHON
import re
reg=re.compile("5+")
a=reg.findall("我的电话是3555487")
print(a)

```
输出：
![](
https://raw.githubusercontent.com/jcjview/jcjview.github.io/master/img/re_20191016101824.png)




#### 贪婪与懒惰
贪婪：匹配尽可能长的字符串
懒惰：匹配尽可能短的字符串
懒惰模式的启用只需在重复元字符之后加?既可。
* `*?` 重复任意次，但尽可能少重复
* `+?` 重复1次或更多次，但尽可能少重复
*  `??` 重复0次或1次，但尽可能少重复
* `{n,m}?` 重复n到m次，但尽可能少重复
* `{n,}?` 重复n次以上，但尽可能少重复

举例

```PYTHON
import re
reg=re.compile("5+?")
a=reg.findall("我的电话是3555487")
print(a)

```
输出
![](https://raw.githubusercontent.com/jcjview/jcjview.github.io/master/img//re_20191016101824111.png)

注意：
如果想匹配元字符本身或者正则中的一些特殊字符，使用`\\`转义。

这里介绍的正则内容是最基础的，想要了解更详细的正则表达式语法，请参考：




## 2.利用正则判断

#### 判断
有时候我们想利用正则表达式对用户输入进行判断，比如判断用户输入的身份证号是否符合规则，那么可以这样写：


```PYTHON
import re
r=r'^([1-9]\d{5}[12]\d{3}(0[1-9]|1[012])(0[1-9]|[12][0-9]|3[01])\d{3}[0-9xX])$'

s1 = '110102200101014779'

#判断s1字符串是符合正则r
an = re.search(r, s1)
if an:
    print ('yes')
else:
    print ('no')
```
输入结果
![](https://raw.githubusercontent.com/jcjview/jcjview.github.io/master/img/re_20191016104208.png)

说明：`^`字符表示必须匹配字符串开头；`$`表示必须匹配字符串结尾。

#### 过滤
假设，输出一串文本，只想保留汉字，去除特殊符号。代码如下：
```PYTHON
import re
special_character_removal = re.compile(r'[，。、【 】“”：；（）《》‘’{}？！⑦%>℃.^-——=&#@￥『』]', re.IGNORECASE)
line="贾蓉看了说：“高明的很。还要请教先生，这病与『性』命终久有妨无妨？”"
l = special_character_removal.sub('', line)
print(l)
```
输入结果：
![](https://raw.githubusercontent.com/jcjview/jcjview.github.io/master/img/re_20191016104753.png)


#### 查找位置
查找某个文本在字符串中的位置，一般用于信息提取。


```PYTHON
import re
p = re.compile("\d+")
content="2019年9月9月9日"
result2 = p.finditer(content)

for m in result2:
    print("str",m.group())  ##字符串
    print("start: ",m.start()," end: ",m.end())  ##字符串位置
```

输出结果

![](https://raw.githubusercontent.com/jcjview/jcjview.github.io/master/img/re_20191016110227.png)


*人生苦短，我用python早下班。如果觉得不错，对你工作中有帮助，请长按下面二维码关注我们。（回复训练营加群，一起探讨python问题）*

  ![flypython微信公众号](https://flypython.com/images/wechat.png)