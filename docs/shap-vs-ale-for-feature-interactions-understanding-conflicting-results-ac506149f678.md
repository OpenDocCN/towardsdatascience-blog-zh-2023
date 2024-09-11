# SHAP 与 ALE 在特征交互中的应用：理解相互矛盾的结果

> 原文：[`towardsdatascience.com/shap-vs-ale-for-feature-interactions-understanding-conflicting-results-ac506149f678?source=collection_archive---------2-----------------------#2023-10-02`](https://towardsdatascience.com/shap-vs-ale-for-feature-interactions-understanding-conflicting-results-ac506149f678?source=collection_archive---------2-----------------------#2023-10-02)

## 模型解释者需要深思熟虑的解读

[](https://medium.com/@vla6?source=post_page-----ac506149f678--------------------------------)![Valerie Carey](https://medium.com/@vla6?source=post_page-----ac506149f678--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ac506149f678--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ac506149f678--------------------------------) [Valerie Carey](https://medium.com/@vla6?source=post_page-----ac506149f678--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1a7c9171898f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-vs-ale-for-feature-interactions-understanding-conflicting-results-ac506149f678&user=Valerie+Carey&userId=1a7c9171898f&source=post_page-1a7c9171898f----ac506149f678---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ac506149f678--------------------------------) ·10 分钟阅读·2023 年 10 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fac506149f678&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-vs-ale-for-feature-interactions-understanding-conflicting-results-ac506149f678&user=Valerie+Carey&userId=1a7c9171898f&source=-----ac506149f678---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fac506149f678&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-vs-ale-for-feature-interactions-understanding-conflicting-results-ac506149f678&source=-----ac506149f678---------------------bookmark_footer-----------)![](img/a6c0057846dba5111997ce4bc1191bb4.png)

照片由[Diogo Nunes](https://unsplash.com/@dialex?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我对比了特征交互的模型解释技术。出乎意料的是，两种常用工具 SHAP 和 ALE 却产生了相反的结果。

也许，我不应该感到惊讶。毕竟，可解释性工具以不同的方式衡量特定的响应。解释需要理解测试方法、数据特征和问题背景。**仅仅因为某物被称为*解释器*，并不意味着它会生成一个*解释*，如果你将解释定义为人类理解模型如何工作的话。**

本文重点介绍特征交互的可解释性技术。我使用了一个源自真实贷款的常见项目数据集[1]，以及一种典型的模型类型（增强树模型）。即使在这种日常情况下，解释也需要深思熟虑的解读。

> 如果忽视方法细节，可解释性工具可能会阻碍理解，甚至破坏确保模型公平性的努力。

以下，我展示了不同的 SHAP 和 ALE 曲线，并演示了这些技术之间的分歧来源于测量的响应和特征扰动的差异…
