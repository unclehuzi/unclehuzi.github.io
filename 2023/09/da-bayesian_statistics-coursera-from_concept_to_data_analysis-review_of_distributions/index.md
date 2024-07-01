# coursera课程-Review of distributions




<!--more-->

## **Bernoulli and binomial distributions**

**Bernoulli distribution** is used when we have **two possible outcomes**, such as flipping a coin, where it could be heads and tails, or the cases where we have a success or a failure.

$X \sim B(p)$ : A random variable $X$ follows a Bernoulli distribution with probability $p$, where $p$ is probability of success, or probability of heads.

这里用 **Indicator Function** 表示$x$ 取值为0,1 的情况

$$
\begin{align*}
f(X=x \mid p) &= f(x \mid p) \newline
&= p^x(1-p)^{1-x} I_{\{x \in \{0,1\} \}}(x) \newline
\end{align*}
$$

> 「ChatGPT-3.5」
The indicator function, also referred to as the characteristic function, is a mathematical construct used to **represent whether a certain condition is satisfied or not.** It is commonly denoted by symbols such as `I` or `1`. The indicator function takes an element from a set and outputs either 1 or 0, depending on whether the element meets a specified condition.
> 
> 
> In formal terms, for a set `A` and an element `x`, the indicator function `I_A(x)` is defined as:
> 
> $$
> \begin{align*}
>   I_A(x) =
>   \begin{cases}
>     1 & \text{if $x \in A$ (x belongs to $A$) } \newline
>     0 & \text{if $x \notin A$ (x does not belong to $A$) }
>   \end{cases}
> \end{align*}
> $$
> 
> In simpler words, the indicator function serves as a way to "indicate" whether an element is part of a set (condition is true) or not (condition is false).
> 
> Indicator functions find applications in various mathematical fields, including probability theory, statistics, and analysis. They are used to express events, define indicator variables, calculate expected values, and simplify mathematical expressions involving conditions or events.
> 

> In some textbooks, they make a strong distinction between **discrete variables** where these were ***probably mass functions***, and **continues variables** where these are ***probability density functions***.
> 

**Bernoulli distribution**的期望和方差

$$
\begin{align*}
E(X) &= \sum_{x} x \cdot P(X=x) \newline
&=  1 \cdot p + 0 \cdot (1-p)  \newline
&= p
\end{align*}
$$

$$
\begin{align*}
Var(X) &= \sum_{x} (x- E(x))^2 \cdot P(X=x) \newline
&=  (1-p)^2 \cdot p + (0-p)^2 \cdot (1-p)  \newline
&= p(1-p)
\end{align*}
$$

The generalization of the Bernoulli when we have ***N repeated trials*** is a **Binomial**. $X \sim Bin(n,p)$

$$
P(X = x \mid p) = \binom{n}{x} p^x (1 - p)^{n - x} \newline
\binom{n}{x} = \frac{n!}{x!(n-x)!}   \; \text{for $x \in \{ 0,1,...,n \} $}
$$

期望和方差：$E(X)=np, \; Var(X)=np(1-p)$

## Uniform distribution

We can define a continuous random variable based on its **probability density function**(**PDF**).

以Uniform distribution为例， $X \sim U(0,1)$

$$
\begin{align*}
  f(x) &=
  \begin{cases}
    1 & x \in [0,1] \newline
    0 & \text{otherwise}
  \end{cases} \newline
&= I_{\{0 \leq x \leq 1\}}(x)
\end{align*}
$$

![Untitled](Review%20of%20distributions%20d8d220dc54f149cc9516e97e5dab39d5/Untitled.png)

