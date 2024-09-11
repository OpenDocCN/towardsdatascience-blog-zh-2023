# Matplotlib技巧以即时提升你的数据可视化——根据《数据故事讲述》

> 原文：[https://towardsdatascience.com/matplotlib-tips-to-instantly-improve-your-data-visualizations-according-to-storytelling-with-8e75601b38cb?source=collection_archive---------3-----------------------#2023-06-19](https://towardsdatascience.com/matplotlib-tips-to-instantly-improve-your-data-visualizations-according-to-storytelling-with-8e75601b38cb?source=collection_archive---------3-----------------------#2023-06-19)

## 使用Matplotlib在Python中重新创建Cole Nussbaumer Knaflic的书中的经验教训

[](https://medium.com/@iamleonie?source=post_page-----8e75601b38cb--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----8e75601b38cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e75601b38cb--------------------------------)[![数据科学进展](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8e75601b38cb--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----8e75601b38cb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmatplotlib-tips-to-instantly-improve-your-data-visualizations-according-to-storytelling-with-8e75601b38cb&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----8e75601b38cb---------------------post_header-----------) 发表在 [数据科学进展](https://towardsdatascience.com/?source=post_page-----8e75601b38cb--------------------------------) · 9分钟阅读· 2023年6月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8e75601b38cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmatplotlib-tips-to-instantly-improve-your-data-visualizations-according-to-storytelling-with-8e75601b38cb&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----8e75601b38cb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8e75601b38cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmatplotlib-tips-to-instantly-improve-your-data-visualizations-according-to-storytelling-with-8e75601b38cb&source=-----8e75601b38cb---------------------bookmark_footer-----------)![](../Images/6c16450db41f47d2c5e2d33889080a98.png)

能够有效地使用数据进行沟通是一项适用于任何从事数据工作者的技能——不仅仅是数据科学家和数据分析师。

我最喜欢的关于这一主题的书之一是Cole Nussbaumer Knaflic的《数据故事讲述》。这本书充满了如何改进数据可视化的实用示例。

[## 讲故事与数据：商业专业人士的数据可视化指南](https://www.amazon.com/Cole-Nussbaumer-Knaflic/dp/1119002257?source=post_page-----8e75601b38cb--------------------------------)

### 讲故事与数据：商业专业人士的数据可视化指南 [努斯鲍默·克纳夫利克，科尔] 在 Amazon.com…

[www.amazon.com](https://www.amazon.com/Cole-Nussbaumer-Knaflic/dp/1119002257?source=post_page-----8e75601b38cb--------------------------------)

在我看来，这本书唯一不幸的方面是它的示例是使用 Microsoft Excel 创建的。

如果你知道一个喜欢在 Excel 中创建数据可视化的工程师，请举手——是的，我也没有。

> “你可能是工程师，但理解你的图表不应该需要一个工程学位。”——科尔·努斯鲍默·克纳夫利克…
