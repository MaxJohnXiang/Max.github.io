+++
title = "Money Management | 金融理财"
author = ["项云"]
date = 2020-06-13
lastmod = 2020-06-13T13:25:11+08:00
draft = false
katex = true
+++

## 股票基础 {#股票基础}


### 股票上涨 {#股票上涨}

买盘和卖盘, 有人愿意用10.01 卖, 没人愿意买. 此时股价不变, 如果有人愿意买股价变
持有方愿意更低价格卖出, 此时股价降低


### 股票上涨原因 {#股票上涨原因}

-   公司资产

<!--list-separator-->

-  市净率(PB)

    -   Market price per share(股价) / Book value per share(净资产)

<!--list-separator-->

-  市盈率(PE) Price to Earnings ratio

    <!--list-separator-->

    -  Definition

        -   Value Per Unit (股价) / Earning per unit (每股盈利金额)
        -   Market Value / Income production

    <!--list-separator-->

    -  Example:

        -   PPS(Price per share ) $10
        -   Earning per share
            -   2016Q1 : $0.5
            -   2016Q2 : $0.1
            -   2016Q3 : $0.1
            -   2016Q4 : $0.3

             PE ratio = 10 / (Q1 + Q2 + Q3 + Q4) 1 = 10
        2D points can be denoted using a pair of values, \\(x = (x, y) \in
        \mathbb{R}^2\\):

        \begin{equation}
          x = \begin{bmatrix}
            x \\\\\\
            y
          \end{bmatrix}
        \end{equation}


### 股票价格 {#股票价格}


#### 贴现 {#贴现}

<!--list-separator-->

-  复利

    今天的一块钱 大于明天的一块钱
    复利计算公式 : x ... x (1+r)  ... x (1 + r )2 ....  x (1 + r )n
    现在的价值  x / (1 +r ) ^ n

<!--list-separator-->

-  贴现率

    -   无风险利率 1.5 %
    -   风险补偿 1%
    -   通货膨胀 4%

<!--list-separator-->

-  银行的债券

    -   每年固定的分红, 是不复利

<!--list-separator-->

-  股票的估值

    -   静态估值 (和债券的计算方法一样)   分红的 r 无法确定
    -   动态估值 引入一个 g 增长率

    -- p = d (1 +r ) + d(1+g)/1+r)^2 ... = D / (r-g)
    -- p = 0.25, g = 4% , r = 8% 所以 p = 6.25

    -   自由现金流 不是每年都分红
        -   自由现金流 1.6 per share, g = 4% r = 8%  p = 40 元


## 基金种类 {#基金种类}


### 债券 {#债券}

{{< figure src="/ox-hugo/2020-06-12_21-48-51_v2-cf605af6c2ed208d429fe2416363ba93_720w.jpg" >}}

普通债券基金主要投资债券，债券和股票不同，债券相当于借钱给别人，别人每年是要付利息的，通常利率在5-7%之间，所以债券基金长期可以看成是一种近似于固定收益的理财。
比如投资债券之后，市场利率持续在下降，那么由于原先持有的债券当前票息比市场利率高，债券的价格就会上涨，从而使债券的真实利率达到市场平均水平。
因此期限越短，债券价格受到利率变动的影响很小
判断长期债券基金未来的收益是高还是低，主要在于预测市场利率走势，如果市场利率一直下降，债券基金的表现就会很给力


### 货币型基金 {#货币型基金}

-   货币基金主要投资的是银行协议存款、国债、央行票据这些安全性几乎100%的资产
-   目前余额宝的收益为1.3%


### 主动管理基金 {#主动管理基金}


## 基金赎回 {#基金赎回}


### 方式一：收益率达到30%赎回 {#方式一-收益率达到30-赎回}

这种方式周期会短一些，平均2-3年一次。


#### 和周期率有关系 {#和周期率有关系}

沪深300和中证500，一年波动率就会高一些，能接近30%。
优秀行业，像消费、医药等，波动率跟沪深300差不太多。
强周期性行业，像能源行业、证券行业等，那就更高了，波动率能到40%以上。


#### 为什么不15%赎回 {#为什么不15-赎回}

假设我们投资一个指数基金，是按照10%、15%的收益率就赎回，那赎回频率就会很高，很短时间就会遇到一次，同时交易费用也比较贵。


#### 来源 {#来源}

很多可转债，带有强制赎回条款。当可转债上涨并保持一定价格一段时间之后，就会触发强制赎回条款。
从历史的角度来看，大部分可转债，大概率会在2-3年时间内遇到强制赎回。


### 方式二：到指数高估赎回 {#方式二-到指数高估赎回}


#### 衡量估值的方法 {#衡量估值的方法}

[蛋卷](https://danjuanapp.com/djmodule/value-center?channel=1300100141)查看估值


## 经验 {#经验}


### 随着大盘上升, 股市成交量变多 牛市的成交量是熊市的十几倍.  不要在牛市买入股票 {#随着大盘上升-股市成交量变多-牛市的成交量是熊市的十几倍-dot-不要在牛市买入股票}


Enable katex by adding `katex = "true"` to the [front matter](https://gohugo.io/content-management/front-matter/)

```toml
+++
katex = "true"
+++
```

If you want to enable KaTeX or MathJax for all post, add `katex = ture` or `math = true` in `config.toml` in `[params]` section.

It's almost a dropin alternative to the mathjax solution,you should just choose one of them.

Inline math looks like this

```tex
This is text with inline math $\sum_{n=1}^{\infty} 2^{-n} = 1$
```

This is text with inline math $\sum_{n=1}^{\infty} 2^{-n} = 1$
and with math blocks:
