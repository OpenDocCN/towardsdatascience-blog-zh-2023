# 贝叶斯因果推断的力量：对库的比较分析，揭示数据集中的隐藏因果关系。

> 原文：[`towardsdatascience.com/the-power-of-bayesian-causal-inference-a-comparative-analysis-of-libraries-to-reveal-hidden-d91e8306e25e?source=collection_archive---------3-----------------------#2023-05-22`](https://towardsdatascience.com/the-power-of-bayesian-causal-inference-a-comparative-analysis-of-libraries-to-reveal-hidden-d91e8306e25e?source=collection_archive---------3-----------------------#2023-05-22)

## 通过使用最合适的贝叶斯因果推断库，揭示数据集中的隐藏因果变量：对五个流行库的比较及动手示例。

[](https://erdogant.medium.com/?source=post_page-----d91e8306e25e--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----d91e8306e25e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d91e8306e25e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d91e8306e25e--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----d91e8306e25e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e636e2ef813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-bayesian-causal-inference-a-comparative-analysis-of-libraries-to-reveal-hidden-d91e8306e25e&user=Erdogan+Taskesen&userId=4e636e2ef813&source=post_page-4e636e2ef813----d91e8306e25e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d91e8306e25e--------------------------------) ·20 分钟阅读·2023 年 5 月 22 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd91e8306e25e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-bayesian-causal-inference-a-comparative-analysis-of-libraries-to-reveal-hidden-d91e8306e25e&source=-----d91e8306e25e---------------------bookmark_footer-----------)![](img/9da363a089f70e5f5c65add731ae7d3b.png)

照片由 [Alexander Schimmeck](https://unsplash.com/@alschim?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/photos/Aohf8gqa7Zc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

理解系统或过程中变量的因果效应非常有价值。有多个 Python 库可以帮助确定因果关系。***我将比较五个流行的因果推断库在功能、易用性和灵活性方面的差异。***每个库都附有实际示例。包括的库有[*Bnlearn*](https://github.com/erdogant/bnlearn)*,* [*Pgmpy*](https://github.com/pgmpy/pgmpy)*,* [*CausalNex*](https://github.com/quantumblacklabs/causalnex)*,* [*DoWhy*](https://github.com/py-why/dowhy),以及[*CausalImpact*](https://github.com/jamalsenouci/causalimpact/)。通过本文末尾，您将更好地了解这五个因果推断库，并确定哪个最适合您的用例。

# 背景

因果推断是确定过程或系统中变量之间因果关系的过程。一般来说，我们可以将变量分为两组不同的群体；***驱动变量和乘客变量***。驱动变量是直接影响结果或因变量的变量，而乘客变量则不是...
