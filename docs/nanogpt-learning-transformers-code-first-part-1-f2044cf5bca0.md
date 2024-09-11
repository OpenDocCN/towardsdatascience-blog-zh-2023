# 变压器代码学习第一部分：设置

> 原文：[`towardsdatascience.com/nanogpt-learning-transformers-code-first-part-1-f2044cf5bca0?source=collection_archive---------7-----------------------#2023-07-07`](https://towardsdatascience.com/nanogpt-learning-transformers-code-first-part-1-f2044cf5bca0?source=collection_archive---------7-----------------------#2023-07-07)

## 使用 nanoGPT 作为起点的变压器四部分探索

[](https://medium.com/@oaguy1?source=post_page-----f2044cf5bca0--------------------------------)![Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----f2044cf5bca0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f2044cf5bca0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2044cf5bca0--------------------------------) [Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----f2044cf5bca0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5389e25ca1bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnanogpt-learning-transformers-code-first-part-1-f2044cf5bca0&user=Lily+Hughes-Robinson&userId=5389e25ca1bb&source=post_page-5389e25ca1bb----f2044cf5bca0---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2044cf5bca0--------------------------------) ·8 分钟阅读·2023 年 7 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff2044cf5bca0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnanogpt-learning-transformers-code-first-part-1-f2044cf5bca0&user=Lily+Hughes-Robinson&userId=5389e25ca1bb&source=-----f2044cf5bca0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff2044cf5bca0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnanogpt-learning-transformers-code-first-part-1-f2044cf5bca0&source=-----f2044cf5bca0---------------------bookmark_footer-----------)![](img/dfca366786f1d736d9f6aebbeedc4b65.png)

图片由 [Josh Riemer](https://unsplash.com/@joshriemer?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我不知道你怎样，但有时候看代码比读论文更容易。当我在研究 [AdventureGPT](https://github.com/oaguy1/AdventureGPT) 时，我从阅读 [BabyAGI](https://github.com/yoheinakajima/babyagi) 的源代码开始，它是对 [ReAct 论文](https://arxiv.org/abs/2210.03629) 的一个实现，约 600 行 Python 代码。

最近，我通过优秀的 [Cognitive Revolution Podcast](https://www.cognitiverevolution.ai/) 的 [第 33 期](https://www.cognitiverevolution.ai/e33-the-tiny-model-revolution-with-ronen-eldan-and-yuanzhi-li-of-microsoft-research/) 了解到了一篇名为 [TinyStories](https://arxiv.org/abs/2305.07759) 的最新论文。TinyStories 尝试展示在参数达到百万级（而非亿级）的模型也可以在足够高质量的数据上表现有效。在论文中的微软研究人员的案例中，他们使用了由 GPT-3.5 和 GPT-4 生成的合成数据，生成这些数据的零售价大约为 $10k。数据集和模型可以从作者的 [HuggingFace 仓库](https://huggingface.co/roneneldan) 获取。

听说一个模型可以在 30M 和更少的参数上进行训练，我感到非常着迷。作为参考，我在搭载 GTX 1660 Ti 的 Lenovo Legion 5 笔记本上进行所有模型训练和推理。即使仅仅是推理，大多数超过 3B 参数的模型也太大，无法在我的机器上运行。我知道有云计算资源可以购买，但我在闲暇时间学习这些内容，真的只能负担得起我累积的 modest OpenAI 账单……
