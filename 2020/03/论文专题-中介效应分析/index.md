# 【论文专题】中介效应分析


在营销、管理领域，发现 $X$ 会影响 $Y$ 后，还会进一步的琢磨是如何影响，回答一个关于 ***HOW*** 的问题。

<!--more-->


> 我觉得是沿用了心理学的研究方法，这里就不谈流派了，毕业要紧

所以憋论文的时候除了找到变量 $X$、$Y$，往往还要憋个变量 $M$ 出来，这个变量 $M$ 就用来回答 $X$ 如何影响 $Y$，$M$ 被称为中介变量（**Mediator Variable**）

为了得到因果关系，之后往往会设计实验，以问卷的形式收集数据，最后用统计学的方法一顿操作验证 $X$ 是否会影响 $M$ 进而影响 $Y$

这篇就是记录如何用 PROCESS[^1] 完成中介效应的分析，以简单的中介效应模型为例。

![中介效应](https://mmbiz.qpic.cn/mmbiz_png/AVUggcicibDoKpubOylWcTMqUIdjnt9h70h4ITJRU1uEqPAhAKugngZpusd77BN2VF4znvZvqce9V84gxJUHn9zg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


## 1 理论部分

上图的理论模型用数学数学语言可写为

$$
M = i_M+aX+e_M \label{M}
$$

$$
Y = i_Y + c^,X + bM + e_Y \label{Y}
$$

正如下图所示

![中介效应](https://mmbiz.qpic.cn/mmbiz_png/AVUggcicibDoKpubOylWcTMqUIdjnt9h704Q1Zia3F6cLmUQxYbPu4GJHwm5XHzia8iby3v0bAQsl9AIEppXd5WZqeA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


* 直接效应

$c^,$ 表示 $X$ 影响 $Y$ 的直接效应

$$
c^,=[\widehat{Y}|(X=x+1,M=m)] - [\widehat{Y}|(X=x,M=m)]
$$

$c^,$ 可解释为：在变量 $M$ 保持不变的情况下，自变量 $X$ 增加一个单位，因变量 $Y$ 均值变化 $c^,$ 。


* 间接效应（indirect effect）

将公式$\ref{M}$ 代入公式$\ref{Y}$，可发现系数 $a \times b$ 便是估计中介效应的，所以中介效应是否存在，就需要关注系数 $ab$ 的情况了


PROCESS也是采用最小二乘法得到系数的

但PROCESS特别的地方在于，他是采用 Bootstrap Confidence Interval 的方法对检验以下假设

$$
H0: ab=0
$$

> 「MARK」之后再另起灶炉简单扯一点Bootstrapping

用PROCESS做中介效应分析，结果的最后会给出间接效应的 95% bootstrap confidence interval

如果区间内不包含0，则说明中介效应成立

## 2 实际操作

步骤如下：

1. SPSS加载 process的语法文件

    ***使用过程中可以自主编写语法、也可以菜单式的操作***

2. 变量和数据字段一一对应

3. 基于理论研究模型，选择PROCESS的模型（本例对应的是model 4）

    > 我感觉这也算是PROCESS的一个弊端吧，灵活性不高。

4. 其他可选

    * number of bootstrap samples
    
        一般是5000，数据量小的时候可往大了选

    * 选项(options)

        标准化、中心化、产生画图的数据
        
        调节效应分析中，spotlight analysis 选择16th、84th分位数表示低高，还是选择 均值±标准差 表示低、高


    * 多分类变量的处理

        如果是二分类（dichotomous）就不管

        因为多分类变量(假设4类)的1，2，3，4并没有意义，只是表示不同的类别而已，一般通过indicator coding（也叫dummy coding）的方式将变量转为***dummy variable*** ，用 $4-1=3$ 个字段表示


一顿操作之后，便可得到如下结果

![结果](https://mmbiz.qpic.cn/mmbiz_png/AVUggcicibDoKpubOylWcTMqUIdjnt9h709ksiczJJGNBraS8r1gLTRLuNsRf5WjbZpnArdxiclbzGS1jtcngOD4nQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

正如理论部分所说，是否存在中介效应，

就看结果中 ***Indirect effect(s) of X on Y*** 的部分

Bootstrap置信区间不包含0，则说明中介效应存在。


## 参考资料

[^1]: Hayes A F: Introduction_to_Mediation_Moderation_and_Conditional_Process_Analysis_A_Regression_Based[M].2ed.2018


<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
