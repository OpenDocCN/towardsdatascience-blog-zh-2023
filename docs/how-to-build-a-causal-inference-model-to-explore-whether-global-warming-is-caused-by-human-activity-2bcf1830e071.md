# 如何构建因果推断机器学习模型来探索全球变暖是否由人类活动引起

> 原文：[https://towardsdatascience.com/how-to-build-a-causal-inference-model-to-explore-whether-global-warming-is-caused-by-human-activity-2bcf1830e071?source=collection_archive---------4-----------------------#2023-01-02](https://towardsdatascience.com/how-to-build-a-causal-inference-model-to-explore-whether-global-warming-is-caused-by-human-activity-2bcf1830e071?source=collection_archive---------4-----------------------#2023-01-02)

## 如何使用 Python 和 DoWhy 库构建因果推断模型以探索全球变暖的原因

[](https://grahamharrison-86487.medium.com/?source=post_page-----2bcf1830e071--------------------------------)[![Graham Harrison](../Images/c6bfe00c6e0cfcdf3bd042c7fdc03554.png)](https://grahamharrison-86487.medium.com/?source=post_page-----2bcf1830e071--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2bcf1830e071--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2bcf1830e071--------------------------------) [Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----2bcf1830e071--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbd1c93739f33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-causal-inference-model-to-explore-whether-global-warming-is-caused-by-human-activity-2bcf1830e071&user=Graham+Harrison&userId=bd1c93739f33&source=post_page-bd1c93739f33----2bcf1830e071---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2bcf1830e071--------------------------------) ·13分钟阅读·2023年1月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2bcf1830e071&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-causal-inference-model-to-explore-whether-global-warming-is-caused-by-human-activity-2bcf1830e071&user=Graham+Harrison&userId=bd1c93739f33&source=-----2bcf1830e071---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2bcf1830e071&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-a-causal-inference-model-to-explore-whether-global-warming-is-caused-by-human-activity-2bcf1830e071&source=-----2bcf1830e071---------------------bookmark_footer-----------)![](../Images/a61ed753b6249ceb58c57f3115df7f48.png)

由[Patrick Hendry](https://unsplash.com/@worldsbetweenlines?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来源于[Unsplash](https://unsplash.com/s/photos/climate-change?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

我最近发布了一篇文章，提供了一个简单的教程，介绍如何使用`Pgmpy`库开始进行因果推断 -

[](/a-simple-explanation-of-causal-inference-in-python-357509506f31?source=post_page-----2bcf1830e071--------------------------------) [## Python中的因果推断简单解释

### 如何在Python中构建端到端因果推断模型的直接解释

towardsdatascience.com](/a-simple-explanation-of-causal-inference-in-python-357509506f31?source=post_page-----2bcf1830e071--------------------------------)

文章的读者之一指出，数据的简单性（例如疫苗接种 = 1 或 0，疾病 = 1 或 0）意味着因果推断模型只能用于解决相对简单的问题，因此它们在实际问题中的适用性有限。

这促使我寻找一个大的问题，尝试使用因果推断模型来解决。我选择了“全球变暖”，特别是气候否认，原因如下 -

+   如果我能够开发一个有效的模型，它将驳斥因果推断的提议……
