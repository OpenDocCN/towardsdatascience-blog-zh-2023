# 使用贝叶斯推断与您的数据集进行对话。

> 原文：[`towardsdatascience.com/chat-with-your-dataset-using-bayesian-inferences-bfd4dc7f8dcd?source=collection_archive---------1-----------------------#2023-11-13`](https://towardsdatascience.com/chat-with-your-dataset-using-bayesian-inferences-bfd4dc7f8dcd?source=collection_archive---------1-----------------------#2023-11-13)

## 向您的数据集提问的能力一直是一个引人入胜的前景。您会惊讶于学习一个本地贝叶斯模型以便于审问数据集是多么容易。

[](https://erdogant.medium.com/?source=post_page-----bfd4dc7f8dcd--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----bfd4dc7f8dcd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bfd4dc7f8dcd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bfd4dc7f8dcd--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----bfd4dc7f8dcd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e636e2ef813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchat-with-your-dataset-using-bayesian-inferences-bfd4dc7f8dcd&user=Erdogan+Taskesen&userId=4e636e2ef813&source=post_page-4e636e2ef813----bfd4dc7f8dcd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bfd4dc7f8dcd--------------------------------) ·13 分钟阅读·2023 年 11 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbfd4dc7f8dcd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchat-with-your-dataset-using-bayesian-inferences-bfd4dc7f8dcd&user=Erdogan+Taskesen&userId=4e636e2ef813&source=-----bfd4dc7f8dcd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbfd4dc7f8dcd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchat-with-your-dataset-using-bayesian-inferences-bfd4dc7f8dcd&source=-----bfd4dc7f8dcd---------------------bookmark_footer-----------)![](img/cd3cbd949d64dc93de294b51ab286416.png)

照片由 [Vadim Bogulov](https://unsplash.com/pt-br/@franku84?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/MfBnqUOz_qY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随着 chatGPT 类模型的兴起，更广泛的用户可以分析自己的数据集，所谓的“*提问*”。虽然这很好，但当将其作为自动化管道中的分析步骤时，这种方法也有缺点。特别是当模型的结果可能产生重大影响时尤为明显。为了保持控制并确保结果的准确性，我们还可以使用贝叶斯推断来与数据集对话。*在本博客中，我们将介绍如何学习贝叶斯模型并在数据科学薪资数据集上应用 do-calculus。我将演示如何创建一个模型，使你可以“提问”你的数据集并保持控制。你会惊讶于使用* [*bnlearn 库*](https://erdogant.github.io/bnlearn/pages/html/index.html)* 创建这样一个模型的简便性。*

# 介绍

从数据集中提取有价值的见解是数据科学家和分析师面临的持续挑战。像 ChatGPT 的模型使得互动…
