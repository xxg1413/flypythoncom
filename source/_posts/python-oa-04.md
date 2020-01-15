---
date: 2019-01-04 00:00
title:  Python读取PDF文字和表格
subtitle: 极简Python自动化办公系列
cover: /images/oa4.jpg
categories: [自动化办公]
---
>【极简Python 自动化办公】专栏是介绍如何利用python办公，减少工作负荷。篇幅精炼，内容易懂，无论是否有编程基础，都非常适合。


在日常的工作中，处理PDF是最平常不过的事情了。今天带来极简Python自动化办公系列之使用Python提取Pdf文字和表格，希望能够在PDF处理上帮到你。


这次我们准备了一个pdf测试文件，内容如下：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g8e0nlr7gdj30kc0zcjua.jpg)



pdf中包括了2页，有文字，图片和表格，覆盖了大部分pdf的场景。



#### pdfplumber介绍

Pdfplumber是一个可以处理pdf格式信息的库。它可以查找关于每个文本字符、矩阵、和行的详细信息，也可以对表格进行提取并进行可视化调试。

官方repo:
    `https://github.com/jsvine/pdfplumber`

安装：
`pip install pdfplumber`


#### 使用入门

```
import pdfplumber
	
with pdfplumber.open("test.pdf") as pdf:
	first_page = pdf.pages[0] #取第一页
	print(first_page.chars[0])#打印第一页第一个字文字信息

```


结果：

```
{'fontname': 'CRSMRF+PingFangTC-Semibold', 'adv': Decimal('1.000'), 'upright': 1, 'x0': Decimal('57.000'), 'y0': Decimal('751.840'), 'x1': Decimal('81.000'), 'y1': Decimal('779.776'), 'width': Decimal('24.000'), 'height': Decimal('27.936'), 'size': Decimal('27.936'), 'object_type': 'char', 'page_number': 1, 'text': '关', 'top': Decimal('62.224'), 'bottom': Decimal('90.160'), 'doctop': Decimal('62.224')}
```


格式化之后：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g8e0nm8qvij30kq0hi0um.jpg)


```
{
	    "fontname":"CRSMRF+PingFangTC-Semibold",
	    "adv":"1.000",
	    "upright":1,
	    "x0":"57.000",
	    "y0":"751.840",
	    "x1":"81.000",
	    "y1":"779.776",
	    "width":"24.000",
	    "height":"27.936",
	    "size":"27.936",
	    "object_type":"char",
	    "page_number":1,  #页数
	    "text":"关",  #第一个文字
	    "top":"62.224",
	    "bottom":"90.160",
	    "doctop":"62.224"
}

```




#### 常用方法

- extract_text() 用来提页面中的文本，将页面的所有字符对象整理为一个字符串
- extract_words() 返回的是所有的单词及其相关信息
- extract_tables() 提取页面的表格



#### 提取文字

```
#!/usr/bin/env python3

import pdfplumber

with pdfplumber.open("test.pdf") as pdf:
	first_page = pdf.pages[0]
	text = first_page.extract_text() #提取第一页的所有文字
	print(text)

```

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g8e0nmzq4wj30r007wgms.jpg)


```
关于我们
关于FlyPython
FlyPython是提供⼀站式Python编程学习的组织，我们致⼒于为⽤户提供⾼
效，有趣的学习环境，打造专注于Python的中⽂学习社区。
联系我们
客服&合作: 微信号 flypython
微信公众号：

```

#### 提取表格

```
#!/usr/bin/env python3

import pdfplumber
import pandas as pd

with pdfplumber.open("test.pdf") as pdf:
    first_page = pdf.pages[0]
    text = first_page.extract_text()
    print(text)

    second_page = pdf.pages[1] #第二页
    table = second_page.extract_tables()#在第二页提取表格
    for t in table:
        df = pd.DataFrame(t[1:],columns=t[0])
        print(df)
```


![](https://tva1.sinaimg.cn/large/006y8mN6ly1g8e0no9hykj30po0f8wg9.jpg)



```
         分类                 书名
0  Python入门  Python编程：从入门到\n实践
1  Python中级          流畅的Python
2
3
```

#### 总结

pdfplumber的接口还是很容易的，如果只是需要提取文字，几行代码就可以提取到。如果是表格并没有提取出来或者错误的提取了非表格的内容，你需要在提取表格时加入`table_settings`参数来指定表格的设置。


这次的demo中，图片并没有提取出来，pdf图片的提取会放到下一篇文章，敬请期待。



*人生苦短，我用python早下班。如果觉得不错，对你工作中有帮助，可以长按下列二维码关注我们的公众号。*

  ![flypython微信公众号](https://flypython.com/images/wechat.png)