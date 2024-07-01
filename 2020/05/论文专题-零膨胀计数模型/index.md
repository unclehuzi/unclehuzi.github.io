# 【论文专题】零膨胀计数模型


基于`R`，总结零膨胀计数模型的应用流程

<!--more-->

---

我之所以用计数模型，主要是受到清华水利专业某博士某篇论文[^1]的启发。我也将他构造变量的思路也写进了我论文的展望部分。

在经济学及社会科学领域也会遇到对计数数据（Count Data）建模的任务

> 计数数据是一种数据类型，取值只能是非负整数{0,1,2,3,...}，并且数值并不是排名而是计数

泊松回归（Poisson Regression）是处理计数数据的常用方法

> stats package - glm() 函数

但是泊松回归并不能很好的解决计数变量数据中“**过度分散**”（over-dispersion）的问题

针对 ***over-dispersion*** 的问题，往往会采用负二项回归（`Negative Binomial Regression`）的方法

> MASS package - glm.nb() 函数

但，无论是泊松回归还是负二项回归都不能很好的解决计数数据零过多的问题

此时，零膨胀计数模型（`zero-inflated models`）就诞生了

> zero-inflated models are mixture models that combine a count component and a point mass at zero.

关于零膨胀计数模型，各自的软件或语言都有自己的实现方法。从我查阅的资料来看，目前 `stata` 和 `R` 在这方面资料比较多，`Python` 相对较少。

我选择的是`R`，因为`Anaconda`装`rstudio`比较便捷，且学习成本比`stata`低。

所以本文基于`R`，在确定计数数据存在零膨胀的前提下（一般看计数数据的频率直方图，或者直接用`vuong()`完成模型选择），总结零膨胀计数模型的应用流程

## 启动rstudio

用 Anaconda 安装 Rstudio 的过程中，需要手动创建一个R运行的新环境（如，r_env）

打开 anaconda 比较费时儿，所以会在 conda命令端 启动Rstudio

```shell
# 查看环境目录
conda info -e

# 切换至文件目录下启动
cd "工作目录"

# 激活环境
conda activate r_env

# 启动 Rstudio
rstudio
```

## 读取数据文件

R打开csv格式的文件比较省事儿

```r
# 数据文件保存在rstudio启动的目录下

my.data1<-read.csv("data.csv")
```

## Over-Dispersion？

有两种方法检验计数数据是否存在over-dispersion的现象，但对象不同，用的模型不同

* odTest()

* dispersiontest()


### odTest()

需要 `pscl` package

```r
odTest(glmnb_obj, alpha=.05, digits = max(3, getOption("digits") - 3))
```

先用负二项模型跑一次回归

```r
library(MASS)
nb <- glm.nb(y ~ x1+x2+x3, data=my.data1)

library(pscl)
odTest(nb)
```

$p-value<0.05$，说明计数数据存在 ***over-dispersion*** 现象


### dispersiontest()

```r
glm_p <- glm(y ~ x1+x2+x3, data = my.data1, family = poisson)

library(AER)
dispersiontest(glm_p,trafo=1)
```

计数数据存在零膨胀且over-dispersion的现象，那么就需要采用零膨胀负二项模型

若不存在over-dispersion的现象，则采用零膨胀泊松模型

当然，可以两个模型都跑一次，最后通过`vuong()`完成模型的选择

所以，也可以比较零膨胀泊松模型和泊松模型

## run regressions

零膨胀计数模型都依赖 `pscl` package

### 零膨胀负二项回归模型

```r
m3 <- zeroinfl(y ~ x1+x2+x3|x4+x5+x6, data=my.data1,dist="negbin")
```

零膨胀计数模型是分成了两部分建模：一部分是计数部分，另一部分处理为二分类 $(0,1)$ 的情况

$x_1,x_2,x_3$ 是选定的计数部分的影响因素；$x_4,x_5,x_6$ 是二分类情况的影响因素

如果两者的影响因素一样的，则公式形式可以简单写为

```r
m3 <- zeroinfl(y ~ x1+x2+x3, data=my.data1,dist="negbin")

# 若数据中除y之外均为影响因素

m3 <- zeroinfl(y ~ ., data=my.data1,dist="negbin")
```

### 零膨胀泊松回归模型

```r
m4 <- zeroinfl(y ~ x1+x2+x3|x4+x5+x6, data=my.data1,dist = 'poisson')
```

$x_1,x_2,x_3$ 的表现方式同上

## 模型选择

```r
vuong(m3,m4)
```

根据结果选择模型即可

## 参考资料

[^1]: Hanchen Jiang et al.: 10.3390/su10051509
[^2]: Achim Zeileis, Christian Kleiber, Simon Jackman: Regression_Models_for_Count_Data_in_R





<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