- Python 生成 $U(0,1)$ PDF
  
    ```python
    # FROM ChatGPT-3.5
    import numpy as np
    import matplotlib.pyplot as plt
    
    x = np.linspace(0, 1, 1000)  # 创建一个横坐标范围从0到1的数据点
    y = np.ones_like(x)  # 每个数据点的纵坐标值都是1，因为在U(0,1)均匀分布中概率密度是常数
    
    fig, ax = plt.subplots()
    ax.plot(x, y, color='blue')
    ax.set_xlabel('x')
    ax.set_ylabel('$f(x)$')
    ax.set_title('$X \sim U(0,1)$')
    
    # 设置纵轴上限为1.1
    ax.set_ylim(0, 1.25)
    
    # 去除上边框和右边框
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    
    # 绘制垂直线段
    plt.plot([0, 0], [0, 1], color='gray', linestyle='dashed', linewidth=1)
    plt.plot([1, 1], [0, 1], color='gray', linestyle='dashed', linewidth=1)
    
    plt.show()
    ```
    

$$
P(0<x< \frac{1}{2})=\int_{0}^{\frac{1}{2}} f(x) \, dx=\frac{1}{2}=P(0 \leq x \leq \frac{1}{2}) \newline
P(x=\frac{1}{2})=0
$$

Some key rules for probability density functions: $\int_{-\infty}^{\infty} f(x) \, dx=1$；$f(x) \geq 0$

通用型Uniform distribution，$X \sim U(\theta_1,\theta_2)$  ⇒ $f(x|\theta_1,\theta_2)=\frac{1}{\theta_2-\theta_1}I_{\{\theta_1 \leq x \leq \theta_2 \}}(x)$

关于连续型随机变量的期望 the expected value for a continuous random variable

$$
\begin{align*}
& E(X)=\int_{-\infty}^{\infty} x f(x) \, dx \newline
& E(g(X))=\int g(x)f(x) \, dx \newline
& E(CX)=cE(x) \text{,$c$为常数} \newline
& E(X+Y) = E(X) + E(Y) \newline
& \text{if $X \perp Y$, } E(XY)=E(X)E(Y)
\end{align*}
$$

## **Exponential and normal distributions**

### Exponential distribution

> 「ChatGPT-3.5」
> 
> 
> The **Exponential distribution** is a mathematical model often used to **describe the time** *between events in situations* where *events occur randomly and independently at a constant average rate.* It's commonly used in fields like queuing theory, reliability analysis, and telecommunications.
> 
> Key points about the Exponential distribution:
> 
> - **Probability Density Function (PDF)**: The distribution is defined by the equation ( $f(x | \lambda) = \lambda e^{-\lambda x} ，\text{for $x \geq 0$}$ ), where ($x$) is the time between events, and ($\lambda$) is the rate parameter.
> - **Memorylessness**: It has the property that the **probability** of *an event occurring in a given interval* is **not influenced by the past**. This means that the distribution of time until the next event remains the same, ***regardless of** how much time has already passed.*
> - **Cumulative Distribution Function (CDF)**: The CDF is ( $F(x | \lambda) = 1 - e^{-\lambda x}$ ), which provides the probability that an event will occur *before a certain time* ($x$).
> - **Mean and Variance**: The mean (average) time between events is ($1/\lambda$), and the variance is ($1/\lambda^2$).
> 
> The Exponential distribution is widely used due to its simplicity and relevance in modeling various real-world scenarios involving random occurrences over time.
> 

![Untitled](Review%20of%20distributions%20d8d220dc54f149cc9516e97e5dab39d5/Untitled%201.png)

- code
  
    ```python
    import numpy as np
    import matplotlib.pyplot as plt
    import scipy.stats as stats
    
    # Parameters for the exponential distributions (rate)
    params = [
        (0.5, 'λ=0.5'),
        (1, 'λ=1'),
        (2, 'λ=2')
    ]
    
    # Create a figure and axis
    fig, ax = plt.subplots(figsize=(10, 6))
    
    # Plotting
    for lambd, label in params:
        x = np.linspace(0, 10, 100)
        y = lambd * np.exp(-lambd * x)
    		#y = stats.expon(scale=1/lambd).pdf(x)
        ax.plot(x, y, label=label)
    
    ax.set_xlabel('x')
    ax.set_ylabel('Probability Density')
    ax.set_title('Exponential Distribution')
    ax.legend()
    ax.grid(True)
    # 去除上边框和右边框
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    
    # Display the plot
    plt.show()
    ```
    

