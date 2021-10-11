# 因果推断补给站


汇总一些【因果推断】方面的学习资料

<!--more-->

最近看了下《原因与结果的经济学》并且结合硕士期间关于相关性与因果的思考，感觉这个 **因果推断**（***causal inference***）似乎挺有意思的。

基于数据分析的特性，我把目前因果推断方面的研究分为四大类

|-\-|聚焦某领域|未聚焦|
|---|-------|---------|
|理论|基于某领域的理论研究（如心理学因果关系的研究方法）|纯理论研究（这种常见于“扛把子”引领一个方向）|
|应用|基于某领域的应用研究（如业界的一些策略评估）|类似咨询|

常见（或高产）的应该是“基于某领域的理论研究” 和 “基于某领域的应用研究”，至于第四种（“未聚焦领域的应用型研究”）因为商业性的问题应该较难找到公开案例。

后续也打算站在小白/门外汉的角度深入了解下，记录下遇到的学习资料。（持续更新...）


## 网文

### 博客

* [Yishi Li](https://dango.rocks/blog/)
* [统计之都](https://cosx.org/tags/%E5%9B%A0%E6%9E%9C%E6%8E%A8%E6%96%AD)
* [Brady Neal](https://www.bradyneal.com/aboutme)，有篇从需求出发选择读物的[文章](https://www.bradyneal.com/which-causal-inference-book)

  ![choose-causal-inference-book](https://www.bradyneal.com/img/books_flowchart.svg)

### 知乎

* [因果推断会是下一个AI热潮吗？](https://www.zhihu.com/question/479067450)

## 书籍

* 《[原因与结果的经济学]({{< ref "posts/notes-原因与结果的经济学/index.md" >}})》
* 《[别拿相关当因果！因果关系简易入门](https://book.douban.com/subject/30271228/)》（*Why: A Guide to Finding and Using Causes*）

* [JUDEA PEARL](http://bayes.cs.ucla.edu/jp_home.html). 《为什么》（*THE BOOK OF WHY: THE NEW SCIENCE OF CAUSE AND EFFECT*）
* JUDEA PEARL. [Causality: Models, Reasoning, and Inference](http://bayes.cs.ucla.edu/BOOK-2K/index.html)
* JUDEA PEARL. [Causal Inference in Statistics: A Primer](http://bayes.cs.ucla.edu/PRIMER/index.html)
* [Donald B. Rubin](https://statistics.fas.harvard.edu/people/donald-b-rubin). Causal Inference for Statistics, Social, and Biomedical Sciences: An Introduction

* [Brady Neal](https://www.bradyneal.com/aboutme). [Introduction to Causal Inference](https://www.bradyneal.com/Introduction_to_Causal_Inference-Dec17_2020-Neal.pdf)

* Hernán MA, Robins JM. [Causal Inference: What If](https://www.hsph.harvard.edu/miguel-hernan/causal-inference-book/)，以及书中对应的[Python-code](https://github.com/jrfiedler/causal_inference_python_code)



## 工具包

* [Uber](https://uber.github.io)-[Causal ML](https://causalml.readthedocs.io/en/latest/index.html): A Python package that provides a suite of uplift modeling and causal inference methods using machine learning.

* [Microsoft](https://opensource.microsoft.com/)-[EconML](https://github.com/microsoft/EconML): A Python package for estimating heterogeneous treatment effects from observational data via machine learning.

* [Microsoft](https://opensource.microsoft.com/)-[DoWhy](https://github.com/Microsoft/dowhy): A Python library that aims to spark causal thinking and analysis.

* [CausalDiscoveryToolbox](https://github.com/FenTechSolutions/CausalDiscoveryToolbox): A package for causal inference in graphs and in the pairwise settings



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
