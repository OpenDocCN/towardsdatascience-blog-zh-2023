# 通过蒸馏身份偏好优化（IPO）微调更好的聊天模型

> 原文：[https://towardsdatascience.com/fine-tune-better-chat-models-with-distilled-identity-preference-optimization-ipo-99cddc819a48?source=collection_archive---------7-----------------------#2023-12-13](https://towardsdatascience.com/fine-tune-better-chat-models-with-distilled-identity-preference-optimization-ipo-99cddc819a48?source=collection_archive---------7-----------------------#2023-12-13)

## Mistral 7B 与 IPO 对齐

[](https://medium.com/@bnjmn_marie?source=post_page-----99cddc819a48--------------------------------)[![Benjamin Marie](../Images/3ea1ad230cb1e67610418a8e36a5e5dd.png)](https://medium.com/@bnjmn_marie?source=post_page-----99cddc819a48--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99cddc819a48--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----99cddc819a48--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----99cddc819a48--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-better-chat-models-with-distilled-identity-preference-optimization-ipo-99cddc819a48&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----99cddc819a48---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99cddc819a48--------------------------------) ·6分钟阅读·2023年12月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F99cddc819a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-better-chat-models-with-distilled-identity-preference-optimization-ipo-99cddc819a48&user=Benjamin+Marie&userId=ad2a414578b3&source=-----99cddc819a48---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F99cddc819a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-better-chat-models-with-distilled-identity-preference-optimization-ipo-99cddc819a48&source=-----99cddc819a48---------------------bookmark_footer-----------)![](../Images/e762c4828a29a687b5f127b736a29db8.png)

照片由 [Rishabh Dharmani](https://unsplash.com/@rishabhdharmani?utm_source=medium&utm_medium=referral) 提供，拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

要成为聊天模型，预训练的大型语言模型（LLMs）需要在大量的指令/问题及预期答案的数据集上进行微调。虽然这种简单的微调可以产生令人信服的聊天模型，但从人类的角度来看，它们的回答仍可能不连贯、偏颇、不道德且不安全。这就是为什么我们通常会进行额外的训练步骤，以更好地使LLM与人类对齐。

这种对齐可以通过使用带有人类反馈的强化学习（RLHF）来完成。正如OpenAI和ChatGPT的成功所示，RLHF可以产生最先进的聊天模型。然而，RLHF的运行成本很高。它需要由人类注释的大型数据集以及多个辅助模型（参考模型和奖励模型）的训练。

作为RLHF的一个更简单且便宜的替代方案，[直接偏好优化（DPO）](https://kaitchup.substack.com/p/fine-tune-your-own-instruct-version)最近已成功应用于对齐LLMs，例如Hugging Face的[Zephyr](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)和Intel的[Neural Chat](https://huggingface.co/Intel/neural-chat-7b-v3)。

在本文中，基于Google DeepMind的工作，我们将看到，尽管RLHF和DPO在对齐LLMs方面表现良好，但考虑到用于训练的数据集，它们仍远未达到最佳水平。DeepMind还展示了为什么DPO容易出现过拟合现象。我将在……中解释。