### Normal distribution

> 「ChatGPT-3.5」
> 
> 
> The **Normal distribution**, also known as the Gaussian distribution, is a bell-shaped curve that describes the distribution of many types of data. It is symmetrical and its mean, median, and mode are all equal.
> 
> Key points:
> 
> - **Bell-shaped**: Its graph is a symmetrical bell-shaped curve.
> - **Parameters**: It is defined by two parameters: the mean ($\mu$) and the standard deviation ($\sigma$).
> - **68-95-99.7 Rule**: About 68% of the data falls within one standard deviation of the mean, 95% within two standard deviations, and 99.7% within three.
> - **Applications**: It's widely used in statistics, science, and engineering due to many natural phenomena and processes being approximately normally distributed.
> 
> The formula for its probability density function (PDF) is:
> 
> $$
> f(x | \mu, \sigma) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}
> $$
> 
> However, for many practical applications, understanding its bell shape and the 68-95-99.7 rule is often more crucial than knowing its exact formula.
> 

![Untitled](Review%20of%20distributions%20d8d220dc54f149cc9516e97e5dab39d5/Untitled%202.png)

- code
  
    ```python
    import numpy as np
    import matplotlib.pyplot as plt
    import scipy.stats as stats
    
    # Parameters for the normal distributions
    params = [
        (0, 1, '$\mu=0, \sigma=1$'),
        (2, 0.5, '$\mu=2, \sigma=0.5$'),
        (-2, 2.5, '$\mu=-2, \sigma=2.5$')
    ]
    
    fig, ax = plt.subplots(figsize=(10, 6))
    
    for mu, sigma, label in params:
        x = np.linspace(mu - 3*sigma, mu + 3*sigma, 100)
        y = stats.norm.pdf(x, mu, sigma)
        ax.plot(x, y, label=label)
        
    # 去除上边框和右边框
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    
    plt.xlabel('x')
    plt.ylabel('Probability Density')
    plt.title('Normal Distribution')
    plt.legend()
    plt.grid(True)
    plt.show()
    ```
    

## **Supplementary material for Lesson 3**

