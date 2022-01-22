# 可视化决策树结果


RT，可视化决策树结果，直观感受决策流程

<!--more-->

早之前可视化领域专家、学者提出之后要结合各种机器学习算法，制作相应可视化的图标以及工具。比如 TensorFlow 的可视化工具包—TensorBoard

针对结构化数据建模的算法中，树模型是较为常见的，[之前](https://mp.weixin.qq.com/s/g4GJjLcpz5_alaooneBvTA) 一直用sklearn自带的`tree_plot()`函数或`Graphviz`

{{< figure src="./treePlot.png" alt="./treePlot.png" title="tree_plot" >}}

最近发现了一个可视化树模型结果的`package`[^1]，画出来的图长这样👇

{{< figure src="https://raw.githubusercontent.com/parrt/dtreeviz/master/testing/samples/iris-TD-3-X.svg" alt="https://raw.githubusercontent.com/parrt/dtreeviz/master/testing/samples/iris-TD-3-X.svg" title="iris-TD-3-X" >}}

各个节点的分布情况也比较清楚，直观感受决策树的逻辑

更多代码示例可参考 [这个](https://raw.githubusercontent.com/parrt/dtreeviz/master/testing/gen_samples.py) 或 [这个](https://colab.research.google.com/github/parrt/dtreeviz/blob/master/notebooks/examples.ipynb)

{{< admonition type=warning title="图片格式" open=True >}}
但目前图片格式只支持 **svg**
{{< /admonition >}}


这里主要记录下安装说明

## 安装[^2]

[原文](https://github.com/parrt/dtreeviz#install)

**Python 版本 >= 3.6**


### 安装 graphviz

确保  `graphviz` 是 通过 `pip` 的方式安装的。

如果有装 Anaconda，又不确定是不是通过 `pip` 安装的，就走遍流程👇

```shell
conda uninstall python-graphviz
conda uninstall graphviz
```


```shell
pip install graphviz
```

接下来以 Windows 为例，

#### [下载 graphviz.msi](https://graphviz.org/download/) 并更新Path环境变量

假设保存路径为 `C:\Program Files (x86)\Graphviz2.38`，则将

`C:\Program Files (x86)\Graphviz2.38\bin` 添加至用户变量（`Path`）

`C:\Program Files (x86)\Graphviz2.38\bin\dot.exe` 添加至系统变量（`PATH`）

可以通过 `where dot` 验证是否安装成功

```shell
(base) C:\Users\Terence Parr>where dot
C:\Program Files (x86)\Graphviz2.38\bin\dot.exe
```


### 安装 dtreeviz

通过 `pip` 安装，Windows 可以直接打开 Anaconda Prompt

```shell
pip install dtreeviz             # install dtreeviz for sklearn
pip install dtreeviz[xgboost]    # install XGBoost related dependency
pip install dtreeviz[pyspark]    # install pyspark related dependency
pip install dtreeviz[lightgbm]   # install LightGBM related dependency
```


{{< admonition type=info title="决策树" open=True >}}
[这里](http://www.r2d3.us/visual-intro-to-machine-learning-part-1/)有酷炫的决策树介绍文档
{{< /admonition >}}

{{< figure src="./cool_tree.png" alt="./cool_tree.png"  >}}

{{< figure src="./cool_tree2.png" alt="./cool_tree2.png"  >}}



## Reference

[^1]: https://explained.ai/decision-tree-viz/index.html
[^2]: https://github.com/parrt/dtreeviz#install





<head>
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script>
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script>
</head>
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">

