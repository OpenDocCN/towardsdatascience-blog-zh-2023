# 你 NLP 模型的终极预处理管道

> 原文：[`towardsdatascience.com/the-ultimate-preprocessing-pipeline-for-your-nlp-models-80afd92650fe?source=collection_archive---------15-----------------------#2023-05-08`](https://towardsdatascience.com/the-ultimate-preprocessing-pipeline-for-your-nlp-models-80afd92650fe?source=collection_archive---------15-----------------------#2023-05-08)

## 通过提供最佳输入，最大化训练 NLP 机器学习模型的效果

[](https://rahulraj24.medium.com/?source=post_page-----80afd92650fe--------------------------------)![Rahulraj Singh](https://rahulraj24.medium.com/?source=post_page-----80afd92650fe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----80afd92650fe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----80afd92650fe--------------------------------) [Rahulraj Singh](https://rahulraj24.medium.com/?source=post_page-----80afd92650fe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fafe0d8607525&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-preprocessing-pipeline-for-your-nlp-models-80afd92650fe&user=Rahulraj+Singh&userId=afe0d8607525&source=post_page-afe0d8607525----80afd92650fe---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----80afd92650fe--------------------------------) · 10 分钟阅读 · 2023 年 5 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F80afd92650fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-preprocessing-pipeline-for-your-nlp-models-80afd92650fe&user=Rahulraj+Singh&userId=afe0d8607525&source=-----80afd92650fe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F80afd92650fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-preprocessing-pipeline-for-your-nlp-models-80afd92650fe&source=-----80afd92650fe---------------------bookmark_footer-----------)![](img/e689a96f5fd638b621c932f2bb07d426.png)

图片由 [Cyrus Crossan](https://unsplash.com/@cys_escapes?utm_source=medium&utm_medium=referral) 拍摄，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

如果你之前做过文本摘要项目，你可能会注意到很难看到你期望的结果。你心中有一个算法应如何工作的概念，以及它应该在文本摘要中标记哪些句子，但往往算法给出的结果是“不是很准确”。更有趣的是关键词提取，因为从主题建模到向量嵌入的各种算法都很优秀，但给定一段文本作为输入，它们给出的结果仍然是“不是很准确”，因为最常出现的词不一定是段落中最重要的词。

> 预处理和数据清理的要求在很大程度上取决于你要解决的用例。我会尝试创建一个通用的管道，应该适用于所有 NLP 模型，但你始终需要调整步骤以获得最佳结果。在这个故事中，我将重点关注解决**主题建模、关键词提取和文本摘要**的 NLP 模型。