[https://d3c33hcgiwev3.cloudfront.net/_29f7eaaba3ac0d35ee74f8bd61aafc39_L3_supp.pdf?Expires=1692144000&Signature=KA2KF0vx3gwhBdV~Qp1XAF3vmfziOf~pLoeyXkGI1sDGXz5P33eynAVVDQoyGfg3mFzxs0Jil10bamlf2z6rm8fb3cqMK7mVH5D~BjvT-W0MxKqjKUKvJsiiq6g1u23yDIWzSOxb39qZp1ISLzVL27Pqkdb6YgaqxrjsxaFAaOE_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A](https://d3c33hcgiwev3.cloudfront.net/_29f7eaaba3ac0d35ee74f8bd61aafc39_L3_supp.pdf?Expires=1692144000&Signature=KA2KF0vx3gwhBdV~Qp1XAF3vmfziOf~pLoeyXkGI1sDGXz5P33eynAVVDQoyGfg3mFzxs0Jil10bamlf2z6rm8fb3cqMK7mVH5D~BjvT-W0MxKqjKUKvJsiiq6g1u23yDIWzSOxb39qZp1ISLzVL27Pqkdb6YgaqxrjsxaFAaOE_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A)

## Additional Discrete Distributions

### Geometric distribution

The geometric distribution is ***the number of trials*** needed to get the first success, i.e., the number of Bernoulli events until a success is observed, such as the first head when flipping a coin. It takes values on the positive integers starting with one (since at least one trial is needed to observe a success).

$$
X \sim Geo(p) \newline
P(X=x|p)=p(1-p)^{x-1} \text{for $x=1,2,...$} \newline
E(X)=\frac{1}{p}
$$

If the probability of getting a success is $p$, then the expected number of trials until the first
success is $\frac{1}{p}$.

🌰 **Example**: What is the probability that we flip a fair coin four times and don’t see any heads?
This is the same as asking what is $P(X > 4)$  where $X \sim Geo(1/2)$ .

$$
\begin{align*}
P(X>4) &= 1-P(X=1)-P(X=2)-P(X=3)-p(X=4) \newline
&=1-\frac{1}{2}-\frac{1}{2} (\frac{1}{2})-\frac{1}{2} (\frac{1}{2})^2-\frac{1}{2} (\frac{1}{2})^3 \newline
&= \frac{1}{16}
\end{align*}
$$

![Untitled](Review%20of%20distributions%20d8d220dc54f149cc9516e97e5dab39d5/Untitled%203.png)

- code
  
    ```python
    import numpy as np
    import matplotlib.pyplot as plt
    from scipy.stats import geom
    
    # Parameters for the geometric distribution
    params = [
        (0.3, '$p=0.3$'),
        (0.5, '$p=0.5$'),
        (0.7, '$p=0.7$')
    ]
    
    fig, ax = plt.subplots(figsize=(10, 6))
    
    for p, label in params:
        x = np.arange(1, 16)
        y = geom.pmf(x, p)   # Probability mass function
        ax.plot(x, y, marker='o', linestyle='-', label=label)
    
    # Remove top and right borders
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    
    plt.xlabel('Number of Trials')
    plt.ylabel('Probability')
    plt.title('Geometric Distribution')
    plt.legend()
    plt.grid(True)
    plt.show()
    ```
    

### Multinomial distribution

Another generalization of the Bernoulli and the binomial is the ***multinomial distribution***, which is ***like a binomial*** when there are ***more than two possible outcomes***.

Suppose we have $n$ trials and there are $k$ different possible outcomes which occur with probabilities $p_1,p_2,...,p_k$ .

For example, we are rolling a six-sided die that might be loaded so that the sides are not equally likely, then $n$ is the total number of rolls, $k=6$, $p_1$ is the probability of rolling a one, and we denote by $x_1,...,x_6$ a possible outcome for **the number of times** we observe rolls of each of one through six, where $\sum\limits_{i=1}^{6}x_i=n \text{ and } \sum\limits_{i=1}^{6}p_i=1$.

$$
f(x_1,\dots ,x_k|p_1,\dots ,p_k)=\frac{n!}{x_1!\dots x_k !}p_1^{x_1}\dots p_k^{x_k}
$$

![Untitled](Review%20of%20distributions%20d8d220dc54f149cc9516e97e5dab39d5/Untitled%204.png)

- 模拟code
  
    ```python
    import numpy as np
    import matplotlib.pyplot as plt
    from scipy.stats import multinomial
    # set seed for reproductibility
    np.random.seed(999) 
    
    # Parameters for the multinomial distribution
    k = 3
    n = 12 # number of trials (games in one tourment)
    pvals = [0.4, 0.35, 0.25]
    
    sizes =[] # number of tournments played
    p = []    # a list to hold ratios (converge to prob) that player 1 wins 7 times, player 2 wins 2 times and 3 ties
    
    # 公式计算
    f_x = round(multinomial.pmf([7,2,3],n=n,p=pvals),4)
    
    # 模拟
    fig, ax = plt.subplots(figsize=(10, 4))
    
    for size in np.logspace(2,4):
        # the line below is where we actually generate discrete random variables according the multinomial distribution
        outcomes = np.random.multinomial(n, pvals, size=int(size))
        
        # let's count the ratio of the expected outcome over all the outcomes - this will lastly converge to the probability
        prob = sum((outcomes[:,0]==7)&(outcomes[:,1]==2)&(outcomes[:,2]==3))/len(outcomes)
        
        p.append(prob)
        sizes.append(int(size))
    
    ax.plot(sizes,p,'o-')
    ax.plot(sizes,[f_x]*len(sizes),'--r')
    
    line1 = r"$p_1,p_2,p_3=(0.4, 0.35, 0.25)$"  # 文本内容
    line2 = r"$x_1,x_2,x_3=(7,2,3)$"  # 文本内容
    
    x_pos = 0.95  # x 坐标位置（相对于轴范围的比例）
    y_pos = 0.95  # y 坐标位置（相对于轴范围的比例）
    ax.text(x_pos, y_pos, line2+"\n"+line1, transform=ax.transAxes, fontsize=12,
            ha='right', va='top')
    # Remove top and right borders
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    
    plt.xlim(xmin=0)
    plt.xlabel('Number of Drawings')
    plt.ylabel('$p(x_1,x_2,x_3|p_1,p_2,p_3)$')
    plt.title('Theoretical $f(x_1,x_2,x_3|p_1,p_2,p_3)={}$'.format(f_x))
    
    #plt.grid(True)
    plt.show()
    ```
    

### Poisson distribution

The Poisson distribution is used for counts, and arises in a variety of situations. The parameter $\lambda > 0$ is the ***rate*** at which we expect to observe the thing we are counting.

$$
X \sim Pois(\lambda) \newline
P(X=x|\lambda)=\frac{\lambda^xexp(- \lambda)}{x!} \text{ for $x=0,1,2,\dots$} \newline
E(X)=\lambda \newline
Var(X) =\lambda
$$

🌰 **Example**: Significant earthquakes occur in the Western United States approximately following a Poisson process with rate of two earthquakes per week. What is the probability there will be at least 3 earthquakes in the next two weeks?

未来两周发生地震的次数记为$X$，由题意得，$X \sim Pois(4)$. 

$$
\begin{align*}
P(X\geq 3) &= 1-P(X\leq2) \newline 
&=1- P(X=0)- P(X=1)- P(X=2) \newline
&=1 - \frac{4^0 exp(- 4)}{0!} -\frac{4^1 exp(- 4)}{1!} - \frac{4^2 exp(- 4)}{2!} \newline
&= 1-13e^{-4} \newline
&=0.762
\end{align*}
$$

## Continuous Distributions

### Gamma

If $X_1,X_2,\dots,X_n$ are independent (and identically distributed $Exp(\lambda)$) ***waiting times*** between successive events, then the total waiting time for all n events to occur $Y=\sum_{i=1}^{n}X_i$ will follow a gamma distribution with shape parameter $\alpha=n$ and rate parameter $\beta=\lambda$.

$$
Y \sim Gamma(\alpha,\beta) \newline
f(y|\alpha,\beta)=\frac{\beta^\alpha}{\Gamma(\alpha)}y^{\alpha-1}e^{-\beta y}I_{ \{y \geq0\}}(y) \newline
E(Y) = \frac{\alpha}{\beta} \newline
Var(Y) =  \frac{\alpha}{\beta^2}
$$

where $\Gamma(·)$ is the gamma function, a generalization of the factorial function which can accept non-integer arguments. If $n$ is a positive integer, then $\Gamma(n) = (n − 1)!$ . Note also that $\alpha> 0$ and $\beta > 0$ .

The exponential distribution is a special case of the gamma distribution with $\alpha = 1$. The gamma distribution commonly appears in statistical problems, as we will see in this course. It is used to model positive-valued, continuous quantities whose distribution is right-skewed. As $\alpha$ increases, the gamma distribution more closely resembles the normal distribution.

![Untitled](Review%20of%20distributions%20d8d220dc54f149cc9516e97e5dab39d5/Untitled%205.png)

- code
  
    ```python
    import numpy as np
    import matplotlib.pyplot as plt
    from scipy.stats import gamma
    
    # Parameters for the gamma distribution
    params = [
        (1, 0.5, '$\\alpha=1, \\beta=0.5$'),
        (5, 1, '$\\alpha=5, \\beta=1$'),
        (8, 2, '$\\alpha=8, \\beta=2$')
    ]
    
    fig, ax = plt.subplots(figsize=(10, 6))
    
    for alpha, beta, label in params:
        x = np.linspace(gamma.ppf(0.001, alpha, scale=1/beta), gamma.ppf(0.999, alpha, scale=1/beta), 100)
        y = gamma.pdf(x, alpha, scale=1/beta)   # Probability density function
        ax.plot(x, y, linestyle='-', label=label)
    
    # Remove top and right borders
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    
    plt.xlabel('X')
    plt.ylabel('Probability Density')
    plt.title('Gamma Distribution')
    plt.legend()
    plt.grid(True)
    plt.show()
    ```
    

### Beta

The beta distribution is used for random variables which **take on values between 0 and 1**. For this reason (and other reasons we will see later in the course), the beta distribution is commonly used to model probabilities

$$
X \sim Beta(\alpha,\beta) \newline
f(x|\alpha,\beta)=\frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)}x^{\alpha-1}(1-x)^{\beta -1}I_{ \{0<x<1\}}(x) \newline
E(X) = \frac{\alpha}{\alpha + \beta} \newline
Var(X) =  \frac{\alpha \beta}{(\alpha+\beta)^2(\alpha+\beta+1)}
$$

