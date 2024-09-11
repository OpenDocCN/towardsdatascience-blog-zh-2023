# 变压器在预测Twitter账户身份中的强大作用

> 原文：[https://towardsdatascience.com/twitter-account-identity-prediction-with-large-language-models-c3ffef114d34?source=collection_archive---------18-----------------------#2023-03-07](https://towardsdatascience.com/twitter-account-identity-prediction-with-large-language-models-c3ffef114d34?source=collection_archive---------18-----------------------#2023-03-07)

## 利用大型语言模型进行高级自然语言处理

## 如何使用最先进的模型进行准确的文本分类

[](https://johnadeojo.medium.com/?source=post_page-----c3ffef114d34--------------------------------)[![John Adeojo](../Images/f6460fae462b055d36dce16fefcd142c.png)](https://johnadeojo.medium.com/?source=post_page-----c3ffef114d34--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c3ffef114d34--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c3ffef114d34--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----c3ffef114d34--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwitter-account-identity-prediction-with-large-language-models-c3ffef114d34&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----c3ffef114d34---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3ffef114d34--------------------------------) ·9分钟阅读·2023年3月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc3ffef114d34&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwitter-account-identity-prediction-with-large-language-models-c3ffef114d34&user=John+Adeojo&userId=f933e1637e40&source=-----c3ffef114d34---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc3ffef114d34&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwitter-account-identity-prediction-with-large-language-models-c3ffef114d34&source=-----c3ffef114d34---------------------bookmark_footer-----------)![](../Images/38cebe369eb5ad1ff89e4d19b53744de.png)

图片由[Jonathan Cooper](https://unsplash.com/ko/@theshuttervision?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

本项目旨在构建一个能够根据账户的推文预测账户身份的模型。我将详细讲解从数据处理、模型微调到性能评估的步骤。

*在继续之前，我需要说明这里的身份定义为男性、女性或品牌。这绝不反映我对性别身份的看法，这只是一个展示 transformers 在序列分类中强大能力的玩具项目。在一些代码片段中，你可能会注意到性别被用来指代身份，这只是数据的到达方式。*

# 方法

由于文本数据的复杂性以及非线性关系的建模，我排除了更简单的方法，选择了利用预训练的 transformer 模型来进行这个项目。

Transformers 是当前自然语言处理和理解任务的最先进技术。Hugging Face 的 [Transformer](https://huggingface.co/) 库为你提供了访问数千个...
