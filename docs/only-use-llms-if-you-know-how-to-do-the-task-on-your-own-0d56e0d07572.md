# 只有在你知道如何独立完成任务时才使用 LLM

> 原文：[`towardsdatascience.com/only-use-llms-if-you-know-how-to-do-the-task-on-your-own-0d56e0d07572?source=collection_archive---------9-----------------------#2023-10-25`](https://towardsdatascience.com/only-use-llms-if-you-know-how-to-do-the-task-on-your-own-0d56e0d07572?source=collection_archive---------9-----------------------#2023-10-25)

## 否则，你可能会面临无声的错误或严峻的后果

[](https://sonery.medium.com/?source=post_page-----0d56e0d07572--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----0d56e0d07572--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0d56e0d07572--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----0d56e0d07572--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----0d56e0d07572--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fonly-use-llms-if-you-know-how-to-do-the-task-on-your-own-0d56e0d07572&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----0d56e0d07572---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0d56e0d07572--------------------------------) ·5 min 阅读·2023 年 10 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0d56e0d07572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fonly-use-llms-if-you-know-how-to-do-the-task-on-your-own-0d56e0d07572&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----0d56e0d07572---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0d56e0d07572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fonly-use-llms-if-you-know-how-to-do-the-task-on-your-own-0d56e0d07572&source=-----0d56e0d07572---------------------bookmark_footer-----------)![](img/3fa18907e4700b861655f9aa3c121946.png)

（图像由作者使用 Midjourney 创建）

对大多数人（或我们所有人）而言，LLM 是能够神奇地快速完成复杂任务的神秘盒子。只要它们能够满足我们的需求，我们通常不会关注“如何”部分。

ChatGPT 和其他 LLM 无疑是生产力的提升者。它们能够轻松处理各种任务，否则这些任务将会繁琐且耗时。

然而，我们不能完全依赖它们。例如，当涉及数据分析时，我们如何确保 ChatGPT 对数据的见解是准确的？是的，它知道 Pandas，一个流行的数据分析库，但如果它犯了错误怎么办？或者，如果它只部分完成了任务而无法继续完成剩下的工作呢？

最好的补充 ChatGPT 的解决方案就是你自己。你需要知道如何独立完成任务，这样：

1.  你可以确保 ChatGPT 的解决方案是正确的。

1.  当 ChatGPT 无法执行任务或不知道怎么做时，你可以替代它。

在本文中，我将展示三个示例，以支持我之前提到的两个主张。

## 示例 1：使用 Pandas 进行数据清理
