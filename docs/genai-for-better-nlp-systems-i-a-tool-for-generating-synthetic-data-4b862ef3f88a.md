# GenAI 用于改进 NLP 系统 I：生成合成数据的工具。

> 原文：[`towardsdatascience.com/genai-for-better-nlp-systems-i-a-tool-for-generating-synthetic-data-4b862ef3f88a?source=collection_archive---------9-----------------------#2023-09-29`](https://towardsdatascience.com/genai-for-better-nlp-systems-i-a-tool-for-generating-synthetic-data-4b862ef3f88a?source=collection_archive---------9-----------------------#2023-09-29)

## 一项关于使用 Python 进行 Prompt Engineering 的 GenAI 生成和增强合成数据的实验。

[](https://nroy0110.medium.com/?source=post_page-----4b862ef3f88a--------------------------------)![Nabanita Roy](https://nroy0110.medium.com/?source=post_page-----4b862ef3f88a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4b862ef3f88a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b862ef3f88a--------------------------------) [Nabanita Roy](https://nroy0110.medium.com/?source=post_page-----4b862ef3f88a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd36a8b28c928&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenai-for-better-nlp-systems-i-a-tool-for-generating-synthetic-data-4b862ef3f88a&user=Nabanita+Roy&userId=d36a8b28c928&source=post_page-d36a8b28c928----4b862ef3f88a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----4b862ef3f88a--------------------------------) · 7 分钟阅读 · 2023 年 9 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4b862ef3f88a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenai-for-better-nlp-systems-i-a-tool-for-generating-synthetic-data-4b862ef3f88a&user=Nabanita+Roy&userId=d36a8b28c928&source=-----4b862ef3f88a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4b862ef3f88a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenai-for-better-nlp-systems-i-a-tool-for-generating-synthetic-data-4b862ef3f88a&source=-----4b862ef3f88a---------------------bookmark_footer-----------)![](img/2c632c9d6ff85d169081e0ad66f249ac.png)

图片由[SR](https://unsplash.com/@lemonmelon?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

机器学习（ML）的一个主要挑战是不平衡的数据及其对 ML 模型的偏差。随着强大生成型 AI（GenAI）模型的出现，我们可以轻松地用合成数据来扩充不平衡的训练数据，特别是对于自然语言处理（NLP）任务。因此，我们可以使用经典的 ML 算法训练模型，以在深度学习模型或直接使用 LLMs 不可行的情况下（如计算成本、内存、基础设施可用性或模型可解释性等原因）获得更好的性能。此外，尽管 LLMs 展现了极大的有效性，我们仍然不能完全信任它们。然而，我们可以利用 LLMs 来帮助我们的数据工作，并克服在构建 NLP 系统中的障碍。

在这篇文章中，我展示了如何使用 GenAI 和 Python 生成合成数据来提高在不平衡数据集中少数类的模型性能，以及如何迭代设计提示以生成期望的结果。

# **平衡行动**
