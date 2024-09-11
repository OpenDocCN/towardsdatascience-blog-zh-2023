# 理解 ChatGPT 插件：好处、风险和未来发展

> 原文：[https://towardsdatascience.com/understanding-chatgpt-plugins-benefits-risks-and-future-developments-7a76f64e52ce?source=collection_archive---------2-----------------------#2023-06-02](https://towardsdatascience.com/understanding-chatgpt-plugins-benefits-risks-and-future-developments-7a76f64e52ce?source=collection_archive---------2-----------------------#2023-06-02)

## 期望改进，而非完美。

[](https://medium.com/@mary.newhauser?source=post_page-----7a76f64e52ce--------------------------------)[![Mary Newhauser](../Images/7f0d7287f7b735bb9391858f1fc641ee.png)](https://medium.com/@mary.newhauser?source=post_page-----7a76f64e52ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7a76f64e52ce--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7a76f64e52ce--------------------------------) [Mary Newhauser](https://medium.com/@mary.newhauser?source=post_page-----7a76f64e52ce--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b27bdb820b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-chatgpt-plugins-benefits-risks-and-future-developments-7a76f64e52ce&user=Mary+Newhauser&userId=6b27bdb820b9&source=post_page-6b27bdb820b9----7a76f64e52ce---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a76f64e52ce--------------------------------) ·13分钟阅读·2023年6月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7a76f64e52ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-chatgpt-plugins-benefits-risks-and-future-developments-7a76f64e52ce&user=Mary+Newhauser&userId=6b27bdb820b9&source=-----7a76f64e52ce---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7a76f64e52ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-chatgpt-plugins-benefits-risks-and-future-developments-7a76f64e52ce&source=-----7a76f64e52ce---------------------bookmark_footer-----------)![](../Images/49a635df55096dc29ed40e6c09b73c07.png)

作者提供的图片。

[*本文最初发表于 GPTech*](https://gptech.ghost.io/understanding-chatgpt-plugins/)*.*

当 ChatGPT 在 2022 年末首次发布时，它的能力既令人印象深刻又令人失望。它可以进行[说唱对决](https://www.reddit.com/r/ChatGPT/comments/122zfa6/rap_battling_chatgpt_is_my_new_favorite_sport/)和用 LaTeX 编写[differential equations](https://twitter.com/bentossell/status/1598269692082151424)，但对乌克兰战争一无所知，有时甚至无法完成[简单数学](https://twitter.com/petergyang/status/1607443647859154946?lang=en)计算。其[能力与局限性](https://medium.com/towards-data-science/gpt-4-vs-chatgpt-an-exploration-of-training-performance-capabilities-and-limitations-35c990c133c5)之间的明显对比，虽然令人困惑且有时显得神秘，却引发了一些有趣的 Twitter 讨论，但最终突显了模型对互联网连接的迫切需求。

![](../Images/c240970139db3bfcd7279820401eeac8.png)

作者提供的图片。

但插件不仅仅将 ChatGPT 连接到互联网，它们还可以将其连接到其他外部数据源，如内部数据库甚至你的电子邮件收件箱。实质上，插件扩展了 ChatGPT 的功能，提高了其响应的准确性，并允许更个性化的聊天体验。

本文涵盖了插件如何工作、促使其集成到 ChatGPT 中的必要性，以及它们对 ChatGPT 用户体验的变革性影响。我们将会...
