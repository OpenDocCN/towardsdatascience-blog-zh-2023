# 使用多任务和集成学习来预测阿尔茨海默病的认知功能

> 原文：[https://towardsdatascience.com/using-multi-task-and-ensemble-learning-to-predict-alzheimers-cognitive-functioning-7b46fe09f9ff?source=collection_archive---------8-----------------------#2023-06-09](https://towardsdatascience.com/using-multi-task-and-ensemble-learning-to-predict-alzheimers-cognitive-functioning-7b46fe09f9ff?source=collection_archive---------8-----------------------#2023-06-09)

## 个人在数据科学中的故事

## 实现我在机器学习领域的影响力，源自于认知科学及我首篇科学论文的发表

[](https://medium.com/@christabellecp?source=post_page-----7b46fe09f9ff--------------------------------)[![Christabelle Pabalan](../Images/24187865b6e9d03ae1aabf873ce1e67c.png)](https://medium.com/@christabellecp?source=post_page-----7b46fe09f9ff--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7b46fe09f9ff--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7b46fe09f9ff--------------------------------) [Christabelle Pabalan](https://medium.com/@christabellecp?source=post_page-----7b46fe09f9ff--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4200eb8e8b26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-multi-task-and-ensemble-learning-to-predict-alzheimers-cognitive-functioning-7b46fe09f9ff&user=Christabelle+Pabalan&userId=4200eb8e8b26&source=post_page-4200eb8e8b26----7b46fe09f9ff---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7b46fe09f9ff--------------------------------) · 11分钟阅读 · 2023年6月9日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7b46fe09f9ff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-multi-task-and-ensemble-learning-to-predict-alzheimers-cognitive-functioning-7b46fe09f9ff&user=Christabelle+Pabalan&userId=4200eb8e8b26&source=-----7b46fe09f9ff---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7b46fe09f9ff&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-multi-task-and-ensemble-learning-to-predict-alzheimers-cognitive-functioning-7b46fe09f9ff&source=-----7b46fe09f9ff---------------------bookmark_footer-----------)![](../Images/ac01b69c80525d944023f859022ded19.png)

图片来源：[Alina Grubnyak](https://unsplash.com/it/@alinnnaaaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/neuroscience?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在我之前的一篇[文章](https://medium.com/towards-data-science/why-my-cognitive-science-degree-was-a-great-foundation-for-data-science-and-machine-learning-f5838b527d40)中，我详细描述了我从认知科学过渡到机器学习的经历以及困扰我的冒名顶替综合症。在那篇文章中，我提到：

> “一个想法开始慢慢展开——也许，我的背景[在认知科学]提供了比我最初预期的更为扎实的基础。”

在这篇文章中，我将分享一个具体的例子，说明我的认知科学背景如何让我1) 在神经科学领域为一个对我个人具有重要意义的疾病开发创新的建模方法，以及2) 建立那些在传统讨论中常被忽视的独特联系。

通过这一经历，我清楚地意识到，深度学习领域虽然潜力巨大，但仍处于初期阶段，这提醒我们它所提供的包容性机会…
