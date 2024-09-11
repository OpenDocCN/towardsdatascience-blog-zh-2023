# 使用GPT构建物理信息神经网络专家

> 原文：[https://towardsdatascience.com/building-an-expert-gpt-in-physics-informed-neural-networks-with-gpts-75ebf6925966?source=collection_archive---------5-----------------------#2023-11-18](https://towardsdatascience.com/building-an-expert-gpt-in-physics-informed-neural-networks-with-gpts-75ebf6925966?source=collection_archive---------5-----------------------#2023-11-18)

## 一个定制化的副驾驶，用于简化PINN研究和开发

[](https://shuaiguo.medium.com/?source=post_page-----75ebf6925966--------------------------------)[![Shuai Guo](../Images/d673c066f8006079be5bf92757e73a59.png)](https://shuaiguo.medium.com/?source=post_page-----75ebf6925966--------------------------------)[](https://towardsdatascience.com/?source=post_page-----75ebf6925966--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----75ebf6925966--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----75ebf6925966--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-an-expert-gpt-in-physics-informed-neural-networks-with-gpts-75ebf6925966&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----75ebf6925966---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----75ebf6925966--------------------------------) ·14 min read·2023年11月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F75ebf6925966&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-an-expert-gpt-in-physics-informed-neural-networks-with-gpts-75ebf6925966&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----75ebf6925966---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F75ebf6925966&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-an-expert-gpt-in-physics-informed-neural-networks-with-gpts-75ebf6925966&source=-----75ebf6925966---------------------bookmark_footer-----------)![](../Images/7a99281c603b8d52064dd2e1f7fa5b4b.png)

定制化GPT的标志。（由DALL·E 3生成）

最近OpenAI的DevDay上最有趣的发布之一是GPTs。本质上，GPTs是ChatGPT的定制版本，任何人都可以为特定目的创建。配置一个可用的GPT的过程不需要编码，仅需通过聊天完成。因此，自发布以来，社区创建了各种GPT，帮助用户提高生产力，为生活增添更多乐趣。

作为物理信息神经网络（PINN）领域的实践者，我经常使用 ChatGPT（GPT-4）来帮助我理解复杂的技术概念，调试在实现模型时遇到的问题，并提出新颖的研究思路或工程解决方案。尽管它非常有用，我经常发现 ChatGPT 在超出其对 PINN 的一般知识之外，给我提供定制化的答案时显得有些吃力。虽然我可以调整我的提示以融入更多的上下文信息，但这是一种耗时的做法，有时会迅速耗尽我的耐心。

现在，随着轻松定制 ChatGPT 的可能性，我想到了一个想法：为什么不开发一个定制的 GPT，作为一个物理信息神经网络专家 🦸‍♀️，从我的精选来源中获取知识，并努力回答我的...
