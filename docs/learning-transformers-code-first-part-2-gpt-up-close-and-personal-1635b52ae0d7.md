# 学习变压器代码第一部分 2 — GPT 亲密接触

> 原文：[`towardsdatascience.com/learning-transformers-code-first-part-2-gpt-up-close-and-personal-1635b52ae0d7?source=collection_archive---------11-----------------------#2023-07-13`](https://towardsdatascience.com/learning-transformers-code-first-part-2-gpt-up-close-and-personal-1635b52ae0d7?source=collection_archive---------11-----------------------#2023-07-13)

## 深入探讨通过 nanoGPT 生成预训练变压器

[](https://medium.com/@oaguy1?source=post_page-----1635b52ae0d7--------------------------------)![莉莉·休斯-罗宾逊](https://medium.com/@oaguy1?source=post_page-----1635b52ae0d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1635b52ae0d7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1635b52ae0d7--------------------------------) [莉莉·休斯-罗宾逊](https://medium.com/@oaguy1?source=post_page-----1635b52ae0d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5389e25ca1bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-transformers-code-first-part-2-gpt-up-close-and-personal-1635b52ae0d7&user=Lily+Hughes-Robinson&userId=5389e25ca1bb&source=post_page-5389e25ca1bb----1635b52ae0d7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1635b52ae0d7--------------------------------) ·13 分钟阅读·2023 年 7 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1635b52ae0d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-transformers-code-first-part-2-gpt-up-close-and-personal-1635b52ae0d7&user=Lily+Hughes-Robinson&userId=5389e25ca1bb&source=-----1635b52ae0d7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1635b52ae0d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-transformers-code-first-part-2-gpt-up-close-and-personal-1635b52ae0d7&source=-----1635b52ae0d7---------------------bookmark_footer-----------)![](img/f90478c1b96dad6cd2667bfcf0da1f03.png)

图片由 [卢卡·昂尼博尼](https://unsplash.com/it/@lucaonniboni?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎来到我的项目的第二部分，在这一部分中，我将深入探讨基于变压器和 GPT 模型的复杂性，使用 [TinyStories 数据集](https://huggingface.co/datasets/roneneldan/TinyStories) 和 [nanoGPT](https://github.com/karpathy/nanoGPT/tree/master)，所有模型都在一台老旧的游戏笔记本电脑上进行训练。在第一部分中，我准备了数据集，以便输入到一个字符级的生成模型中。你可以在下面找到第一部分的链接。

[](/nanogpt-learning-transformers-code-first-part-1-f2044cf5bca0?source=post_page-----1635b52ae0d7--------------------------------) ## 学习变压器代码第一部分

### 这是一个新系列的第一部分，我致力于使用 nanoGPT 首先学习变压器代码。

[towardsdatascience.com

在这篇文章中，我的目标是剖析 GPT 模型及其组件，以及在 nanoGPT 中的实现。我选择 nanoGPT 是因为它提供了一个大约 300 行代码的 GPT 模型的简单 Python 实现，以及类似的易于理解的训练脚本。具备必要的背景知识后，人们可以通过阅读源代码快速理解 GPT 模型。坦白说，当我第一次检查代码时缺乏这种理解。一些材料仍然让我困惑。然而，我希望通过我所学到的，这个解释能为那些希望获得直观了解的人提供一个起点……
