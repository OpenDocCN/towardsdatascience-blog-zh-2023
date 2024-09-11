# 如何构建和管理数据资产组合

> 原文：[`towardsdatascience.com/how-to-build-and-manage-a-portfolio-of-data-assets-9df83bd39de6?source=collection_archive---------0-----------------------#2023-09-14`](https://towardsdatascience.com/how-to-build-and-manage-a-portfolio-of-data-assets-9df83bd39de6?source=collection_archive---------0-----------------------#2023-09-14)

## 一种逐步方法

[](https://medium.com/@willemkoenders?source=post_page-----9df83bd39de6--------------------------------)![Willem Koenders](https://medium.com/@willemkoenders?source=post_page-----9df83bd39de6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9df83bd39de6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9df83bd39de6--------------------------------) [Willem Koenders](https://medium.com/@willemkoenders?source=post_page-----9df83bd39de6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa754b81446b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-and-manage-a-portfolio-of-data-assets-9df83bd39de6&user=Willem+Koenders&userId=a754b81446b6&source=post_page-a754b81446b6----9df83bd39de6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9df83bd39de6--------------------------------) ·13 分钟阅读·2023 年 9 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9df83bd39de6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-and-manage-a-portfolio-of-data-assets-9df83bd39de6&user=Willem+Koenders&userId=a754b81446b6&source=-----9df83bd39de6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9df83bd39de6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-and-manage-a-portfolio-of-data-assets-9df83bd39de6&source=-----9df83bd39de6---------------------bookmark_footer-----------)![](img/00239f24b8b26c50ee075753e35947b3.png)

由 [Viktor Forgacs](https://unsplash.com/@sonance) 拍摄，来自 [Unsplash](https://unsplash.com/)。

数据资产（或产品）——一组为一组已识别的用例轻松消费的准备数据或信息——是数据管理领域的热点。能够识别、构建和管理单个数据产品是一回事，但如何在企业级别进行这些工作呢？从何处开始？

数据赋能领导者，尤其是首席数据官，面临这一动员挑战。在这篇观点文章中，我们将讨论如何对数据资产采取投资组合方法。下图 1 展示了逐步的方法，文章的其余部分将详细说明这 7 个步骤。在整个过程中，我们将解释方法论并结合示例。

在我跟随这种方法的各种现实生活例子中，为了避免任何数据来自特定客户的怀疑，同时展示生成式 AI 如何在正确提示下被有效利用，我使用了 ChatGPT 4.0 来生成这些例子。完整的对话记录可以在[这里](https://chat.openai.com/share/2acbcb4e-e77c-437c-ab67-9f319c653c81)查看。

# 步骤 1：用例与影响

第一步是识别对你组织重要的数据驱动用例。你不必一次性为整个企业做这件事——你可以从一个开始……
