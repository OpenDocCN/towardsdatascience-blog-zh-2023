# 数据管道编排

> 原文：[`towardsdatascience.com/data-pipeline-orchestration-9887e1b5eb7a?source=collection_archive---------10-----------------------#2023-04-03`](https://towardsdatascience.com/data-pipeline-orchestration-9887e1b5eb7a?source=collection_archive---------10-----------------------#2023-04-03)

## 正确的数据管道管理简化了部署过程，并提高了数据的可用性和可访问性，以供分析使用。

[](https://mshakhomirov.medium.com/?source=post_page-----9887e1b5eb7a--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----9887e1b5eb7a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9887e1b5eb7a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9887e1b5eb7a--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----9887e1b5eb7a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-pipeline-orchestration-9887e1b5eb7a&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----9887e1b5eb7a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9887e1b5eb7a--------------------------------) · 7 分钟阅读 · 2023 年 4 月 3 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9887e1b5eb7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-pipeline-orchestration-9887e1b5eb7a&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----9887e1b5eb7a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9887e1b5eb7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-pipeline-orchestration-9887e1b5eb7a&source=-----9887e1b5eb7a---------------------bookmark_footer-----------)![](img/6ab99cb7a0920a4d1f69a66bee5f00b7.png)

图片由 [Manuel Nägeli](https://unsplash.com/@gwundrig?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 什么是数据编排？

DataOps 团队使用**数据管道编排**作为集中管理和监督端到端数据管道的解决方案。

> 自动化数据管道的过程称为数据编排。

管理数据管道非常重要，因为它几乎影响所有方面，即数据质量、处理速度和数据治理。

> 什么才是有效的数据管道管理？

**透明度和可见性。** 每个团队成员都知道数据如何被转换，数据从哪里来以及数据转换过程在哪里结束，这一点至关重要。

**更快的部署。** 能够持续复制数据管道的元素至关重要。把这些元素视为数据管道的构建模块。因此，当需要创建新的数据管道时，重要的是为每个新的数据处理轻松复制构建模块，而不是从头开始创建。