where $\Gamma(·)$ is the gamma function introduced with the gamma distribution. Note also that $\alpha> 0$ and $\beta > 0$ . The standard Uniform(0, 1) distribution is a special case of the betadistribution with $\alpha=\beta=1$ .

![Untitled](Review%20of%20distributions%20d8d220dc54f149cc9516e97e5dab39d5/Untitled%206.png)

- code
  
    ```python
    import numpy as np
    import matplotlib.pyplot as plt
    from scipy.stats import beta
    
    # Parameters for the Beta distribution
    params = [
        (2, 5, '$\\alpha=2, \\beta=5$'),
        (5, 1, '$\\alpha=5, \\beta=1$'),
        (2, 2, '$\\alpha=2, \\beta=2$')
    ]
    
    fig, ax = plt.subplots(figsize=(10, 6))
    
    for a, b, label in params:
        x = np.linspace(0, 1, 100)
        y = beta.pdf(x, a, b)   # Probability density function
        ax.plot(x, y, label=label)
    
    # Remove top and right borders
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    
    plt.xlabel('Value')
    plt.ylabel('Probability Density')
    plt.title('Beta Distribution')
    plt.legend()
    plt.grid(True)
    plt.show()
    ```
    

### t

If we have normal data, we can use $\bar{X} \sim N(\mu,\frac{\sigma^2}{n})$ to help us estimate the mean $\mu$. 标准正态分布： $\frac{\bar{X} - \mu}{\sigma/ \sqrt{n}} \sim N(0,1)$. However, we may not know the value of $\sigma$. If we estimate it from data, we can replace it with $S=\sqrt{\sum_i(X_i - \bar{X})^2/(n-1)}$ , the sample standard deviation. This causes the $\frac{\bar{X} - \mu}{\sigma/ \sqrt{n}}$ to no longer be distributed as standard normal, but as a standard t distribution with $\nu=n-1$ degrees of freedom.

