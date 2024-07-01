# Customer Lifetime Value


客户终生价值-CLV

<!--more-->




## 简介

企业要活下去，终究是要讲究盈利的，尽管现代营销理论中谈及的 customer-based，终点也是获利。

![](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220218224236375.png)

为此企业花钱获客、投广告、促销、让利等等。但总有些用户是不买账的，没啥付费意愿，拉低了盈利。就像有人会买电商等平台的会员，但也有人不会购买这个会员。从企业盈利的角度来看，企业需要识别这些行为模式，细分客户并采取相应的行动。like

![image-20220218224642471](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220218224642471.png)

本文将记录如何用 [`Lifetimes`](https://github.com/CamDavidsonPilon/lifetimes) 测算客户终生价值（**C**ustomer **L**ifetime **V**alue, ***CLV***）

![lifetimes-pkg](https://raw.githubusercontent.com/unclehuzi/pic/master/img/687474703a2f2f692e696d6775722e636f6d2f3773336a715a4d2e706e67.png)

一顿扒，大胆推测 `Lifetimes` 这个包的[作者](https://github.com/CamDavidsonPilon) 在 Data Science 的工作中有遇到相关的问题，一不做二不休基于相关论文、manual、R的包等等参考资料，整了个Python版本。（是我羡慕及崇拜的样子）

整体纵向来看，`Lifetimes` 分为两大部分：一是数据准备等基建工作；二是模型等应用层。前者“基建”工作主要包括 `utils.py`里的数据处理部分，将客户维度的明细数据，汇总为 `Frequency`, `Recency`, `T`, `Monetary_value`  维度；以及切分时间外样本等。后者应用层部分主要是各Model的实现，基于利益细分为两大部分：（1）能预测未来交易的Model（transaction_prediction_model），譬如 Pareto/NBD, BG/NBD Model；（2）测算CLV的模型`gamma_gamma_filter.GammaGammaFilter` 。

这里的“Model”是传统统计学的方法，假设 xxx 服从xxx分布，进行统计建模，完成未来交易次数、CLV的估计。

For all models, the following nomenclature is used:

- `frequency`
	
	represents the number of repeat purchases the customer has made. This means that it’s one less than the total number of purchases. This is actually slightly wrong. 
	
	***It’s the count of time periods the customer had a purchase in***. So if using days as units, then it’s the count of days the customer had a purchase on.

- `T` 

	观测时点 - 第一次购买时间    
	
	represents the age of the customer in whatever time units chosen (weekly, in the above dataset). This is equal to the duration between a customer’s first purchase and the end of the period under study.

- `recency`

	观测时点内，最近一次购买时间 - 第一次购买时间
	
	represents the age of the customer when they made their most recent purchases. This is equal to the ***duration*** between a customer’s first purchase and their latest purchase. (Thus if they have made only 1 purchase, the recency is 0.)

- `monetary_value`

	⚠️ 均值
	
	⚠️ 客户维度的 value，譬如利润、收益

	represents the average value of a given customer’s purchases. This is equal to the sum of all a customer’s purchases divided by the total number of purchases. Note that the denominator here is different than the frequency described above.

以客户维度的明细数据为例完成 CLV 的测算


![](https://miro.medium.com/max/1120/1*XgB-Sjd4ZuwQgvxLijnnDw.gif)

## CLV

假设有张表（transaction_data）记录着客户交易的明细数据，形如

| cust_no | date       | amt   |
| ------- | ---------- | ----- |
| a1      | 2020-02-15 | 113.3 |
| a2      | 2020-02-15 | 213   |
| a1      | 2020-03-05 | 12    |
| a3      | 2020-02-25 | 4.66  |

### 变量衍生

```python
from lifetimes.utils import summary_data_from_transaction_data

summary_with_money_value = summary_data_from_transaction_data(transaction_data
                                                                ,customer_id_col =  'cust_no'
                                                                ,datetime_col = 'date'
                                                                , monetary_value_col = 'amt'
                                                                , observation_period_end='2022-02-15'
                                                                )

returning_customers_summary = summary_with_money_value[summary_with_money_value['frequency']>0]
# returning_customers_summary.head()
```

[Paper](https://www.researchgate.net/publication/228351678_RFM_and_CLV_Using_iso-value_curves_for_customer_base_analysis) 中是假设交易频次和金额是独立的

> Monetary value is independent of the underlying transaction process.

![image-20220304154047837](https://raw.githubusercontent.com/unclehuzi/pic/master/img/image-20220304154047837.png)

### 计算

```python
from lifetimes import GammaGammaFitter
from lifetimes import BetaGeoFitter


# """Beta Geo Fitter, also known as BG/NBD model."""
# similar API to scikit-learn and lifelines.
bgf = BetaGeoFitter(penalizer_coef=0.0)
## 数据量小的时候调整 penalizer_coef（0.001 to 0.1）
bgf.fit(returning_customers_summary['frequency'], returning_customers_summary['recency'], returning_customers_summary['T'])
#print(bgf)

ggf = GammaGammaFitter(penalizer_coef = 0)
ggf.fit(returning_customers_summary['frequency'],
        returning_customers_summary['monetary_value'])


clv_df = pd.DataFrame(ggf.customer_lifetime_value(
    bgf, #the model to use to predict the number of future transactions
    returning_customers_summary['frequency'],
    returning_customers_summary['recency'],
    returning_customers_summary['T'],
    returning_customers_summary['monetary_value'],
    time=12, # months
    discount_rate=0.01 # monthly discount rate ~ 12.7% annually
)).reset_index()

```

这个方法更多是针对存量户测算CLV，在观测时点内`Frequency>0`，对于新户或者说没发生交易的用户来说就不适用了。不过，看着似乎可以参考 🔗[这个](https://towardsdatascience.com/data-driven-growth-with-python-part-3-customer-lifetime-value-prediction-6017802f2e0f) 的思路。结合测算后的CLV join 些 X，训练个回归模型，实现CLV的预测

## Reference

1. ***Principles of Marketing***, 17th.
2. https://github.com/CamDavidsonPilon/lifetimes
3. ***Counting your customers: who are they and what will they do next?***
4. ***“Counting Your Customers” the Easy Way: An Alternative to the Pareto/NBD Model***
5. ***RFM and CLV: Using iso-value curves for customer base analysis***








<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
