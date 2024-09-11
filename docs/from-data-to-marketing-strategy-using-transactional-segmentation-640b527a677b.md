# 从数据到营销策略：使用事务性细分

> 原文：[https://towardsdatascience.com/from-data-to-marketing-strategy-using-transactional-segmentation-640b527a677b?source=collection_archive---------13-----------------------#2023-01-05](https://towardsdatascience.com/from-data-to-marketing-strategy-using-transactional-segmentation-640b527a677b?source=collection_archive---------13-----------------------#2023-01-05)

## 如何有效利用细分实现业务增长

[](https://pranay-dave9.medium.com/?source=post_page-----640b527a677b--------------------------------)[![Pranay Dave](../Images/accecc418ea23d26862761bf470fcf04.png)](https://pranay-dave9.medium.com/?source=post_page-----640b527a677b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----640b527a677b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----640b527a677b--------------------------------) [Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----640b527a677b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F89d4bb7ead78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-to-marketing-strategy-using-transactional-segmentation-640b527a677b&user=Pranay+Dave&userId=89d4bb7ead78&source=post_page-89d4bb7ead78----640b527a677b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----640b527a677b--------------------------------) · 5分钟阅读 · 2023年1月5日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F640b527a677b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-to-marketing-strategy-using-transactional-segmentation-640b527a677b&user=Pranay+Dave&userId=89d4bb7ead78&source=-----640b527a677b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F640b527a677b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-to-marketing-strategy-using-transactional-segmentation-640b527a677b&source=-----640b527a677b---------------------bookmark_footer-----------)![](../Images/fbeefdb9b1737744fb5f895198d6374e.png)

使用 openai.com 创建 [https://labs.openai.com/s/SpAZlVi8fVRXueTBoKdi9p0w](https://labs.openai.com/s/SpAZlVi8fVRXueTBoKdi9p0w)

大型公司拥有数百万的客户。如果没有细分，很难制定营销策略或进行客户沟通，如电子邮件或新闻通讯。客户细分是企业优化营销和产品开发工作并更好服务客户的重要工具。

有多种方法可以对客户进行细分。在这篇博客中，我将重点讨论交易型客户细分，也称为RFM（recency, frequency, monetary）建模。

RFM 是

R = 最近一次购买 = 客户最后一次购买的时间

F = 频率 = 客户购买的频繁程度

M = 金额 = 购买的金额

使用交易细分来制定营销策略的过程如下所示。

![](../Images/84dcf51bac4a979ceaa42b89e3ea4f0d.png)

交易建模过程（作者提供的图片）

# 购买交易
