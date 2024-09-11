# T5：文本到文本的变换器（第一部分）

> 原文：[`towardsdatascience.com/t5-text-to-text-transformers-part-one-6b655f27c79a?source=collection_archive---------5-----------------------#2023-06-27`](https://towardsdatascience.com/t5-text-to-text-transformers-part-one-6b655f27c79a?source=collection_archive---------5-----------------------#2023-06-27)

## 创建一个统一的语言建模框架

[](https://wolfecameron.medium.com/?source=post_page-----6b655f27c79a--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----6b655f27c79a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b655f27c79a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b655f27c79a--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----6b655f27c79a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ft5-text-to-text-transformers-part-one-6b655f27c79a&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----6b655f27c79a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b655f27c79a--------------------------------) ·14 min read·Jun 27, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6b655f27c79a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ft5-text-to-text-transformers-part-one-6b655f27c79a&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----6b655f27c79a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6b655f27c79a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ft5-text-to-text-transformers-part-one-6b655f27c79a&source=-----6b655f27c79a---------------------bookmark_footer-----------)![](img/03be7b5832a9d134961e51b08dd7a5a7.png)

（照片由 [Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/text?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

迁移学习范式包括两个主要阶段。首先，我们在一大堆数据上对深度神经网络进行预训练。然后，我们在一个更具体的下游数据集上对模型进行微调（即，再次训练）。这些阶段的具体实现形式可能有许多不同的变体。例如，在计算机视觉中，我们通常使用监督学习目标在 ImageNet 数据集上对模型进行预训练。然后，这些模型在下游数据集上进行监督微调（即，我们实际尝试解决的任务）。另外，在自然语言处理（NLP）中，我们通常在未标记的文本语料库上进行[自监督](https://cameronrwolfe.substack.com/i/76273144/self-supervised-learning)预训练。

结合大型、深层神经网络与海量的（预）训练数据集，通常会取得令人印象深刻的结果。这一发现对于自然语言处理（NLP）尤其适用。由于原始文本数据在互联网上自由获取，我们可以简单地下载一个庞大的文本语料库，在这些数据上预训练一个大型神经网络，然后在各种下游任务上进行微调（或直接使用零样本/少样本学习技术）。这一大规模迁移学习方法最初由 BERT [2] 探索，该方法在未标记数据上使用[变换器编码器](https://cameronrwolfe.substack.com/i/76273144/transformer-encoders)进行预训练，并使用[掩码目标](https://cameronrwolfe.substack.com/i/76273144/training-bert)。
