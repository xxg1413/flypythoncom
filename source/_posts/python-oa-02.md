---
date: 2019-01-02 00:00
title:  Python写入Word文档
subtitle: 极简Python自动化办公系列
cover: /images/oa2.jpg
categories: [自动化办公]
---
>【极简Python 自动化办公】专栏是介绍如何利用python办公，减少工作负荷。篇幅精炼，内容易懂，无论是否有编程基础，都非常适合。





在上次文章中，我们学习了【用python写入excel】，这次我们学习Python写word文档吧！



场景：
有时候，办公室需要按模版生成的固定的文件，模板是固定的，只是每次需要替换信息。如下图的收入证明，模版中所有标黄的都是需要替换的信息：
![](http://jcjview.github.io/img/pythonword_model0.png)
如果手工来做这个事情，每次至少需要10分钟的时间。假如每天要开15份，则至少要花2个半小时，而且手工编辑word很容易出错。

可不可用python写个程序，解决这个问题呢？

结论当然是肯定的！

## 0.摘要

**本文大约需要15分钟，建议在电脑上打开，边阅读边操作。**

1. 安装Python读写word模块，python-docx
2. 准备word模板，准备写入word文档内容
3. 编写python代码并运行
4.通过读取excel表格中的信息，批量生成word文件

## 1.安装python-docx模块

与上篇文章类似，需要在cmd窗口输入`pip install python-docx`。
![](http://jcjview.github.io/img/flypython_python_docx_pip.png)


## 2.准备word模板，准备写入word文档内容
word模板如上所示，（可以不需要标黄），这里注意，需要替换的文字或数字位置，用"XXXX"来固定替代。保存为`个人收入证明.docx`。

| 名称 | 内容|
|---------|------------|
| 姓名 | 张三 |
| 身份证号 | 104111199009103531 |
|职务 | 工程师|
| 工作年限 | 10 |
| 月收入 | 10000 |
| 大写 | 壹万元整|
|联系人|李四|
| 单位名称 | 格物致知股份有限公司 |
| 单位地址 |  珠海市横琴新区宝华路6号105室-67425|
| 联系电话 | 0756-8627528 |

## 3. 编写python代码并运行


在word模板的同级目录，新建一个writeword.py文件，用记事本或其他文本编辑工具打开。

编程思路：


1. 用python打开对应doc模板
1. 按顺序找到每一个需要替换的位置字符"XXXX"，替换为对应的内容
1. 另存为doc为另一个文件


在文本编辑工具中输入如下代码，保存并关闭。

```
from docx import Document
#准备写入内容
name="张三"
id_code="104111199009103531"
career="工程师"
working_years="10"
salary="10000"
salary_uppercase="壹万元整"
contact="李四"
company="格物厚德股份有限公司"
address="珠海市横琴新区宝华路6号105室-67425"
tel="0756-8627528"
#打开doc

textlist=[name,id_code,career,working_years,salary,salary_uppercase,company,address,contact,tel]

doc = Document("个人收入证明.docx")

count=0

for p in doc.paragraphs:
        if 'XXXX' in p.text:
            inline = p.runs
            for i in range(len(inline)):
                if 'XXXX' in inline[i].text:
                    text = inline[i].text.replace('XXXX', textlist[count])
                    inline[i].text = text
                    count+=1
                    print(count)
doc.save("%s_个人收入证明.docx"%name)

```

在同级目录，打开cmd，运行writeword.py `python writeword.py`

生成结果如下：
`张三_个人收入证明.docx`
![](http://jcjview.github.io/img/pythonword2.png)
## 4.通过读取excel表格中的信息，批量生成word文件

这里生成了对应word文件，但也有几个问题：

1. 对应的日期并没有自动填写，应当填写文件生成时对应的日期
2. 如果是生成大量同样word文档的话，目前的程序也需要一个一个改，并没有提升太多效率

如果您看过我们之前的2篇用python读写excel的文章，您肯定就会想到，可以利用读取excel表格里的内容，批量生成对应的word文档。对，让我们继续吧！

这里再准备一个excel文件，将需要批量写入的信息写在excel中，并保存为income.xlsx在同级目录，如下图：

![](http://jcjview.github.io/img/pythonword23.png)

修改python 文件writeword.py

```
from docx import Document
#准备写入内容
import xlrd
import time
# 当前时间元组
from datetime import datetime
nt=datetime.now()
# 可以输入中文年月日
datestr=nt.strftime('%Y{y}%m{m}%d{d}').format(y='年', m='月', d='日')

xlsx=xlrd.open_workbook('income.xlsx')
sheet=xlsx.sheet_by_index(0)
for row in range(1,sheet.nrows):
    doc = Document("个人收入证明.docx")
    count=0
    textlist=[]
    for col in range(0,sheet.ncols):
        textlist.append(str(sheet.cell_value(row, col)))

    for p in doc.paragraphs:
            if 'XXXX' in p.text:
                inline = p.runs
                for i in range(len(inline)):
                    if 'XXXX' in inline[i].text:
                        text = inline[i].text.replace('XXXX', textlist[count])
                        inline[i].text = text
                        count+=1
            if 'X 年   X 月  X 日' in p.text:
                inline = p.runs
                for i in range(len(inline)):
                    if 'X 年   X 月  X 日' in inline[i].text:
                        text = inline[i].text.replace('X 年   X 月  X 日', datestr)
                        inline[i].text = text

    doc.save("%s_个人收入证明.docx"%textlist[0])
```
   
  运行后，输入结果：
  
  

![](http://jcjview.github.io/img/pythonword3.png)


![](http://jcjview.github.io/img/pythonword4.png)


*人生苦短，我用python早下班。如果觉得不错，对你工作中有帮助，请加我微信公众号flypython，我们一起探讨python相关问题*

  ![flypython微信公众号](https://flypython.com/images/wechat.png)