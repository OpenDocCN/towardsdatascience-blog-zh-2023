# 随机变量参数的最大似然估计

> 原文：[https://towardsdatascience.com/maximum-likelihood-estimation-4a1a866dfa70?source=collection_archive---------4-----------------------#2023-12-10](https://towardsdatascience.com/maximum-likelihood-estimation-4a1a866dfa70?source=collection_archive---------4-----------------------#2023-12-10)

## 通过观察数据最高可能性对参数进行建模

[](https://romanmichaelpaolucci.medium.com/?source=post_page-----4a1a866dfa70--------------------------------)[![Roman Paolucci](../Images/61d8dd3f53507c1d69cc441c4303400b.png)](https://romanmichaelpaolucci.medium.com/?source=post_page-----4a1a866dfa70--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4a1a866dfa70--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4a1a866dfa70--------------------------------) [Roman Paolucci](https://romanmichaelpaolucci.medium.com/?source=post_page-----4a1a866dfa70--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fba54f7458d02&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaximum-likelihood-estimation-4a1a866dfa70&user=Roman+Paolucci&userId=ba54f7458d02&source=post_page-ba54f7458d02----4a1a866dfa70---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4a1a866dfa70--------------------------------) ·11 min read·Dec 10, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4a1a866dfa70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaximum-likelihood-estimation-4a1a866dfa70&user=Roman+Paolucci&userId=ba54f7458d02&source=-----4a1a866dfa70---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4a1a866dfa70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaximum-likelihood-estimation-4a1a866dfa70&source=-----4a1a866dfa70---------------------bookmark_footer-----------)![](../Images/3914d7333e71686deb52327047a97ded.png)

弗朗西斯科·温加罗的照片：[https://www.pexels.com/photo/blue-and-white-abstract-painting-1912832/](https://www.pexels.com/photo/blue-and-white-abstract-painting-1912832/)

概率和统计学中的概念可能会有些难以捉摸，因为高级数学、糟糕的符号表示以及随机变量和数据的纠缠。本文阐明了估计器、估计、偏差和方差以及最大似然估计方法在随机变量和数据之间的关系。

本文将分为以下几个部分。

+   **概率质量和分布函数的参数**

+   **估计器**

+   **估计**

+   **偏差和方差**

+   **最大似然估计**

+   **Quant Guild** 的[**自荐**](https://quantguild.com/)

## 概率质量和分布函数的参数

本文不是关于常见随机变量的入门指南（[这篇文章可以帮助你了解](https://medium.com/quant-guild/common-random-variables-f30c537a01e4)）。我建议您阅读那篇文章，或在继续之前对基础概率（公理、质量/分布函数等）有坚实的基础。

让我们在病人到达医院的情境中讨论一个例子。
