# 精调的 LLM 用于情感预测——如何分析和评估

> 原文：[`towardsdatascience.com/fine-tuned-llms-for-sentiment-prediction-how-to-analyze-and-evaluate-1c31b4f06835?source=collection_archive---------8-----------------------#2023-08-09`](https://towardsdatascience.com/fine-tuned-llms-for-sentiment-prediction-how-to-analyze-and-evaluate-1c31b4f06835?source=collection_archive---------8-----------------------#2023-08-09)

## 在 Hugging Face 上评估情感预测模型

[](https://pranay-dave9.medium.com/?source=post_page-----1c31b4f06835--------------------------------)![Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----1c31b4f06835--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c31b4f06835--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c31b4f06835--------------------------------) [Pranay Dave](https://pranay-dave9.medium.com/?source=post_page-----1c31b4f06835--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F89d4bb7ead78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tuned-llms-for-sentiment-prediction-how-to-analyze-and-evaluate-1c31b4f06835&user=Pranay+Dave&userId=89d4bb7ead78&source=post_page-89d4bb7ead78----1c31b4f06835---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c31b4f06835--------------------------------) ·10 分钟阅读·2023 年 8 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1c31b4f06835&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tuned-llms-for-sentiment-prediction-how-to-analyze-and-evaluate-1c31b4f06835&user=Pranay+Dave&userId=89d4bb7ead78&source=-----1c31b4f06835---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1c31b4f06835&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tuned-llms-for-sentiment-prediction-how-to-analyze-and-evaluate-1c31b4f06835&source=-----1c31b4f06835---------------------bookmark_footer-----------)![](img/984be0b6638392acc2e46f468ea1b7f1.png)

图片由 [Oleksandr Baiev](https://unsplash.com/@bayevs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/photos/PQhq3qLebmc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

情感分析在大型语言模型（LLMs）时代经历了显著的变革。由于 LLMs 能够理解文本的上下文，它们正证明自己是一种非常强大的情感分析方式。在 Hugging Face 上可用于情感分析的 LLMs 数量令人印象深刻。我在写这篇文章时最后检查时，Hugging Face 上用于情感分析的模型数量是[3017](https://huggingface.co/models?sort=trending&search=sentiment)！这是一个相当大的数字。过去用少数几种技术进行情感分析的时代已经过去了，比如使用 TFIDF 特征的传统机器学习、统计积极和消极词汇，或使用像 VADER 这样的库。

尽管众多模型的出现令人兴奋，但也可能让人不知所措。因此，本文将帮助你在情感分析的 LLM 丛林中找到方向。我将介绍顶级模型，并展示如何分析和评估它们。这可以帮助你更好地理解哪个模型适合你的情感分析需求。

# 为什么需要用你的数据分析和评估模型
