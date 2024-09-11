# 重新塑造模型的记忆，无需重新训练

> 原文：[`towardsdatascience.com/reshaping-the-models-memory-without-the-need-for-retraining-9ade69f56296?source=collection_archive---------5-----------------------#2023-10-20`](https://towardsdatascience.com/reshaping-the-models-memory-without-the-need-for-retraining-9ade69f56296?source=collection_archive---------5-----------------------#2023-10-20)

## | AI | 大型语言模型| 机器遗忘|

## 消除大型语言模型学习的任何问题内容的回声

[](https://salvatore-raieli.medium.com/?source=post_page-----9ade69f56296--------------------------------)![萨尔瓦托雷·拉伊利](https://salvatore-raieli.medium.com/?source=post_page-----9ade69f56296--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9ade69f56296--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ade69f56296--------------------------------) [萨尔瓦托雷·拉伊利](https://salvatore-raieli.medium.com/?source=post_page-----9ade69f56296--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freshaping-the-models-memory-without-the-need-for-retraining-9ade69f56296&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----9ade69f56296---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ade69f56296--------------------------------) ·11 min read·2023 年 10 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9ade69f56296&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freshaping-the-models-memory-without-the-need-for-retraining-9ade69f56296&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----9ade69f56296---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9ade69f56296&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freshaping-the-models-memory-without-the-need-for-retraining-9ade69f56296&source=-----9ade69f56296---------------------bookmark_footer-----------)![](img/4d98c180635de8d1ec4607a94fdfa029.png)

图片由 [Drew Saurus](https://unsplash.com/@drew_saurus?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> “宽恕是智慧，遗忘是天才。”
> 
> ― **乔伊斯·卡里**

[大型语言模型](https://en.wikipedia.org/wiki/Large_language_model)（LLMs）已经引起了全球的关注。在不到一年的时间里，它们变得无处不在，现在被数以百万计的用户使用。这些模型通常用大量文本（包括有问题的材料和敏感数据）进行训练。如何让一个能够存储整个人类知识的模型忘记信息呢？

# 学会如何忘记

![](img/c7c7cf08a9381a67f4ce29c3f641e677.png)

图片由 [Paul Pastourmatzis](https://unsplash.com/@pueblovista?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 大型语言模型（LLMs）不仅体现了我们的成就，也展示了面临的挑战 — [来源](https://arxiv.org/pdf/2310.02238.pdf)

LLMs 以其从大量文本中学习并识别语言模式和文化细微差别的能力，令用户和研究人员都感到惊讶。虽然它们可能成为新应用和科学研究的基础，但...
