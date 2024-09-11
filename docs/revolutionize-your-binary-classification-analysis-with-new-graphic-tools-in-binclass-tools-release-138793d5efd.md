# 利用 binclass-tools 中的新图形工具革新你的二分类分析

> 原文：[https://towardsdatascience.com/revolutionize-your-binary-classification-analysis-with-new-graphic-tools-in-binclass-tools-release-138793d5efd?source=collection_archive---------10-----------------------#2023-03-15](https://towardsdatascience.com/revolutionize-your-binary-classification-analysis-with-new-graphic-tools-in-binclass-tools-release-138793d5efd?source=collection_archive---------10-----------------------#2023-03-15)

## 发现校准曲线、增益和提升图的强大功能，以及更多在 binclass-tools 最新版本中——这是解决二分类问题的终极解决方案！

[](https://lucazavarella.medium.com/?source=post_page-----138793d5efd--------------------------------)[![Luca Zavarella](../Images/6af72e50d79498ac378e2f1e469a0e65.png)](https://lucazavarella.medium.com/?source=post_page-----138793d5efd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----138793d5efd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----138793d5efd--------------------------------) [Luca Zavarella](https://lucazavarella.medium.com/?source=post_page-----138793d5efd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc00872c87df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frevolutionize-your-binary-classification-analysis-with-new-graphic-tools-in-binclass-tools-release-138793d5efd&user=Luca+Zavarella&userId=c00872c87df&source=post_page-c00872c87df----138793d5efd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----138793d5efd--------------------------------) · 9 分钟阅读 · 2023年3月15日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F138793d5efd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frevolutionize-your-binary-classification-analysis-with-new-graphic-tools-in-binclass-tools-release-138793d5efd&user=Luca+Zavarella&userId=c00872c87df&source=-----138793d5efd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F138793d5efd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frevolutionize-your-binary-classification-analysis-with-new-graphic-tools-in-binclass-tools-release-138793d5efd&source=-----138793d5efd---------------------bookmark_footer-----------)![](../Images/fcef35a830c2dff148c8f38d56a4927c.png)

(图片来源：Unsplash)

**binclass-tools** 包几天前达到了主要版本 1.*，并庆祝了从 PyPI 上获得的 **13K 次下载**！新版本包含了许多新功能，包括：

+   校准图

+   累积增益图

+   累积提升图

+   响应图

+   累积响应图

让我们更详细地了解一下这是什么。

# 校准图

二分类模型性能的评估可以通过利用校准图来辅助。这些图有助于评估预测概率与实际概率之间的对应程度。校准的基本前提是，如果一个模型对某个观察值预测的概率是0.8，那么实际上该观察值的正类概率也应该是0.8。
