---
date: 2019-01-03 00:00
title:  Python批量转换任意文档格式
subtitle: 极简Python自动化办公系列
cover: /images/oa3.jpg
categories: [自动化办公]
---
在工作中，常常会遇到文档格式的转换。如果数据不多，手工转换就可以。如果是大量文档，那我们应该怎么办呢？

今天我们将使用Python来批量处理文档转换的问题.

#### 关于unoconv 

unoconv是一款跨平台的工具，用于格式转换，支持命令行。底层实现是依赖于开源的LibreOffice/OpenOffice。

项目地址：https://github.com/unoconv/unoconv

文档地址： http://dag.wiee.rs/home-made/unoconv/

根据unoconv的文档介绍，支持上百种文档格式的转换，已经覆盖了绝大部分的需求。

#### 使用unoconv

安装unoconv比较繁琐，而且需要针对中文进行进一步的字符集配置。我们可以选择别人已经集成好的服务来进行操作，在这里我们选择了docker-unoconv-webservice项目。

项目地址为： https://github.com/zrrrzzt/docker-unoconv-webservice

查看项目的README，接口如下: 

`curl --form file=@myfile.docx http://localhost/unoconv/pdf > myfile.pdf`

我们使用下列命令，先把项目的镜像pull下来

`docker pull zrrrzzt/docker-unoconv-webservice`

然后启动命令如下：

`docker run -d -p 80:3000 zrrrzzt/docker-unoconv-webservice`

服务在80端口上提供服务，如果80端口被占用，可以调整为其他的端口

确认服务正在运行：

`docker ps | grep zrrrzzt/docker-unoconv-webservice`

```
[flypython] docker ps | grep zrrrzzt/docker-unoconv-webservice                                         
c014cf335b31        zrrrzzt/docker-unoconv-webservice   "/bin/sh -c '/usr/bi…"   2 minutes ago       Up 2 minutes        0.0.0.0:80->3000/tcp   brave_blackburn

```

从docx转换为pdf：

`curl --form file=@demo.docx http://localhost/unoconv/pdf > demo.pdf` 

```
[flypython] curl --form file=@demo.docx http://localhost/unoconv/pdf > demo.pdf               
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 12089  100  4242  100  7847   2532   4684  0:00:01  0:00:01 --:--:--  7213
[flypython] ls demo*                                                                                   
demo.docx demo.pdf

```

#### 使用Python批量请求

Python批量请求的思路是，把需要转换的文档发送到服务器，服务器会返回转换后的格式，我们保存为文件就可以了。

```
def post_file(url,path):
    filename = os.path.basename(path)
    convert_name = str(filename).split('.')[0] + '.pdf'

    m = MultipartEncoder(
        fields= {
            'file':(filename,open(path,'rb')),
        }
    )
    response = requests.request('POST', url, data=m, headers={'Content-Type':m.content_type})

    with open(convert_name, 'wb') as f:
        f.write(response.content)

    return convert_name
```

好了，更多类型转换，更完整的应用需要你根据业务来完善，这次的介绍就到这里了。demo完整代码在github上，点击原文可以获取。


https://github.com/flypythoncom/flypython/blob/master/convert.py





