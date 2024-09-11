# 使用 Black 和 GitHub Actions 维护干净的 Python 代码

> 原文：[`towardsdatascience.com/black-with-git-hub-actions-4ffc5c61b5fe?source=collection_archive---------11-----------------------#2023-01-16`](https://towardsdatascience.com/black-with-git-hub-actions-4ffc5c61b5fe?source=collection_archive---------11-----------------------#2023-01-16)

## 没有人愿意面对混乱的代码库；很少有人有耐心去清理它

[](https://thuwarakesh.medium.com/?source=post_page-----4ffc5c61b5fe--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----4ffc5c61b5fe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4ffc5c61b5fe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4ffc5c61b5fe--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----4ffc5c61b5fe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fblack-with-git-hub-actions-4ffc5c61b5fe&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----4ffc5c61b5fe---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4ffc5c61b5fe--------------------------------) ·8 分钟阅读·2023 年 1 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4ffc5c61b5fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fblack-with-git-hub-actions-4ffc5c61b5fe&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----4ffc5c61b5fe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4ffc5c61b5fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fblack-with-git-hub-actions-4ffc5c61b5fe&source=-----4ffc5c61b5fe---------------------bookmark_footer-----------)![](img/5b6f032c9c15cbfb6fd4451172f5f685.png)

像这个清洁机器人一样，我们可以建立一个自动系统，通过 Black 和 GitHub Actions 清理我们的 Python 代码库。 — 照片来自 [Onur Binay](https://unsplash.com/@onurbinay?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

编写代码本身已经很困难，但确保代码格式良好且易于阅读则可能更具挑战性。

我的编程技能在十年中有了很大提高。但其中大部分并不是一些花哨的 API 使用或其他东西，而是如何格式化代码。

在我最早的编程岁月中，我只关注结果。当然，这也是每个程序员的*终极目标*。但随着我的进步，我明白了优秀的编码不仅仅是完成任务这么简单。

将我的代码与他人分享并不容易，而且往往会得到大量的问题需要解释。即使有支持文档和内联注释，其他程序员也觉得理解我的代码非常有挑战性。

# 程序员的工作并不仅仅是写出能工作的代码。

除了文档，还有些东西使得优秀的代码变得更为卓越。而编程社区没有遗漏去寻找这个因素。

这与我们如何格式化代码有关。

Python，我最喜欢的编程语言，其语法最为简单。但这并不保证所有阅读你 Python 代码的人都会理解它。
