# 最新版jupyter notebook导出为PDF




RT，关于jupyter notebook导出为PDF的那些事儿

<!--more-->

## 背景

自己在用`Python` 分析数据时总会依赖jupyter notebook（以下简称 notebook） 交互式的界面，然后把报告中需要的数据复制出来贴在Excel里

这波操作里，就不想贴数据了，而且在notebook里写结论也能直接定位到操作过程，或者说日后能复现、方便管理。所以可以考虑使用 notebook 导出的功能。虽然目前支持多种格式（html、markdown、pdf等），但又想兼顾各设备（电脑、手机）打开的便捷性，所以导出为pdf是个不错的选择。

但是，在导出为PDF的过程中遇到了以下几个问题/需求：

1. 不显示中文

2. Code Free

   对外展示的时候只需要结果即可

3. 添加报告的目录

4. pandas.DataFrame Style格式导出后直接不显示

5. ...

以上遇到的几个问题/需求，结合notebook导出为pdf的流程来看主要对应 **notebook2LaTex** 和 **LaTex2PDF** 两个阶段的事。但**使用感知依然还是一件事**，即notebook导出为PDF

（「Mark」目前还有个先转html再转pdf的方式 [`to webpdf`](https://nbconvert.readthedocs.io/en/latest/usage.html#webpdf)）

> Choose to convert using latex or chrome web browser when converting to pdf. Output is significantly different for each.
>
> - Use 'latex' when you desire a formal report. 
> - Use 'browser' to get output similar to that when printing to pdf within a chrome web browser.

上文所列的 2、4便对应**notebook2LaTex**阶段，而剩下的第1、3个问题便是**LaTex2PDF**阶段的问题

在此均使用` jupyter nbconver` 终端命令实现导出，且略过需要提前配置好的 **Pandoc** 和 **Tex**

## notebook2LaTex

### Code Free

关于不需要input中的代码问题，终端命令是能顺利实现的

```shell
jupyter nbconvert notebook.ipynb --to pdf --no-input
```

但这样导出的pdf会遇到表格数据串行的问题，而且`pandas.DataFrame` 加上一些渲染后再导出也无法显示，即第4个问题

### DataFrames2images

（当目前问题解决不了的时候，那就装个包：[dataframe-image](https://pypi.org/project/dataframe-image/)）

![png](https://warehouse-camo.ingress.cmh1.psfhosted.org/c3559fe1b7a0480586546e509dda16d1aa562784/68747470733a2f2f6769746875622e636f6d2f646578706c6f2f646174616672616d655f696d6167652f7261772f67682d70616765732f696d616765732f646174616672616d655f696d6167655f636f6d706172652e706e67)



但是这个包没有封装`--no-input`的选项，好在这个包的`pdfExporter`部分也是从`nbconvert`继承过来的，所以改下dataframe-image的源代码（`_convert.py`文件）也是能实现code-free的

```python
pdf = PDFExporter(config={'NbConvertBase': {'display_data_priority': 
                                            self.DISPLAY_DATA_PRIORITY}  })

# 改为
from traitlets.config import Config
c = Config({"NbConvertBase": {
                    "display_data_priority": self.DISPLAY_DATA_PRIORITY
                }})
c.PDFExporter.exclude_input_prompt = True
c.PDFExporter.exclude_input = True
pdf = PDFExporter(config=c)
```





## LaTex2PDF

### 中文及目录问题

这个问题放在**LaTex2PDF**这个阶段就好理解了，这个是跟LaTex有关，这玩意儿一开始就是老外整出来的，要显示中文得修改`base.tex.j2`文件，加载一些宏包。（网上说要改`article.tplx`文件的都是比较老的版本了，目前`nbconvert`不那么整了）

```latex
\usepackage[slantfont, boldfont]{xeCJK}

% 其他一些问题
% %加水印
\usepackage{draftwatermark, everypage}
\SetWatermarkText{DRAFT}
\SetWatermarkLightness{0.95}
\SetWatermarkScale{0.4}

% %中文时间 
\usepackage{zhnumber}
\date{\zhtoday}

% %目录
\tableofcontents
```



之后工作中遇到新需求、问题再继续更新～



## Reference

1. https://pypi.org/project/dataframe-image/
2. https://nbconvert.readthedocs.io/en/latest/index.html





<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
