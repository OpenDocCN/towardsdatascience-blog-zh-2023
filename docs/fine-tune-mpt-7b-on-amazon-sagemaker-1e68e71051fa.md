# 在 Amazon SageMaker 上微调 MPT-7B

> 原文：[`towardsdatascience.com/fine-tune-mpt-7b-on-amazon-sagemaker-1e68e71051fa?source=collection_archive---------5-----------------------#2023-06-20`](https://towardsdatascience.com/fine-tune-mpt-7b-on-amazon-sagemaker-1e68e71051fa?source=collection_archive---------5-----------------------#2023-06-20)

![](img/2f5691f2830e17d1af6c907a144f4567.png)

图片由 [Jeffery Ho](https://unsplash.com/@jefferyho?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

## 了解如何准备数据集并创建训练作业，以在 Amazon SageMaker 上微调 MPT-7B

[](https://medium.com/@joao.pereira.abt?source=post_page-----1e68e71051fa--------------------------------)![João Pereira](https://medium.com/@joao.pereira.abt?source=post_page-----1e68e71051fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e68e71051fa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e68e71051fa--------------------------------) [João Pereira](https://medium.com/@joao.pereira.abt?source=post_page-----1e68e71051fa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6743ea128017&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-mpt-7b-on-amazon-sagemaker-1e68e71051fa&user=Jo%C3%A3o+Pereira&userId=6743ea128017&source=post_page-6743ea128017----1e68e71051fa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e68e71051fa--------------------------------) ·9 分钟阅读·2023 年 6 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1e68e71051fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-mpt-7b-on-amazon-sagemaker-1e68e71051fa&user=Jo%C3%A3o+Pereira&userId=6743ea128017&source=-----1e68e71051fa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1e68e71051fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-mpt-7b-on-amazon-sagemaker-1e68e71051fa&source=-----1e68e71051fa---------------------bookmark_footer-----------)

每周都有新的大型语言模型（LLMs）发布，每个模型都试图超越其前任并登上评估排行榜。最新发布的模型之一是 [MPT-7B](https://www.mosaicml.com/blog/mpt-7b)，由 MosaicML 发布。与其他同类模型不同的是，这款拥有 70 亿参数的模型是开源的，并且具有商业使用许可（[Apache 2.0 许可证](https://github.com/mosaicml/llm-foundry/blob/main/LICENSE)）🚀。

类似 MPT-7B 的基础模型是在从网络爬取的拥有万亿个标记（100 tokens ~ 75 words）的数据集上进行预训练的，当提示得当时，它们能够生成令人印象深刻的输出。然而，要真正发掘大语言模型在实际应用中的价值，仅仅依靠智能提示工程可能不足以使其适应你的用例，因此，需要在特定领域的数据集上对基础模型进行微调。

大语言模型（LLMs）具有数十亿个参数，因此，微调如此大型的模型是具有挑战性的。好消息是，相较于对基础模型进行预训练，微调的成本和时间都要便宜得多，因为 1) 特定领域的数据集是“较小”的，2) 微调只需对训练数据进行少量遍历。
