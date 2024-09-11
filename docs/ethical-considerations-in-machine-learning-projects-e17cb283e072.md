# 机器学习项目中的**伦理考量**

> 原文：[https://towardsdatascience.com/ethical-considerations-in-machine-learning-projects-e17cb283e072?source=collection_archive---------0-----------------------#2023-02-11](https://towardsdatascience.com/ethical-considerations-in-machine-learning-projects-e17cb283e072?source=collection_archive---------0-----------------------#2023-02-11)

![](../Images/22f131eeee213e6dddb5a6519e111693.png)

图片由 Dall-E 2 生成。

## 在构建 AI 系统时，不要忘记这些主题

[](https://hennie-de-harder.medium.com/?source=post_page-----e17cb283e072--------------------------------)[![Hennie de Harder](../Images/3e4f2cccd6cb976ca3f8bf15597daea8.png)](https://hennie-de-harder.medium.com/?source=post_page-----e17cb283e072--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e17cb283e072--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e17cb283e072--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----e17cb283e072--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fethical-considerations-in-machine-learning-projects-e17cb283e072&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----e17cb283e072---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e17cb283e072--------------------------------) ·7 min read·2023年2月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe17cb283e072&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fethical-considerations-in-machine-learning-projects-e17cb283e072&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----e17cb283e072---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe17cb283e072&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fethical-considerations-in-machine-learning-projects-e17cb283e072&source=-----e17cb283e072---------------------bookmark_footer-----------)

**使用敏感数据、基于个人信息的歧视、影响人们生活的模型输出…… 数据产品或机器学习模型可以以多种方式造成伤害。不幸的是，存在** [**许多例子**](https://www.amazon.com/Weapons-Math-Destruction-Increases-Inequality/dp/0553418815) **其中确实出现了问题。另一方面，许多项目是无害的：不使用敏感数据，这些项目仅仅是改善流程或自动化无聊的任务。当有可能造成伤害时，你应该花时间深入探讨这篇文章的主题。**

是的，数据伦理可能不是你能想到的最激动人心的话题。但如果你希望以负责任的方式使用数据，你可能会对确保这一点的不同方法感兴趣。这篇文章包含了六个重要的伦理话题以及调查你模型表现的方式。还有实用工具可以帮助你做到这一点。除了对个人可能造成的伤害之外，另一个重要的责任理由是伦理考虑可以影响技术的信任度和可信度。如果技术被认为不值得信赖和透明，它可能会妨碍其采纳并影响其有效性。人们会失去信任，不再愿意使用AI产品。

# 治理
