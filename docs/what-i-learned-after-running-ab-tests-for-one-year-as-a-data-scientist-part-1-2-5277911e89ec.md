# 作为数据科学家，经过一年 AB 测试后我学到的东西 — 第1/2部分

> 原文：[https://towardsdatascience.com/what-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-1-2-5277911e89ec?source=collection_archive---------9-----------------------#2023-01-03](https://towardsdatascience.com/what-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-1-2-5277911e89ec?source=collection_archive---------9-----------------------#2023-01-03)

## 按照这些简单步骤像数据科学家一样设置你的 A/B 测试

[](https://medium.com/@alex.vamvakaris.ds?source=post_page-----5277911e89ec--------------------------------)[![Alex Vamvakaris](../Images/bf8cf7c92a94d2b0d9242afcc03bf425.png)](https://medium.com/@alex.vamvakaris.ds?source=post_page-----5277911e89ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5277911e89ec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5277911e89ec--------------------------------) [Alex Vamvakaris](https://medium.com/@alex.vamvakaris.ds?source=post_page-----5277911e89ec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8072260dd591&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-1-2-5277911e89ec&user=Alex+Vamvakaris&userId=8072260dd591&source=post_page-8072260dd591----5277911e89ec---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5277911e89ec--------------------------------) ·13 min read·2023年1月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5277911e89ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-1-2-5277911e89ec&user=Alex+Vamvakaris&userId=8072260dd591&source=-----5277911e89ec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5277911e89ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-1-2-5277911e89ec&source=-----5277911e89ec---------------------bookmark_footer-----------)![](../Images/4c386c57ec8515e24726eff34021fe0a.png)

[Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄于 [Unsplash](https://unsplash.com/photos/tSGHEbYSUGw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

数据科学家工作的一大部分是测量差异。更具体地说，是比较产品或服务的不同版本，并确定哪个表现更好。这是通过**随机对照试验** **(RCT)** 或其更高级的商业名称**A/B测试**来完成的。

思路非常简单。我们想要改进一个特定的KPI，比如结账页面的转化率。然后我们假设某些变化可能对我们的KPI产生积极影响。例如，我们可能想测试将结账按钮的颜色从灰色改为绿色。然后，我们将随机分配结账页面的访客，一半看到当前版本（灰色按钮），另一半看到新版本（绿色按钮）。当前版本和新版本的术语分别为**控制**和**处理**。作为数据科学家，我们的工作是测量两个版本之间的转化率差异，并确定这是否具有统计显著性或仅仅是运气（随机变异）。

在这个系列中，我将带你全面了解A/B测试的整个过程，从设计实验到分析和展示结果。案例研究旨在模拟在现实环境中进行A/B测试时遇到的挑战，同时涵盖数据科学面试中常见的关键问题。指南将分为以下两部分：

## 第1部分

+   **思路：** 商业案例研究概述

+   **设计测试：** 选择目标人群和关注的KPI

+   **测试前分析：** 使用功效和模拟来估计样本量

## [第2部分：AB测试分析](/what-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-2-2-4b30118fec26)

+   **EDA：** 合理性检查，时间序列可视化

+   **分析实验结果：** P值、置信区间、自助法等。

# 1\. 思路

![](../Images/0404d12dfa4fe695274b9ce9ab0f45b3.png)

照片由 [Riccardo Annandale](https://unsplash.com/@pavement_special?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/7e2pe9wjL9M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

对于我们的案例研究，假设一个电商零售商在英国销售巴西咖啡豆。每周五，公司提供每消费50英镑减10英镑的优惠。一个产品经理一直在尝试提高促销效果，并通过研究提出了一个想法。如果我们将优惠的描述改为百分比折扣，那么每消费50英镑将获得20%的折扣。思路是20比10高，所以客户可能会认为同样的优惠具有更高的价值（尽管20%和10英镑折扣是相同的交易）。

这时我们的主角登场了。对这个想法非常兴奋的产品经理安排了一次与公司数据科学家的电话会议，并向他讲解了这个想法。我们的数据科学家对A/B测试并不陌生，因为他过去多次帮助业务利益相关者做出这样的决策。于是，我们的旅程开始了。

# 2\. 设计测试

![](../Images/7fd284eafa8167dfe57302988b8da822.png)

图片由 [Alex wong](https://unsplash.com/@killerfvith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/l5Tzv1alcps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

## 目标人群

在进一步进行之前，重要的是明确我们实验的目标人群。在我们的例子中，就是任何在星期五（即优惠活动上线时）访问网站的人。这是一个重要的区别。如果用户在星期一访问了网站，那么将他们纳入实验没有意义，因为他们不会受到优惠活动变化的影响。

按照这个逻辑，你可能还会问是否适合包括所有星期五的访问者。优惠可能并非在所有页面上都有展示，因此只应包括访问了有优惠展示的页面的用户。你说得对。对于我们的案例研究，我们假设优惠是在网站每个页面顶部的横幅（如下所示），以简化问题。

![](../Images/f7085c3301d58e5f56f3bd9a97720e1f.png)

为A/B测试设计两个版本 [图片来源：作者]

## 选择感兴趣的KPI

无论实验的想法是来自CEO还是初级分析师，明确KPIs的需求总是存在的。但是为什么会这样呢？我们为什么不能先进行实验，然后查看所有可能受到影响的指标？

除了影响效率之外，还有另一个理由需要在开始测试之前定义我们的KPI。为了估算（关键词）进行准确推断所需的样本量，我们还必须了解KPI的方差。一个波动性较大的指标，如平均支出，可能会偏斜，并具有更高的方差和异常值，因此需要比对称正态分布指标更高的样本量。

好的，让我们开始正式工作。对于我们的实验，我们关注两个KPI。两者都在**访客层面**，并且与我们期望的新版本的效果有合理的关联。更多的访客兑换优惠（即更高的ATPV），而不降低他们的整体支出（更高的ARPV或至少对ARPV没有负面影响）。

> **每访客平均收入 (ARPV)：** 总收入 / 总访客数
> 
> **每访客平均交易数 (ATPV)：** 总交易数 / 总访客数

## 时间窗口

你可能已经注意到，周五没有出现在任何一个指标的公式中。这是因为如果周五的访客支出减少会影响到其他日期的销售，从而导致每周或每月支出保持不变，我们并不关心在周五增加访客支出。我们真正关心的是在设定的时间窗口内逐步增加他们的整体交易量和支出。

由于大多数公司每月报告其关键KPI，我们也将以28天为窗口报告我们的两个KPI（其他公司可能按周或每两周运营，因此7天或14天的KPI可能更合适）：

+   每个访客在测试上线后第一次访问网站的周五进入实验。这将是他们的第1天。

+   接下来，我们收集接下来的27天的数据，从而获得每个访客的28天KPI。

+   因此，28天ARPV将是访客进入测试后的28天内收入总和，除以访客数量。

+   仅包括在测试中待满28天的访客（不是所有访客，因为我们希望给每个人相同的购买时间窗口）。

+   28天窗口还确保没有新奇效应（访客对变化最初感到兴奋，但这种兴奋在一两周后很快消退）。

![](../Images/ab1baf76bed7e9bcc14a8228106952d5.png)

KPI的28天窗口示例 [由作者提供的图片]

这些都是面试中需要覆盖的重要点。展示你对总体情况的理解，以及用户进入测试的漏斗的哪个部分（是所有页面还是特定页面）。最后，始终将其与业务需求联系起来。解释清晰地与利益相关者沟通定义KPI的必要性，以及你如何设计实验以使其成为资产！

# 3\. 预测试分析

![](../Images/25aebfeacb7ee9f6f27ecc0aefd3f11b.png)

图片来源于 [AbsolutVision](https://unsplash.com/es/@freegraphictoday?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/uCMKx2H1Y38?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这就是令人兴奋的部分开始的地方。我们在这一部分的主要目标是估计所需的样本量，以准确推断我们的两个KPI。这部分被称为功效分析。**功效分析** 允许我们在给定以下三个量的情况下估计所需的样本量：

1.  **效应大小**

1.  **显著性水平** = P（I型错误）= 发现实际上不存在的效应的概率

1.  **功效** = 1 — P（II型错误）= 发现实际上存在的效应的概率

我们将显著性水平设定为5%，功效设定为80%（我们将在[**第2部分**](/what-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-2-2-4b30118fec26)中详细讨论这些概念）。然后我们只需要计算效应量来获得所需的样本量。幸运的是，[Cohen 1988](https://www.amazon.com/Statistical-Power-Analysis-Behavioral-Sciences/dp/0805802835) 为我们提供了在比较两个均值时计算效应量的公式（我还添加了比例的公式，因为它是一个常选的KPI，例如付款率等）。

![](../Images/3f65accaebae6667cd7bd838fa456e3e.png)

[Cohen 1988](https://www.amazon.com/Statistical-Power-Analysis-Behavioral-Sciences/dp/0805802835) 提供的效应量公式 [作者提供的图片]

但我们仍未脱离困境。我们需要找出当前版本和新版本的均值及其标准差。由于我们不知道我们的KPI在未来会如何表现，我们可以查看历史数据以找到这些数字（作为我们对测试上线时期望结果的最佳估计）。因此，我们将回溯两个月，假设我们当时开始了实验。然后我们将计算28天ARPV和28天ATPV。这为20000名访问者提供了数据（这些用户至少在测试中待了28天）。

```py
##########################################
# Load Libraries
##########################################
library("ggplot2")
library("dplyr")
library("scales")
library("patchwork")

#############
# ARPV
#############
# Plot
revenue_plot <- 
  dataset %>%
  ggplot(aes(revenue)) +                          
  geom_histogram(fill ="turquoise3", colour = "white", binwidth = 2, boundary = 0) +
  scale_x_continuous(breaks = seq(0, max(dataset$revenue), 14) ) +
  ylab("No of Visitors") +
  xlab("28-day Revenue (each bar is £2)") +
  ggtitle("Histogram of 28-day Revenue") +
  theme_classic()

# Statistics
dataset %>% select(revenue) %>% summary()
dataset %>% summarise(sqr_rt = sd(revenue))

#############
# ATPV
#############
# Plot
transactions_plot <- 
  dataset %>%
  ggplot(aes(x = transactions)) +                          
  geom_bar(fill ="turquoise3", colour = "white") +
  scale_x_continuous(breaks = seq(0, max(dataset$transactions), 1) ) +
  ylab("No of Visitors") +
  xlab("28-day Transactions") +
  ggtitle("Histogram of 28-day Transactions") +
  theme_classic()

# Statistics
dataset %>% select(transactions) %>% summary()
dataset %>% summarise(sqr_rt = sd(transactions))

#################
# Output plot
#################
revenue_plot + transactions_plot
```

![](../Images/64efc4063ddb0504b9c299f4b76f83af.png)

历史收入和交易的直方图 [作者提供的图片]

从这些数据中，我们可以初步了解我们两个KPI的分布——从上述图表中可以看出，一半的用户（10000人）在28天窗口期内没有进行任何购买（没有收入和交易）。我们还可以计算当前版本的均值（ARPV和ATPV）以及效应量公式的标准差。然而，我们仍然缺少新版本的均值数据。由于我们没有关于新版本表现的数据，这一点尚不可知。不过，我们可以声明我们感兴趣的**最小可检测效应（MDE）**，在我们的情况下为5%的差异。然后我们可以计算新版本的均值为当前版本增加5%。

```py
##########################################
# Load Libraries
##########################################
library("pwr")

##########################################
# Cohen's power for ARPV
##########################################
std_arpv <- dataset %>% summarise(std = sd(revenue))
arpv_current_vers <- dataset %>% summarise(avg = mean(revenue))
arpv_new_vers <- arpv_current_vers * 1.05
effect_size_arpv <- as.numeric(abs(arpv_current_vers - arpv_new_vers)/std_arpv)

pwr_results_arpv <- pwr.t.test(
  d = effect_size_arpv, 
  sig.level = 0.05, 
  power = 0.8, 
  type = c("two.sample")
  )
plot(pwr_results_arpv)
pwr_results_arpv

##########################################
#Cohen's power for ATPV
##########################################
std_atpv <- dataset %>% summarise(std = sd(transactions))
atpv_current_vers <- dataset %>% summarise(avg = mean(transactions))
atpv_new_vers <- atpv_current_vers * 1.05
effect_size_atpv <- as.numeric(abs(atpv_current_vers - atpv_new_vers)/std_atpv)

pwr_results_atpv <- pwr.t.test(
  d = effect_size_atpv, 
  sig.level = 0.05, 
  power = 0.8, 
  type = c("two.sample")
  )
plot(pwr_results_atpv)
pwr_results_atpv
```

![](../Images/32072eecb2df983dc31ed57a4d03d046.png)

**ARPV**的功效图 [作者提供的图片]

![](../Images/a885030f7a314e4e34aa0093d4d30544.png)

**ATPV**的功效图 [作者提供的图片]

从上述内容来看，我们估算ARPV和ATPV的样本量分别为6464和7279。这些估算指的是控制组和处理组的样本量，因此总共我们需要大约14000名访问者的样本。

好奇的读者可能已经注意到，功效计算是基于我们将使用双样本t检验进行分析的假设，并向后推导，以给出对于给定效应大小、显著水平和功效所需要的样本量。但该技术有一些假设。特别是，它假设我们的数据大致遵循正态分布（即大致对称且围绕平均值中心）。通过观察直方图，可以清楚地看到营收和交易数据明显不同。这可能仍然可以，因为中心极限定理。我们将在[**第二部分**](/what-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-2-2-4b30118fec26)详细介绍CLT，但一般来说，具有足够样本量时，违背正态分布并不会导致问题（数据越倾斜和非对称，我们需要的样本量越大）。尽管如此，我还想估计无需假设即可估计所需样本量的方法，即使用非参数方法。

另一种方法是使用历史数据并生成给定大小的2000个样本。因此，我们可以从20000个观察值中模拟出100个大小的2000个随机样本。在这些样本中的每一个中，我们然后可以随机将一半用户分配到对照组，另一半用户分配到实验组，并计算指标之间的差异。最后，我们可以在ARPV和ATPV的差异的2000个95%区间（从第0.025到第0.975分位数）中可视化，并了解KPI的可变性。通过尝试不同的样本量，我们可以找到我们的MDE所需的样本量。因此，让我们创建我们将用于模拟样本的函数。

```py
##########################################
# Load Libraries
##########################################
library("caret")

##########################################
# Function for Simulation
##########################################
simulating_sample_size <-
  function(dataset, iterations, sample_sizes_vector, kpi) {

    n <- iterations
    output_df <- data.frame(NULL)

    for (j in sample_sizes_vector) {
      # create 2,000 samples for sample size j
      sampling_df <- data.frame(NULL)
      for (i in 1:n) {
        sampling_temporary <-
          data.frame(kpi = sample(dataset[[kpi]], j, replace = TRUE))
        trainIndex <-
          createDataPartition(
            sampling_temporary$kpi,
            p = 0.5,
            list = FALSE,
            times = 1
          )
        ## split into control and treatment
        sampling_df[i, 1] <- mean(sampling_temporary[trainIndex,])
        sampling_df[i, 2] <- mean(sampling_temporary[-trainIndex,])
        }

      # compute aggregates for sample size j
      # and union with old entries
      output_df <- output_df %>%
        union_all(
          .,
          sampling_df %>%
            mutate(diff = round((V2 - V1) / V1, 2)) %>%
            summarize(
              trim_0.05_diff = round(quantile(diff, c(0.025)),2),
              trim_0.95_diff = round(quantile(diff, c(0.975)),2),
              mean_0.05_abs = round(mean(V1),2),
              mean_0.95_abs = round(mean(V2),2)
            ) %>%
            mutate(iter = j)
        )
    }

    return(output_df)
}
```

我们现在可以使用我们的函数从我们的历史数据中生成给定大小的样本。我们将使用500、1000、2000、3000、4000、10000、15000和20000的样本量，并生成每个大小的2000个样本。

```py
##########################################
# ARPV simulation
##########################################
simulation_arpv <- 
  simulating_sample_size (
    dataset = dataset,
    iterations = 2000,
    sample_sizes_vector = c(500,1000,2000,3000,4000, 10000, 15000, 20000),
    kpi = "revenue"
)

##########################################
# ATPV simulation
##########################################
simulation_atpv <- 
  simulating_sample_size (
    dataset = dataset,
    iterations = 2000,
    sample_sizes_vector = c(500,1000,2000,3000,4000, 10000, 15000, 20000),
    kpi = "transactions"
)
```

```py
##########################################
# Plot ARPV from simulations
##########################################
arpv_sim_plot <- simulation_arpv %>%  
  ggplot(aes(x = as.factor(iter), y = trim_0.05_diff )) +                          
  geom_col(aes(y = trim_0.05_diff ), fill="turquoise3", alpha=0.9,width = 0.5) +
  geom_text(aes(label = scales::percent(trim_0.05_diff)), vjust = -0.5, size = 5) +
  geom_col(aes(y = trim_0.95_diff ), fill="turquoise3", alpha=0.9,width = 0.5) +
  geom_text(
    aes(x = as.factor(iter), y = trim_0.95_diff,  label = scales::percent(trim_0.95_diff)),
    vjust = -0.5, size = 5
  ) +
  scale_y_continuous(labels = scales::percent) +
  ylab("% Difference in ARPV") +
  xlab("Sample size") +
  theme_classic() +
  geom_hline(yintercept= 0, linetype="dashed", color = "red")

##########################################
# Plot ATPV from simulations
##########################################
atpv_sim_plot <- simulation_atpv %>%  
  ggplot(aes(x = as.factor(iter), y = trim_0.05_diff )) +                          
  geom_col(aes(y = trim_0.05_diff ), fill="turquoise3", alpha=0.9,width = 0.5) +
  geom_text(aes(label = scales::percent(trim_0.05_diff)), vjust = -0.5, size = 5) +
  geom_col(aes(y = trim_0.95_diff ), fill="turquoise3", alpha=0.9,width = 0.5) +
  geom_text(
    aes(x = as.factor(iter), y = trim_0.95_diff,  label = scales::percent(trim_0.95_diff)),
    vjust = -0.5, size = 5
  ) +
  scale_y_continuous(labels = scales::percent) +
  ylab("% Difference in ATPV") +
  xlab("Sample size") +
  theme_classic() +
  geom_hline(yintercept= 0, linetype="dashed", color = "red")

#################
# Output plot
#################
 (arpv_sim_plot + coord_flip()) + (atpv_sim_plot + coord_flip())
```

![](../Images/07102030d75ce60b5c7b98e50ee09627.png)

模拟不同大小的2000个样本中的指标变化[作者提供的图片]

上述结果与我们的功效分析的结果相吻合。看一个样本量为15000，两个KPI的95%区间在-2%和2%变化之间。所以如果我们取一个15000的样本，并将7500分配给对照组和实验组，我们期望在没有差异的情况下，具有四个百分点的变异性（从-2%到2%）。因此，如果新版本的提升为5%，我们预计15000个样本会返回3%至7%的结果（5%-2% = 3%，5% + 2% = 7%）。

# **摘要**

通过最后一步，我们已经准备好启动我们的**RCT实验**了！让我们总结一下我们的现状：

✅ 我们定义了测试的两个版本

✅ 我们将人口定义为星期五的所有访问者

✅ 一旦访问者进入测试，他们将被随机分成对照组和实验组（50% — 50%）

✅ 我们定义了我们感兴趣的两个KPI：28天ARPV和28天ATPV

✅ 我们使用功效分析和模拟估算了实验的样本量为15000（对照组和处理组各7500）。

在本系列的下一篇文章中，我们将深入探讨如何分析A/B测试的结果。如果你想自己动手操作数据，可以查看下面我用来生成20000个观察值的数据集的代码。

```py
set.seed(15)

##########################################
# Create normal skewed ARPV attribute
##########################################
sigma = 0.6
mu = 2
delta = 1
samples = 10000
revenue <- rnorm(samples, rlnorm(samples, mu, sigma) , delta)
revenue <- revenue + 40

##########################################
# Create normal symmetric ATPV attribute
##########################################
transactions <- round(rnorm(10000, 2, 0.5),0)

##########################################
# Create data set with both attributes
##########################################
dataset <- 
  data.frame(revenue, transactions) %>%
  # fixing records with 0 or negative atpv but positive arpv
  mutate(transactions = case_when(transactions <= 0 & revenue > 0 ~ 1 , TRUE ~ transactions)) %>%
  # adding non purchasing visitors
  union_all(., data.frame(revenue = rep(0,10000), transactions = rep(0,10000)))

summary(dataset)
```

# **保持联系**

如果你喜欢阅读这篇文章并想了解更多，不要忘记[订阅](https://medium.com/@alex.vamvakaris.ds/subscribe)以便直接将我的故事发送到你的收件箱。

在下面的链接中，你还可以找到一个免费的PDF指南，讲解如何在实际业务场景中使用数据科学技术和最佳实践在R中完成客户集群分析。

[## 数据科学项目清单 - 渴望成为数据科学家](https://www.aspiringdatascientist.net/community?source=post_page-----5277911e89ec--------------------------------)

### 我是一名拥有7年以上分析经验的数据科学家，目前在英国伦敦的一家游戏公司工作。我的……

[www.aspiringdatascientist.net](https://www.aspiringdatascientist.net/community?source=post_page-----5277911e89ec--------------------------------)
