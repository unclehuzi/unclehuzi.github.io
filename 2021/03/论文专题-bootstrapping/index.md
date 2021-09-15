# 【论文专题】Bootstrapping


Bootstrapping基本思想

<!--more-->

---

之前几篇关于

* [中介效应](https://mp.weixin.qq.com/s?__biz=MzU1Mzc0MTEzMA==&mid=2247483844&idx=1&sn=e5576f41f1785aa570ab49cffa0ca6a4&scene=21#wechat_redirect)

* [有调节的中介效应模型](https://mp.weixin.qq.com/s?__biz=MzU1Mzc0MTEzMA==&mid=2247483875&idx=1&sn=715ebcc2c1eb726034dbe48b50af5982&scene=21#wechat_redirect)

文章中都有提到 ***Bootstrapping***

这篇就简单的记录Bootstrapping

平时在做分析的时候，总体的数据往往是比较难获取的

所以，我们会基于抽样理论，从总体中抽取具有代表性的样本去估计总体的信息

比如用样本均值 $\bar x$ 估计总体均值 $\mu$ ；样本方差 $S^2$ 估计总体方差 $\sigma^2$ 等等

或者用区间估计的方法给出总体的均值的置信区间

本文以样本均值估计总体均值为例，探究Bootstrap方法的实现方式

Bootstrapping[^1] 是一种重采样的方法（**resampling method**）

之前有提到，从总体中进行抽样，得到样本数据， $x_1,x_2,...,x_n$

而Bootstrapping呢，从得到的样本中再进行有放回的抽样

如果抽样个数等于样本数据个数（$n$），则称其为 ***Bootstrap Sample***

$$
S_i^*={x_{i1}^*,x_{i2}^*,...,x_{in}^*}
$$

以这种方式不断的对样本进行重采样，就会得到 **bootstrap samples**，$S^*$

$$
S^*={S_{1}^*,S_{2}^*,...,S_{R}^*}
$$

其中，$R$ 一般要大于等于1000

$R$ 便是对应着 ***number of bootstrap samples*** 的选择

Bootstrapping 估计样本均值就是先分别计算 $R$ 个Bootstrap Sample的均值，最后再计算 $R$ 个均值的均值

Python代码示例

```python
import numpy as np

# 假设总体为X
X = np.random.normal(size=10000000)

# 抽样
n = 1000
x_i = np.random.permutation(X)[:n]

# Bootstrapping估计
R = 5000
boot_samples = [x_i[np.random.randint(0,n,n)].mean() for _ in range(R)]

boot_mean = np.sum(boot_samples) / R
```

当然，也可以给出均值的 Bootstrap置信区间

需要先将每个Bootstrap Sample得到的均值从小到大排序

进而计算 $\alpha /2$ 及 $(1- \alpha/2)$ 分位数作为区间的上下限

```python
s_sorted = np.sort(boot_samples)
alpha = 0.05
s_sorted[[round(R*alpha/2), round(R*(1-alpha/2))]]
```


## 参考资料

[^1]: Brad Efron: http://www.jstor.org/discover/10.2307/2958830?uid=3739568&uid=2&uid=4&uid=3739256&sid=21102342537691



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
