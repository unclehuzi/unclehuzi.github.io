# Python曲线拟合




最近接触了曲线拟合（[curve fitting](https://en.wikipedia.org/wiki/Curve_fitting)），在此简单整理一波Python的实现方式

<!--more-->

依稀记得高中数学课本有提到这个，$x$​​、$y$​​​​ 二维坐标。大致是两种方式：一种是看着像啥样或基于先验知识给出常见函数的关系式，通过数据拟合得到相应的系数；第二种是直接从数据出发，采用“基函数”拟合，和泰勒展开、级数有关系，函数越复杂拟合的越完美，但**泛化**能力就有待考究了，即复杂度越高越容易出现**过拟合**

![](https://upload.wikimedia.org/wikipedia/commons/a/a8/Regression_pic_assymetrique.gif)

「插播」这两种又让我想起了统计里的“参数估计”和“非参数估计”

备注：以下分类名称仅从个人理解出发

## 自定义函数拟合

“自定义函数拟合”即我们可以自行编写定义各种函数（如幂函数、指数函数等）关系，基于此对现有数据进行拟合。往往需要一些领域内的知识🤔

具体实现可参考 [`scipy.optimize.curve_fit`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.curve_fit.html) 

### 官方示例

```python
import matplotlib.pyplot as plt
import numpy as np
from scipy.optimize import curve_fit

# 自定义函数
def func(x, a, b, c):
    return a * np.exp(-b * x) + c

# 构造数据
xdata = np.linspace(0, 4, 50)
y = func(xdata, 2.5, 1.3, 0.5)
rng = np.random.default_rng()
y_noise = 0.2 * rng.normal(size=xdata.size)
ydata = y + y_noise

# 拟合
popt, pcov = curve_fit(func, xdata, ydata)

## 设置参数取值范围
popt1, pcov1 = curve_fit(func, xdata, ydata, bounds=(0, [3., 1., 0.5]))

# 可视化
plt.plot(xdata, ydata, 'b-', label='data')
plt.plot(xdata, func(xdata, *popt), 'r-',
         label='fit: a=%5.3f, b=%5.3f, c=%5.3f' % tuple(popt))
plt.plot(xdata, func(xdata, *popt1), 'g--',
         label='fit: a=%5.3f, b=%5.3f, c=%5.3f' % tuple(popt1))

plt.legend()
plt.show()
```

![result](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220113162513531.png)

`curve_fit()` 的参数方面：

- `p0`

  系数初始值

- `bounds`

  各系数的取值范围 

- `method`

	最优化算法，'lm', 'trf', 'dogbox'

### MARK-log

此外还要 MARK 的一点是关于  $log$​ 的问题，Python中 `numpy` 和 `math` 都可以计算对数（$log$）

首先 `math.log`  和 `numpy.log` 都是以自然常数 $e$​​​ 为底的**自然对数**，针对底数不同各自都有以2、10为底的函数，分别为`log2()`, `log10()` 。其中，`math.log2(x)`, `math.log10(x)`计算的准确性高于 `math.log(x,2)`, `math.log(x,10)`

但是 `math.log()` 支持自选底数，比如计算 $log_{12}{10}$​​ ：`math.log(10,12)`

其实结合“换底公式”， 这个就等价于  `math.log(10)/math.log(12)`
$$
log_ab=\frac{log_ca}{log_cb}
$$
**但但但但是**，`numpy.log` 是支持数组、列表等形式的，享受 [broadcasting](https://numpy.org/doc/stable/user/basics.broadcasting.html)带来的快感；而 `math.log` 只支持单个数字的计算，若传入数组等 `array_like` 则会报错：`TypeError: only size-1 arrays can be converted to Python scalars`

![error](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220113175124951.png)

若一定要用 `math.log` 也是可以的： `[math.log(i) for i in [1,2,3,4,5]]`

![image-20220113180229076](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220113180229076.png)

## 基函数拟合

关于这部分，目前记录的 “基函数” 更多是多项式（polynomials）形式，`numpy`提供了 sub-package： [`numpy.polynomial`](https://numpy.org/doc/stable/reference/routines.polynomials.package.html#module-numpy.polynomial) （NumPy 1.4 版本，2009-12-28，引入的）

| **Name**                                                     | **Provides**     |
| ------------------------------------------------------------ | ---------------- |
| [`Polynomial`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.polynomial.Polynomial.html#numpy.polynomial.polynomial.Polynomial) | Power series     |
| [`Chebyshev`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.chebyshev.Chebyshev.html#numpy.polynomial.chebyshev.Chebyshev) | Chebyshev series |
| [`Legendre`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.legendre.Legendre.html#numpy.polynomial.legendre.Legendre) | Legendre series  |
| [`Laguerre`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.laguerre.Laguerre.html#numpy.polynomial.laguerre.Laguerre) | Laguerre series  |
| [`Hermite`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.hermite.Hermite.html#numpy.polynomial.hermite.Hermite) | Hermite series   |
| [`HermiteE`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.hermite_e.HermiteE.html#numpy.polynomial.hermite_e.HermiteE) | HermiteE series  |

Power series（[`Polynomial`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.polynomial.Polynomial.html#numpy.polynomial.polynomial.Polynomial)）就是我们常见的 $y=1+2x+3x^2$​ 这种

```python
import numpy as np
import matplotlib.pyplot as plt
from numpy import polynomial as P

x = np.array([10,20,30,40,50,60,70,80])
y = np.array([174,236,305,334,349,351,342,323])

# 3 表示想要拟合的最高次项是多少。
p = P.polynomial.Polynomial.fit(x,y,deg=3)

# 表达式
# print(p)
# 343.18750000000006 + 59.98042929292918·x¹ - 96.68750000000027·x² + 15.80744949494958·x³


yvals = p(x) #拟合y值

plt.plot(x, y, 's',label='original values')
plt.plot(x, yvals, 'r',label='Power series')
plt.legend()
plt.show()
```

![power series](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220114144138119.png)

### MARK

在官方文档会看到 `Polynomial` 这个类前面套了好几层`numpy.polynomial.polynomial.Polynomial`

![class-of-Polynomial](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220114140637878.png)

实则关系是 `numpy.polynomial` 下面有个 `polynomial.py` 文件，文件里有个类是 `Polynomial`。同理，剩下五种多项式也是如此，`chebyshev.py` 有个 `Chebyshev`类

![polynomial](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220114141225177.png)

在实际使用的时候可以严格遵守这种关系

```python
from numpy import polynomial as P

## 使用 Power series 拟合
p = P.polynomial.Polynomial.fit(x,y,deg=3)

## 使用 Chebyshev series 拟合
c = P.chebyshev.Chebyshev.fit(x,y,deg=2)

## 其他几种多项式类似

plt.plot(x, y, 's',label='original values')
plt.plot(x, p(x), 'r',label='Power series')
plt.plot(x, c(x), 'g--',label='Chebyshev series')
plt.legend()
plt.show()
```

![image-20220114144615412](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220114144615412.png)

## Reference

1. https://numpy.org/doc/stable/reference/generated/numpy.log.html
2. https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.curve_fit.html
3. https://docs.python.org/3.7/library/math.html
4. https://numpy.org/doc/stable/reference/routines.polynomials.html
5. https://numpy.org/doc/stable/reference/routines.polynomials.package.html#module-numpy.polynomial
6. https://numpy.org/doc/stable/reference/generated/numpy.polynomial.polynomial.polyfit.html#numpy.polynomial.polynomial.polyfit
7. https://numpy.org/doc/stable/reference/routines.polynomials.html
8. https://www.jianshu.com/p/44baeed131df





<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