$$
Y \sim t_{\nu} \newline
f(y) = \frac{\Gamma(\frac{\nu +1}{2})}{\Gamma(\frac{\nu}{2}) \sqrt{\nu \pi}}(1+\frac{y^2}{\nu})^{-(\frac{\nu + 1}{2})} \newline
E(Y) = 0 \text{, if $\nu > 1$ } \newline
Var(Y) = \frac{\nu}{\nu - 2} \text{, if $\nu > 2$}
$$

![Untitled](Review%20of%20distributions%20d8d220dc54f149cc9516e97e5dab39d5/Untitled%207.png)

- code
  
    ```python
    import numpy as np
    import matplotlib.pyplot as plt
    from scipy.stats import t
    
    # Parameters for the t distribution
    params = [
        (2, '$df=2$'),
        (5, '$df=5$'),
        (10, '$df=10$')
    ]
    
    fig, axes = plt.subplots(1, 2, figsize=(12, 6))
    
    # Plot probability density function
    axes[0].set_title('t Distribution - Probability Density Function')
    for df, label in params:
        x = np.linspace(-4, 4, 100)
        y = t.pdf(x, df)   # Probability density function
        axes[0].plot(x, y, label=label)
    
    # Plot cumulative distribution function
    axes[1].set_title('t Distribution - Cumulative Distribution Function')
    for df, label in params:
        x = np.linspace(-4, 4, 100)
        y = t.cdf(x, df)   # Cumulative distribution function
        axes[1].plot(x, y, label=label)
    
    # Remove top and right borders
    for ax in axes:
        ax.spines['top'].set_visible(False)
        ax.spines['right'].set_visible(False)
    
    axes[0].set_xlabel('Value')
    axes[0].set_ylabel('Probability Density')
    axes[1].set_xlabel('Value')
    axes[1].set_ylabel('Cumulative Probability')
    
    axes[0].legend()
    axes[1].legend()
    axes[0].grid(True)
    axes[1].grid(True)
    plt.tight_layout()
    plt.show()
    ```
    

