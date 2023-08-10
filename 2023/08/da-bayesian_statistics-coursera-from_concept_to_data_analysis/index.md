# coursera课程-Probability and Bayes’ Theorem




coursera课程-简单回顾Probability and Bayes’ Theorem

<!--more-->



## Module overview

[https://d3c33hcgiwev3.cloudfront.net/CE-7SC5rEeayHwq9iLdlAw.processed/full/720p/index.mp4?Expires=1690502400&Signature=Bk1Qel4SY~MS5TN3fDOtD0JtZfs0T1itCrm4OGXHCKcfowhAZKg8TRGt8CZjZLSSfUn69bJU473N8NDhJt4THk5xg06KnOP~8AJio6GuKXR-SiqUJgxQM24xxmMLwWF80tJDrnqSyqlaHSRJRG6WOKRQWLHXFOwzbUNmeSAqPSM_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A](https://d3c33hcgiwev3.cloudfront.net/CE-7SC5rEeayHwq9iLdlAw.processed/full/720p/index.mp4?Expires=1690502400&Signature=Bk1Qel4SY~MS5TN3fDOtD0JtZfs0T1itCrm4OGXHCKcfowhAZKg8TRGt8CZjZLSSfUn69bJU473N8NDhJt4THk5xg06KnOP~8AJio6GuKXR-SiqUJgxQM24xxmMLwWF80tJDrnqSyqlaHSRJRG6WOKRQWLHXFOwzbUNmeSAqPSM_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A)

There are two main philosophies of probability and statistics, ***Bayesian*** and ***Frequentist***.

[Probability](https://www.notion.so/Probability-7f69aa1d4d9a4ea09d829ab6a57070ab?pvs=21) 模块儿会快速回顾些 frequent distant inference

视频内容方面比较少，更多是在exercises，labeled as quizzes

## Probability

### Background for Lesson 1

[https://d3c33hcgiwev3.cloudfront.net/_2ffa04a4be97e282ca9e65d96897a77c_L1_background.pdf?Expires=1690502400&Signature=Hn2xEnhq7C9UuERyL6S~XR0GNxNuupCf9ryTJHFVF92tpesy502YndG2csd5ae0kVQEXDveKgs-fUBb9VJss6gJrc8hdZTYlDNEX4~047I3mqHYbw40UqMNsDPWSmR~rcb7ZhgAU9qfVsPbzmIAxNkmJO6Umik86H4oo5zawwvI_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A](https://d3c33hcgiwev3.cloudfront.net/_2ffa04a4be97e282ca9e65d96897a77c_L1_background.pdf?Expires=1690502400&Signature=Hn2xEnhq7C9UuERyL6S~XR0GNxNuupCf9ryTJHFVF92tpesy502YndG2csd5ae0kVQEXDveKgs-fUBb9VJss6gJrc8hdZTYlDNEX4~047I3mqHYbw40UqMNsDPWSmR~rcb7ZhgAU9qfVsPbzmIAxNkmJO6Umik86H4oo5zawwvI_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A)

**Rules of Probability**

1. Probabilities must be between zero and one, i.e., 0 ≤ P(A) ≤ 1 for any event A.
2. Probabilities add to one, i.e., if we add up the probabilities of all possible events, those
probabilities must add to one.
3. The complement of an event, $A^ \complement$, means that the event does not happen. Since probabilities
must add to one, $P(A^\complement)=1-P(A)$   （之前见的是 $\overline{A}$）
4. $P(A \cup B)=P(A)+P(B)-P(A \cap B)$. where $\cup$ represents union (“or”) and $\cap$ represents intersection (“and”)

**Odds**

The odds for event A, denoted $\mathcal{O}(A)$ is defined as $\mathcal{O}(A)=\frac{P(A)}{P(A^ \complement)}=\frac{P(A)}{1-P(A)}$

**Expectation**

The expected value of a random variable $X$ is a weighted average of values $X$ can take, with
weights given by the probabilities of those values.

$$
E(X)=\sum_{i=1}^{n} x_i \cdot P(X=x_i)
$$

For example, the expected value of a fair six-sided die would be

$$
E(X)=\sum_{i=1}^{n} x_i \cdot P(X=x_i)=\sum_{i=1}^{n} i \cdot \frac{1}{6}=3.5
$$

Note that the die cannot achieve this value, but if you were to **roll the die many times and
average the values**, the result would likely be close to $3.5$.

### **Classical, frequentist and bayesian probability**

There are three different frameworks under which we can define probabilities.

- Classical framework
  
    outcomes that are equally likely have **equal** probabilities.
    
    ![Untitled](Probability%20and%20Bayes%E2%80%99%20Theorem%209c5c1098e5f84d6b8f23b5d45c2023ec/Untitled.png)
    
- Frequentist framework
  
    Frequentist definition, requires us to have a hypothetical infinite sequence of events, and then we look at the relevant **frequency**, in that hypothetical infinite sequence.
    
- Bayesian framework
  
    偏主观。尽管如此，大多数情况下结果也优于“频率学派”
    
    Bayesian perspective is one of **personal perspective**. Your probability represents your own perspective, it's your measure of uncertainty, and it takes into account what you know about a particular problem.
    
    So inherently a **subjective approach** to probability, but it can work well in a mathematically rigorous foundation, and it leads to much more intuitive results in many cases than the Frequentist approach.
    
    **Suppose you'd be willing to take the bet** that if it rains tomorrow, you win $4. If it doesn't rain tomorrow, you lose $1, or whatever your local currency is. 若认为公平，换个说法理应也会参加：if it rains, you lose $4. And if no rain, you win a $1.（插播：行为经济学告诉你，大多数人面对这俩说法的态度是不一样的，不一定就会选择后者，因为后者放大了损失。尽管数学上这俩收益的期望=0）
    

## Bayes’ theorem

**Bayes' theorem** is the theoretical underpinning of most of what we do within the Bayesian statistical framework.

### **Conditional probability**

Conditional probability is when we're trying to consider two events that are related to each other.

$$
P(A \mid B)=\frac{P(A \cap B)}{P(B)}
$$

🌰 一个例子：30个学生，9个女生；12个学计算机，学计算机中4个女生。 

$$
\begin{align*}
& P(Female)=\frac{9}{30}=\frac{3}{10} \\
& P(CS)=\frac{12}{30}=\frac{2}{5} \\
& P(Female \cap CS)=\frac{4}{30}=\frac{2}{15} \\
& P(Female \mid CS)=\frac{4}{12}=\frac{1}{3} \\
\end{align*}
$$

⇒

$$
\begin{align*}
P(CS \mid Female) &= \frac{P(CS \cap Female)}{P(Female)} \\ 
& = \frac{\frac{2}{15}}{\frac{3}{10}}=\frac{4}{9} \\
P(Female \mid CS^{\complement}) &= \frac{P(Female \cap CS^{\complement})}{P(CS^{\complement})} \\
&= \frac{\frac{5}{30}}{\frac{18}{30}}=\frac{5}{18}
\end{align*}
$$

There's a concept of **independence**, which is when one event doesn't depend on the other. When two events are independent, we have that the probability of A given B is equal to just the probability of A, it doesn't matter whether, or not B occurred. When this is true, we also get that the probability of A and B happening is just the probability of A times the probability of B.

事件A、B独立，⇒ $P(A \mid B)=P(A)$； $P(A\cap B)=P(A) \times P(B)$

### **Bayes' theorem**

$$
P(A \mid B)=\frac{P(A \cap B)}{P(B)}=\frac{P(B \mid A)\times P(A)}{P(B \mid A)\times P(A)+P(B \mid A^{\complement})\times P(A^{\complement})}
$$

🌰 一个例子：test for HIV antibodies known as the ELISA test

已知，$P(+ \mid HIV)=0.977,P(- \mid no HIV)=0.926,P(HIV)=0.0026$

⇒ 

$$
\begin{align*}

P(HIV \mid +) &= \frac{P(HIV \cap +)}{P(+)} \\ 
&=\frac{P(+ \mid HIV)\times P(HIV)}{P(+ \mid HIV)\times P(HIV)+P(+ \mid noHIV)\times P(noHIV)} \\
&= 0.033

\end{align*}
$$

> Bayes' Theorem is an important part of our approach to Bayesian statistics. We can use it to **update our information**. We start with prior beliefs, we'll collect data, we'll then condition on the data to lead to our posterior beliefs. Bayes' Theorem is the coherent way to do this updating.
> 

### **Supplementary material for Lesson 2**

[https://d3c33hcgiwev3.cloudfront.net/J9_9-JhwEeiAaxI_EQt0HA_2844d980987011e8a5a229e907589355_L2_supp-v2.pdf?Expires=1690761600&Signature=Etha8OmIFpPupX~gFpyvTVPM8PvzfuS5x6roInaqU4-15-Qgeq7nXFw3ifQuDCbCeHmB6YeX4-mDx0oiG8k1ZGmPAG0ZwghFpWuhy0NvVbTQRAGAbft0LoMurbdJL5HnRIstl3tRFWN8IPFq-zhZJj8cHXy7ONQv868oxALqtwM_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A](https://d3c33hcgiwev3.cloudfront.net/J9_9-JhwEeiAaxI_EQt0HA_2844d980987011e8a5a229e907589355_L2_supp-v2.pdf?Expires=1690761600&Signature=Etha8OmIFpPupX~gFpyvTVPM8PvzfuS5x6roInaqU4-15-Qgeq7nXFw3ifQuDCbCeHmB6YeX4-mDx0oiG8k1ZGmPAG0ZwghFpWuhy0NvVbTQRAGAbft0LoMurbdJL5HnRIstl3tRFWN8IPFq-zhZJj8cHXy7ONQv868oxALqtwM_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A)



<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
