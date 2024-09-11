# 通过 AWS SageMaker AutoML 实现机器学习民主化

> 原文：[`towardsdatascience.com/democratizing-machine-learning-with-aws-sagemaker-automl-150299c70396?source=collection_archive---------17-----------------------#2023-04-25`](https://towardsdatascience.com/democratizing-machine-learning-with-aws-sagemaker-automl-150299c70396?source=collection_archive---------17-----------------------#2023-04-25)

## 概述

[](https://brus-patrick63.medium.com/?source=post_page-----150299c70396--------------------------------)![Patrick Brus](https://brus-patrick63.medium.com/?source=post_page-----150299c70396--------------------------------)[](https://towardsdatascience.com/?source=post_page-----150299c70396--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----150299c70396--------------------------------) [Patrick Brus](https://brus-patrick63.medium.com/?source=post_page-----150299c70396--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5db2cf7aa0b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemocratizing-machine-learning-with-aws-sagemaker-automl-150299c70396&user=Patrick+Brus&userId=5db2cf7aa0b7&source=post_page-5db2cf7aa0b7----150299c70396---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----150299c70396--------------------------------) ·16 分钟阅读·2023 年 4 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F150299c70396&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemocratizing-machine-learning-with-aws-sagemaker-automl-150299c70396&user=Patrick+Brus&userId=5db2cf7aa0b7&source=-----150299c70396---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F150299c70396&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemocratizing-machine-learning-with-aws-sagemaker-automl-150299c70396&source=-----150299c70396---------------------bookmark_footer-----------)![](img/9d6177546bde28b57814053a64914628.png)

图片由 [Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

目前，人工智能仍然是最热门的话题之一，特别是随着 ChatGPT 的崛起。许多公司现在正试图利用人工智能从数据中提取有用的见解，以优化他们的流程或推出更好的产品。

然而，构建有效的 AI 模型需要在不同领域拥有大量专业知识，如数据预处理、模型选择、超参数调优等。这些领域都可能非常耗时，并且需要专业知识。

这就是 AutoML 的作用所在。AutoML 自动化了构建 AI 模型所需的许多领域。

AutoML 正迅速成为企业和数据科学家们的热门解决方案。它使组织能够利用 ML 和 AI 做出明智的决策，而不需要他们成为数据科学专家。随着企业对 ML 需求的增加，AutoML 提供了一种简单高效的方法来创建准确的模型，无论个人的专业水平如何。

在这篇文章中，我们将探讨当前市场上一个非常受欢迎的 AutoML 工具 AWS SageMaker AutoML，并演示如何利用它来解决复杂的 ML 用例。
