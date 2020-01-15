---
date: 2019-01-05 00:00
title:  Python读取PDF图片
subtitle: 极简Python自动化办公系列
cover: /images/oa4.jpg
categories: [自动化办公]
---
>【极简Python 自动化办公】专栏是介绍如何利用python办公，减少工作负荷。篇幅精炼，内容易懂，无论是否有编程基础，都非常适合。

在上次的文章中，我们从PDF中提取了文字和表格，这次我们需要提取图片。

还是先来看看我们上次的测试例子

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g8e0nlr7gdj30kc0zcjua.jpg)

这次我们要提取第一页的二维码图片。

#### fitz介绍

pymupdf是mupdf的Python绑定，而今天我们要使用的fitz是pymupdf的子模块。需要的时候，使用pip安装。

`pip install pymupdf`

导入的时使用`import fitz`导入模块。


更多信息可参考pymupdf的文档:
`https://pymupdf.readthedocs.io/en/latest/intro/`

#### 提取图片

提取图片的思路是通过正则表达式找到图片对象，然后保存为图片格式。


```
#!/usr/bin/env python3

import fitz  #pip install pymupdf
import re
import os

def find_imag(path,img_path):

    checkXO = r"/Type(?= */XObject)"
    checkIM = r"/Subtype(?= */Image)"

    pdf = fitz.open(path)

    img_count = 0
    len_XREF = pdf._getXrefLength()

    print("文件名:{}, 页数: {}, 对象: {}".format(path, len(pdf), len_XREF - 1))

    for i in range(1, len_XREF):
        text = pdf._getXrefString(i)
        isXObject = re.search(checkXO, text)

        # 使用正则表达式查看是否是图片
        isImage = re.search(checkIM, text)

        # 如果不是对象也不是图片，则continue
        if not isXObject or not isImage:
            continue
        img_count += 1
        # 根据索引生成图像
        pix = fitz.Pixmap(pdf, i)
  
        new_name = path.replace('\\', '_') + "_img{}.png".format(img_count)
        new_name = new_name.replace(':', '')

        # 如果pix.n<5,可以直接存为PNG
        if pix.n < 5:
            pix.writePNG(os.path.join(img_path, new_name))
     
        else:
            pix0 = fitz.Pixmap(fitz.csRGB, pix)
            pix0.writePNG(os.path.join(img_path, new_name))
            pix0 = None
       
        pix = None
     
        print("提取了{}张图片".format(img_count))


if __name__=='__main__':
    pdf_path = r'test.pdf'
    img_path = r'img'
    m = find_imag(pdf_path, img_path)

```


运行程序结果：

```
[pdf] python3 pdf_img.py
文件名:test.pdf, 页数: 2, 对象: 115
提取了1张图片
```

在img目录中，已经存在了我们需要的文件

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g8osipfp3uj30uu0jqgrw.jpg)


#### 总结

pymupdf的使用，今天就简单介绍到这里。更多的功能请参考pymupdf文档。

下一篇，我们将带来pdf转换为图片的讨论。


*人生苦短，我用python早下班。如果觉得不错，对你工作中有帮助，可以长按下列二维码关注我们的公众号。*

  ![flypython微信公众号](https://tva1.sinaimg.cn/large/006y8mN6ly1g8pj2981c1j3076076mxn.jpg)
 