> 作者：吴端
> 
> 
> 链接：https://www.zhihu.com/question/34866983/answer/1540230125
> 
> 来源：知乎
> 
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
> 
> 分布分为离散分布和连续分布。连续分布几乎都能找到对应的离散分布。因为连续分布本质上就是把离散分布中投硬币的次数，变成连续的时间长度。把每次投硬币的正反面概率，变成单位时间内出现正面的概率密度。
> 
> 下面我从贝努利实验出发，把所有分布的关系梳理出来：
> 
> 贝努利实验：投硬币，正面反面概率分别为p，1-p
> 
> 二项分布：如果把贝努利实验连续做n次，出现正面的次数服从的分布。
> 
> 二项分布的极限：泊松分布。给定时间内时间发生次数的分布。
> 
> 负二项分布：在贝努利实验中，如果想让正面出现n次，需要做的实验次数的分布。
> 
> 负二项分布的极限：gamma分布。问如果指定事件出现N次，需要等待的时间。
> 
> 二项分布与负二项分布的关系：二项分布是在固定实验次数情况下，问结果分布。负二项分布是在固定结果情况下问实验次数分布。一个是在固定投入问产出，一个是在固定产出下问投入。
> 
> 几何分布：n重贝努利实验，正面第一次出现时的实验次数。
> 
> 几何分布的极限：指数分布。事件第一次发生等待的时间。
> 
> 几何分布与负二项分布的关系：负二项分布是N个几何分布的和。相当于做N次几何分布，事件正好发生N次。各几何分布的实验次数之和就是负二项分布的总次数。
> 
> 指数分布与gamma分布的关系：N次指数分布时间之和就是gamma分布中事件发生N次等待总时间。
> 
> 如果课本能这样组织这些知识，估计也没有人困惑为啥整这么多奇怪的分布，到底有什么用。推倒过程反而没那么重要
> 

## Central Limit Theorem

The Central Limit Theorem is one of the most important results in statistics, basically saying
that with sufficiently large sample sizes, ***the sample average*** approximately follows a normal
distribution.

In formal mathematical notation, the Central Limit Theorem says: Let $X_1,\dots ,X_n$ be independent and identically distributed with $E(X_i)=\mu \text{ and } Var(X_i)= \sigma^2,0 < \sigma^2 < \infty$, then,

$$
\frac{\sqrt{n}(\bar{X}-\mu)}{\sigma} \sim N(0,1)
$$

![Untitled](Review%20of%20distributions%20d8d220dc54f149cc9516e97e5dab39d5/Untitled%208.png)



<script src="https://gist.github.com/unclehuzi/31feb53fe47d33b937fc70f7a71d1cf4.js"></script>




