# R 中的十大错误及其修复方法

> 原文：[https://towardsdatascience.com/top-10-errors-in-r-and-how-to-fix-them-e8848718c836?source=collection_archive---------14-----------------------#2023-02-07](https://towardsdatascience.com/top-10-errors-in-r-and-how-to-fix-them-e8848718c836?source=collection_archive---------14-----------------------#2023-02-07)

## 在这篇文章中，我突出了 R 中最常见的 10 个错误及其解决方法。我还提到了一些警告（与错误不同）。

[](https://antoinesoetewey.medium.com/?source=post_page-----e8848718c836--------------------------------)[![Antoine Soetewey](../Images/51d7837d18ff15a62cac2343a485e35d.png)](https://antoinesoetewey.medium.com/?source=post_page-----e8848718c836--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e8848718c836--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e8848718c836--------------------------------) [Antoine Soetewey](https://antoinesoetewey.medium.com/?source=post_page-----e8848718c836--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fca32a96e6dc7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftop-10-errors-in-r-and-how-to-fix-them-e8848718c836&user=Antoine+Soetewey&userId=ca32a96e6dc7&source=post_page-ca32a96e6dc7----e8848718c836---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e8848718c836--------------------------------) ·19分钟阅读·2023年2月7日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe8848718c836&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftop-10-errors-in-r-and-how-to-fix-them-e8848718c836&source=-----e8848718c836---------------------bookmark_footer-----------)![](../Images/4de29e9b3b38890b3f696b925355bfba.png)

图片由 [Kai Pilger](https://unsplash.com/@kaip?utm_source=medium&utm_medium=referral) 提供

# 引言

如果你刚开始使用 R，你经常会遇到阻止代码运行的错误。我记得刚开始使用 R 时，我的代码错误频繁到几乎让我放弃学习这个编程语言。我甚至记得因为找不到问题原因，我回过几次 Excel 完成我的分析。

幸运的是，尽管一开始遇到了困难，我还是强迫自己继续前行。即使今天我几乎每次编写R代码时仍会遇到错误，但通过经验和练习，修复这些错误所需的时间越来越少。如果你也在一开始时挣扎，请放心，这很正常：每个人在学习一种新的编程语言时都会经历一些挫折（不仅仅是R语言如此）。

在这篇文章中，我强调了**R语言中最常见的10种错误及其修复方法**。当然，错误的种类取决于你的代码和分析，因此不可能涵盖所有错误（而且Google在这方面做得比我好得多）。不过，我还是想重点讲述一些…
