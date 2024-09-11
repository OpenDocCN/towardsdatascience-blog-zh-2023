# 说一次！重复词汇并没有帮助人工智能

> 原文：[https://towardsdatascience.com/say-once-repeating-words-is-not-helping-ai-58f38035f66e?source=collection_archive---------9-----------------------#2023-06-20](https://towardsdatascience.com/say-once-repeating-words-is-not-helping-ai-58f38035f66e?source=collection_archive---------9-----------------------#2023-06-20)

## | 人工智能 | 自然语言处理 | 大型语言模型

## 为什么重复令牌对LLMs（大型语言模型）有害？这是什么问题？

[](https://salvatore-raieli.medium.com/?source=post_page-----58f38035f66e--------------------------------)[![Salvatore Raieli](../Images/6bb4520e2df40d20283e7283141b5e06.png)](https://salvatore-raieli.medium.com/?source=post_page-----58f38035f66e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----58f38035f66e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----58f38035f66e--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----58f38035f66e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsay-once-repeating-words-is-not-helping-ai-58f38035f66e&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----58f38035f66e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----58f38035f66e--------------------------------) ·14分钟阅读·2023年6月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F58f38035f66e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsay-once-repeating-words-is-not-helping-ai-58f38035f66e&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----58f38035f66e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F58f38035f66e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsay-once-repeating-words-is-not-helping-ai-58f38035f66e&source=-----58f38035f66e---------------------bookmark_footer-----------)![](../Images/40a23dee61acd2fe912d9497b6a07d5f.png)

图片来源：[Kristina Flour](https://unsplash.com/it/@tinaflour) 在 Unsplash

[大型语言模型（LLMs）](https://en.wikipedia.org/wiki/Large_language_model)已经展示了它们的能力，并在全球范围内引起了轰动。现在每个大公司都有一个花哨的模型名字。但在幕后，它们都是[变压器模型](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))。**每个人都梦想拥有万亿参数，但难道没有限制吗？**

在这篇文章中，我们讨论了：

+   更大的模型是否保证性能优于小模型？

+   我们是否拥有巨型模型的数据？

+   如果你不收集新的数据，而是重复使用已有的数据，会发生什么？

# 在天空中的扩展：是什么导致了机翼的损伤？

![](../Images/4c30898f3c8683fc888bf2424f1a20f8.png)

图片由 [Sean Pollock](https://unsplash.com/it/@seanpollock) 提供，来自 Unsplash

[OpenAI 已经定义了扩展法则](https://arxiv.org/abs/2001.08361)，该法则指出，模型性能遵循一个幂律，具体取决于使用的参数数量和数据点的数量。这与对新兴属性的探索一起，形成了参数竞赛：**模型越大，效果越好。**
