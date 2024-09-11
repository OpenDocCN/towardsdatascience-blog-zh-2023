# Transformer 模型用于通过微调进行自定义文本分类

> 原文：[https://towardsdatascience.com/transformer-models-for-custom-text-classification-through-fine-tuning-3b065cc08da1?source=collection_archive---------0-----------------------#2023-01-20](https://towardsdatascience.com/transformer-models-for-custom-text-classification-through-fine-tuning-3b065cc08da1?source=collection_archive---------0-----------------------#2023-01-20)

## 关于如何通过微调 DistilBERT 模型来构建垃圾邮件分类器（或任何其他分类器）的教程

[](https://skanda-vivek.medium.com/?source=post_page-----3b065cc08da1--------------------------------)[![Skanda Vivek](../Images/9d25bee2fb75176ca7f7ea6eff7d7ab5.png)](https://skanda-vivek.medium.com/?source=post_page-----3b065cc08da1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3b065cc08da1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3b065cc08da1--------------------------------) [Skanda Vivek](https://skanda-vivek.medium.com/?source=post_page-----3b065cc08da1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F220d9bbb8014&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformer-models-for-custom-text-classification-through-fine-tuning-3b065cc08da1&user=Skanda+Vivek&userId=220d9bbb8014&source=post_page-220d9bbb8014----3b065cc08da1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b065cc08da1--------------------------------) · 4 分钟阅读 · 2023 年 1 月 20 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3b065cc08da1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformer-models-for-custom-text-classification-through-fine-tuning-3b065cc08da1&source=-----3b065cc08da1---------------------bookmark_footer-----------)![](../Images/b7152fa9d2fbe16e6c1e4d3080b00560.png)

[微调的 SMS 垃圾邮件分类器模型](https://huggingface.co/skandavivek2/spam-classifier) 输出 | Skanda Vivek

[DistiBERT 模型](https://arxiv.org/abs/1910.01108) 是 Hugging Face 团队发布的，作为大规模变换器模型（如 BERT）的便宜且更快的替代方案。它 [最初在一篇博客文章中介绍](https://medium.com/huggingface/distilbert-8cf3380435b5)。该模型的工作方式是使用教师-学生训练方法，其中“学生”模型是教师模型的较小版本。然后，模型不是在最终目标输出（基本上是标签类别的独热编码）上进行训练，而是在原始“教师模型”的 softmax 输出上进行训练。这是一个极其简单的想法，作者展示了：

> “可以将 BERT 模型的大小减少 40%，同时保留 97% 的语言理解能力，并且速度提高 60%。”

# 加载和预处理数据以进行分类

在这个例子中，我使用了 [SMS 垃圾短信数据集](https://www.kaggle.com/datasets/uciml/sms-spam-collection-dataset) 来自 UCI 机器学习库，并构建了一个分类器来检测 SPAM 与 HAM（非 SPAM）。数据包含 5,574…
