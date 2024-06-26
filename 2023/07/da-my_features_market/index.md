# 信贷风险特征维护框架


结合关系型数据库的思想，基于Notion实现及维护信贷风险领域特征

<!--more-->

整理信贷风险主题下的（常用）特征是我一直想做的一件事儿，但之前一直没捋清楚由下至上的实现、维护的逻辑及方式。最近被老板领着研究人行报告，交流过程中思路、框架慢慢成型，便从人行报告相关变量出发，依托Notion搭建信贷风险特征框架，供后续策略、模型等业务场景使用

## 为什么做这件事？

当处于一个成熟的团队、业务也达到一定规模，在迭代策略、建模时，一切似乎都很敏捷，然后就误把平台能力的强大归为自己的能力，尤其是模型、数据团队很强的时候。但可能自己并不了解所用特征是如何加工的或者说不会去细盘逻辑，尤其是在样本量足够大的时候。之后，当你换到一个新环境，甚至是接触一个冷启动的产品，那将面临巧妇难为无米之炊的情况，一度陷入“特征荒”的局面。

简而言之，由下至上抽象出日常工作中的特征，将平台能力赋能自我成长。另外，玄学来讲，策略打法也会因产品阶段、外部环境的变化而变化，似乎也需要记录些什么。

## 怎么做？

首先，顶层的强假设在于我们的短中期职业规划在于信贷风控领域，这个特征（/变量）也是围绕着信贷风控领域的。同时，我们在这短中期窗口内可能会经历多家公司或多个产品。所以，变量整理、维护的思路依然沿用了[卡片笔记](https://unclehuzi.github.io/2022/05/notes-zettelkasten_method/)的思想：由下至上规整，同时由上至下反哺，以此形成正循环♻️

![循环反哺.drawio.png](%E5%9F%BA%E4%BA%8ENotion%E7%9A%84%E4%BF%A1%E8%B4%B7%E9%A3%8E%E9%99%A9%E7%89%B9%E5%BE%81%E6%A1%86%E6%9E%B6%20674f119297aa4e8581645b6c288d41db/上下反哺_drawio.png)

在策略、模型工作中，往往都是局部最优的结果，会遇到些有效的特征以满足当时的需求，在这个时点我们记录下这些特征，以及在A公司环境下实现的方案。正如上文所述，我们短中期仍聚焦信贷风控领域且未来也可能会换工作，为了方便，我们就需要一个再上一层的架构，凌驾于公司之上、关注特征本身。如下图所示

![信贷风险领域特征框架示例](%E5%9F%BA%E4%BA%8ENotion%E7%9A%84%E4%BF%A1%E8%B4%B7%E9%A3%8E%E9%99%A9%E7%89%B9%E5%BE%81%E6%A1%86%E6%9E%B6%20674f119297aa4e8581645b6c288d41db/Untitled.png)

同时考虑到后续维护的便捷性，这里基于关系型数据库的思想主要建立了三张表：

- 一是信贷风控领域可用的数据源及其分类；

- 二是对应的特征；

- 三是基于所在公司数据环境的落地方案

  这里又有点“数据集市”的味道。仍可以在各张表中加入各自的属性，比如特征表中可标明调额、过退等应用场景。
  

### 为什么是Notion

其实在工具选择上也纠结和很久，也想用能呈现关系网的思源等软件。但其实能满足以下俩条件即可

1. 上文所述的框架
2. 支持跨设备等便捷性

而我最终选择Notion，其实更多是在于转化成本。截至目前，Notion已经是我All In One的生产力工具，唯一担心的就是Notion哪天黄了，数据全没了o.o


<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">

