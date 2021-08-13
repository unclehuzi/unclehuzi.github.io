# 毕业论文模板


毕业论文模板，**样式先行**

<!--more-->



三年前的我搞毕业设计，第一次接触这玩意儿，一脸懵逼

好在我朱哥搞过大创（还是国家级的），当时给我各种科普单片机的知识

搞大创的好处就是当我们不知道是画机械图还是搞电路的时候，朱哥设计的成品以及论文都搞定了，，，跑马灯跑起来、酒精浓度测起来、小屏幕亮起来

> 不过朱哥也没继续搞汽车，后来投身了我国交通事业。依稀记得朱哥远程面试的时候，寝室几个人都在打游戏，，，在我们的影响下，朱哥最终去了广东某985高校

但那时候我们对Word排版都不怎么了解

以下情景历历在目：

{{< admonition type=example title="" open=true >}}
- 这个参考文献的上标怎么弄？
- 又要加篇参考文献？序号不得又重写？
- 参考文献的引用格式是啥？
- 这些字体都要改成三号、加粗？然后就用格式刷一顿更新
- 三线表是什么鬼？又要画三线表？
- 。。。。。。
{{< /admonition >}}


总之，**我们都在Word上花了很长时间，不断的进行重复性的工作，以满足格式上要求**

但当我们面对硕士毕业论文时，在格式方面花的时间就很少很少了，没有了各种重复性的操作

因为我们秉承着**样式先行。**在写论文之前，就根据学校的要求，把字体、表格的格式调整好，写作的过程中随时切换，而且就算后续要调整，统一调整对应的样式即可，避免了重复性操作。

本文并不是记录如何去调整/设计样式，因为我认为这个真的是太多了，重点是**基于毕业论文格式记录word的正确打开方式，抽象出复用的部分**。这也是我开发模板的原因。面对不同的需求，后续只需要微调即可。

**可以在我的公众号内回复 word， 获取华东师范大学毕业论文模板**

（用过的朋友都说好）

![模板](https://gitee.com/unclehu/pic/raw/master/img/image-20210615154022680.png)


## 模块拆解

根据论文的总体框架，分别设计对应的样式。

换句话说便是先弄清楚整篇论文的字体、段落会涉及到哪些不同的格式要求，在正式写作之前，分别设计好对应的样式。以便写作过程调用。

毕业论文主体部分的格式要求，往往会涉及到各级标题、正文、脚注、参考文献（参考文献部分后续会单拎出来记录）字体的格式，理工科专业往往会要求表格是三线表。

确定好字体、段落的格式要求后，就可以针对性的修改/设计样式

![image-20210615155537100](https://gitee.com/unclehu/pic/raw/master/img/image-20210615155537100.png)

为了方便后续的写作，可以设计相应的快捷键（样式->格式->快捷键）

![快捷键](https://gitee.com/unclehu/pic/raw/master/img/image-20210615155511228.png)

## 参考文献格式

正文中往往会引用相关参考文献

这时候就要放出文献管理大杀器——**NoteExpress**

> 有了他，妈妈再也不用担心我修改参考文献的格式了

在软件中记录好被引用的内容，如作者、论文标题、年份、期刊或会议名称等等（大部分pdf、caj文献都能有效识别）

每当需要标注引用时，直接点击“引用”即可

![image-20210615155547774](https://gitee.com/unclehu/pic/raw/master/img/image-20210615155547774.png)

通过**NoteExpress**引用至word后主要涉及两大部分：

- 正文部分的标注

  上标序号 或 作者+年份 的形式

- 附在最后的参考文献

  如 [1] LIU Y. Word of mouth for movies: Its dynamics and impact on box office revenue[J]. Journal of Marketing. 2006, 70(3): 74-89.

依然是**样式先行**的逻辑，简便的办法是先在NoteExpress样式库中选择与自己要求相似的样式，当然，完全满足需求就更好了。

接着，在所选样式的基础上修改相关格式以满足自己的需求

![image-20210615155556670](https://gitee.com/unclehu/pic/raw/master/img/image-20210615155556670.png)

引文下的修改便是对应修改“正文部分的标注”的格式

题录下的修改便是对应修改“附在最后的参考文献”的格式

值得一提的是，题录中可设置排序的规则，因为有些学校要求英文文献在前面、之后再按照时间或作者排序。

![image-20210615155604889](https://gitee.com/unclehu/pic/raw/master/img/image-20210615155604889.png)

## Tips

最后再记录些杂七杂八的tips

### 表格、图片名称

![image-20210615155611996](https://gitee.com/unclehu/pic/raw/master/img/image-20210615155611996.png)

选中表格（/图片），右键“插入题注”，标签选择“表”，编号选择“包含章节号”

### 交叉引用

论文中往往会看到如图xxx所示、如表xxx所示等表述方式，并且希望能定位到相关表格或图片时，就需要采用文内交叉引用的方法

![image-20210615155620930](https://gitee.com/unclehu/pic/raw/master/img/image-20210615155620930.png)

### 公式

当我们用MathType编辑公式，粘贴至word时，往往会影响word的格式，主要体现在行与行之间的距离变大。

对应的解决方案便：在对应的段落内，右键选择“段落”，取消勾选下图中所示内容

![image-20210615155631972](https://gitee.com/unclehu/pic/raw/master/img/image-20210615155631972.png)

### 代码高亮

涉及到代码的话，为了美观，往往希望word中代码也实现高亮

推荐 [planetB](http://www.planetb.ca/syntax-highlight-word)

![image-20210615155642274](https://gitee.com/unclehu/pic/raw/master/img/image-20210615155642274.png)



暂时统计了这几个tips，欢迎交流



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
