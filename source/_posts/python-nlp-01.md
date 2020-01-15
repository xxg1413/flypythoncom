---
date: 2019-03-01 00:00
title:  使用 Python 生成《红楼梦》词云
subtitle: Python自然语言处理教程
cover: /images/nlp1.png
categories: [自然语言处理]
---


使用 Python 生成《红楼梦》词云

![](http://jcjview.github.io/img/1210058744_15500375990201n.jpg)


本文介绍如何使用python绘制《红楼梦》的词云。

>“词云”就是对网络文本中出现频率较高的“关键词”予以视觉上的突出，形成“关键词云层”或“关键词渲染”，从而过滤掉大量的文本信息，使浏览网页者只要一眼扫过文本就可以领略文本的主旨。
>[“词云”——网络内容发布新招式  ．人民网](http://media.people.com.cn/GB/22100/61748/61749/4281906.html)


## 0.摘要

**本文建议在电脑上打开，边阅读边操作。**

1. 安装python词云工具wordcloud，画图软件matplotlib
2. 准备红楼梦文本
3. 编写python代码并运行
4.展示词云结果

## 1.安装wordcloud


可以在cmd窗口输入```pip install wordcloud  matplotlib```

![](http://jcjview.github.io/img/wordcloud001.png)


## 2.准备红楼梦文本

文本可以用下面链接下载


`https://github.com/flypythoncom/flypython/blob/master/wordcloud_hlm_seg.txt`

或者可以自己写代码，对文本进行清洗，分词。
这里需要安装jieba分词，`pip install jieba`
``` python
import jieba
import re

special_character_removal = re.compile(r'[，。、【 】“”：；（）《》‘’{}？！⑦%>℃.^-——=&#@￥『』]', re.IGNORECASE)

fw=open("hlm_seg.txt","w",encoding="utf-8")

with open('hlm.txt',encoding="utf-8") as fp:
    for line in fp:
        l = special_character_removal.sub('', line.strip())
        words=jieba.cut(l)
        t=" ".join(words)
        fw.write(t)
        fw.write("\n")
 
fw.close()

```


## 3. 编写词云python代码并运行

```python
from os import path  
from wordcloud import WordCloud

d = path.dirname(__file__)  
# Read the whole text.  
text = open(path.join(d, 'hlm_seg.txt'),encoding="utf-8").read()  
# Generate a word cloud image  
# font=path.join(d, "simkai.ttf")  
font='C:/Windows/Fonts/simkai.ttf'  
wordcloud = WordCloud(font_path=font,#设置中文字体，不指定就会出现中文不显示  
  width=1024,#宽  
  height=840,#高  
  background_color='white',#设置背景色   
  # max_words=100,#最大词汇数  
  # max_font_size=100#最大号字体  
  ).generate(text)  
  
# Display the generated image:  
# the matplotlib way:  
import matplotlib.pyplot as plt  
  
plt.figure()  
plt.imshow(wordcloud)  
plt.axis("off")  
plt.show()

```

结果：

![词云运行结果](
http://jcjview.github.io/img/Figure_1.png)



后台回复“词云”获得完整运行代码


*人生苦短，我用python早下班。如果觉得不错，对你工作中有帮助，请加我微信公众号flypython，我们一起探讨python相关问题*

  ![flypython微信公众号](https://flypython.com/images/wechat.png)