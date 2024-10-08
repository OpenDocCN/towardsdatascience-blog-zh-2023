# 从数据到营销策略，通过事务性细分

> 原文：[`towardsdatascience.com/from-data-to-marketing-strategy-using-transactional-segmentation-640b527a677b`](https://towardsdatascience.com/from-data-to-marketing-strategy-using-transactional-segmentation-640b527a677b)

## 如何有效利用细分实现业务增长

[](https://pranay-dave9.medium.com/?source=post_page-----640b527a677b--------------------------------)![Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----640b527a677b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----640b527a677b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----640b527a677b--------------------------------) [Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----640b527a677b--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----640b527a677b--------------------------------) ·阅读时间 5 分钟·2023 年 1 月 5 日

--

![](img/fbeefdb9b1737744fb5f895198d6374e.png)

创建于 openai.com [`labs.openai.com/s/SpAZlVi8fVRXueTBoKdi9p0w`](https://labs.openai.com/s/SpAZlVi8fVRXueTBoKdi9p0w)

大公司拥有数百万客户。如果没有细分，很难制定营销策略或进行客户沟通，如电子邮件或新闻通讯。客户细分是企业优化营销和产品开发工作、以及更好服务客户的重要工具。

有多种方式来细分客户。在本博客中，我将重点介绍事务性客户细分，也称为 RFM（Recency, Frequency, Monetary）建模。

RFM 是

R = 最近一次购买 = 客户上次购买的时间

F = 频率 = 客户购买的频率

M = 金额 = 购买的金额

这里展示了使用事务性细分创建营销策略的过程。

![](img/84dcf51bac4a979ceaa42b89e3ea4f0d.png)

事务性建模过程（图像由作者提供）

# 购买交易

为了演示事务性细分，我将以一个在线零售商为例。这里展示了零售交易的样本数据。

![](img/d053a5220909eb8525ae094d1bba6d92.png)

零售交易样本数据（图像由作者提供）

数据包含信息，如发票号码、购买的产品、购买的数量以及价格。

# RFM 模型

RFM 计算是在客户层面进行的。发票日期可以用来计算最近一次购买，发票号码可以用来计算购买频率，总价可以用来计算金额值。

在 RFM 计算之后，数据将包含自上次购买以来的天数、按月计算的发票频率以及作为一个月所有购买总额的货币价值。

这里展示了一个 RFM 计算的示例数据。

![](img/7f6857d288585270976ff34690d89d03.png)

针对每个客户的 RFM 计算（图片由作者提供）

# 聚类

RFM 计算针对每个客户进行，时间范围可以是每月、每年或业务所需的其他时间范围。使用 RFM 值，可以通过聚类算法对客户进行细分。

这里展示的是聚类算法的结果。每个点对应一个客户。点的颜色对应于细分市场。总共有 5 个细分市场，每个客户都被分配到一个细分市场中。

![](img/70ba7a3403610cbe2ea3004212f9da00.png)

基于 RFM 数据的聚类（图片由作者提供）

# 分段解释

拥有一个美观的聚类可视化是很好的。然而，解释这些集群的含义同样重要。

可以帮助进行分段解释的可视化方法是平行坐标，如下所示。可以看到关于最近购买、购买频率和货币价值的垂直线。所有值都已标准化为 0 到 100 之间。最右侧的垂直线用于表示各个细分市场或集群。

![](img/f2d40990983e168e2764e365169adb8f.png)

使用平行坐标可视化进行解释（图片由作者提供）

# 营销策略

使用平行坐标可视化，我们可以解释每个集群并制定营销策略。这里展示的是每个集群或细分市场的可视化。

![](img/0820eece60e9b189d18c5889c541a166.png)

解释每个集群（图片由作者提供）

我们现在可以解释每个集群并制定策略。

**集群 0**

+   *解释* — 最近未进行购买的客户

+   *策略* — 提供优惠以促使他们回归购买

**集群 1**

+   *解释* — 具有高货币价值的客户

+   *策略* — 创建忠诚度计划，以便他们能够继续增加消费

**集群 2**

+   *解释* — 最近未进行购买的客户

+   *策略* — 提供优惠以促使他们回归购买

**集群 3**

+   *解释* — 可能会流失的客户

+   *策略* — 通过激动人心的优惠来留住他们

**集群 4**

+   *解释* — 常客

+   *策略* — 创建忠诚度计划，以保持他们定期购买

# **结论**

在这篇博客中，你看到了如何从数据到创建营销策略以促进业务增长。使用 RFM 进行客户细分是一种非常有效的策略，可以通过聚类和解释可视化来实施。

# 数据源引用

博客中使用的数据源可以在这里找到

[`archive.ics.uci.edu/ml/datasets/online+retail`](https://archive.ics.uci.edu/ml/datasets/online+retail)

可以用于商业或非商业目的，引用格式如下

Daqing Chen, Sai Liang Sain 和 Kun Guo，在线零售行业的数据挖掘：基于 RFM 模型的数据挖掘客户细分案例研究，《数据库营销与客户战略管理杂志》，第 19 卷，第 3 期，第 197–208 页，2012（在线出版时间：2012 年 8 月 27 日。doi: 10.1057/dbm.2012.17）。

# **演示**

您还可以在我的 YouTube 频道观看 RFM 建模的演示

请通过我的推荐链接**加入 Medium**。

[](https://pranay-dave9.medium.com/membership?source=post_page-----640b527a677b--------------------------------) [## 通过我的推荐链接加入 Medium - Pranay Dave

### 阅读 Pranay Dave 的每个故事（以及 Medium 上其他成千上万的作者的故事）。您的会员费直接支持…

pranay-dave9.medium.com](https://pranay-dave9.medium.com/membership?source=post_page-----640b527a677b--------------------------------)

请**订阅**以便在我发布新故事时保持信息更新

[](https://pranay-dave9.medium.com/subscribe?source=post_page-----640b527a677b--------------------------------) [## 每当 Pranay Dave 发布新内容时，您将收到一封电子邮件。

### 每当 Pranay Dave 发布新内容时，您将收到一封电子邮件。通过注册，如果您还没有 Medium 账户，您将创建一个…

pranay-dave9.medium.com](https://pranay-dave9.medium.com/subscribe?source=post_page-----640b527a677b--------------------------------)

# 额外资源

# 网站

您可以访问我的网站，这是一个无代码平台，用于学习数据科学。[**https://experiencedatascience.com**](https://experiencedatascience.com/)

# YouTube 频道

这是我的 YouTube 频道的链接

[`www.youtube.com/c/DataScienceDemonstrated`](https://www.youtube.com/c/DataScienceDemonstrated)
