# 揭示传统 DiD 方法的局限性

> 原文：[`towardsdatascience.com/uncovering-the-limitations-of-traditional-did-method-2f068f56d19a?source=collection_archive---------9-----------------------#2023-02-21`](https://towardsdatascience.com/uncovering-the-limitations-of-traditional-did-method-2f068f56d19a?source=collection_archive---------9-----------------------#2023-02-21)

## 处理多个时间段和错综复杂的处理时间

[](https://medium.com/@nalagoz13?source=post_page-----2f068f56d19a--------------------------------)![Nazlı Alagöz](https://medium.com/@nalagoz13?source=post_page-----2f068f56d19a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2f068f56d19a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f068f56d19a--------------------------------) [Nazlı Alagöz](https://medium.com/@nalagoz13?source=post_page-----2f068f56d19a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ba02da50bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-the-limitations-of-traditional-did-method-2f068f56d19a&user=Nazl%C4%B1+Alag%C3%B6z&userId=4ba02da50bf&source=post_page-4ba02da50bf----2f068f56d19a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2f068f56d19a--------------------------------) ·11 分钟阅读·2023 年 2 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2f068f56d19a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-the-limitations-of-traditional-did-method-2f068f56d19a&user=Nazl%C4%B1+Alag%C3%B6z&userId=4ba02da50bf&source=-----2f068f56d19a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2f068f56d19a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-the-limitations-of-traditional-did-method-2f068f56d19a&source=-----2f068f56d19a---------------------bookmark_footer-----------)![](img/44dea706340ac506d236a11bad3313ec.png)

封面图片，由作者使用[NightCafé](https://creator.nightcafe.studio/)生成

**差分中的差分（DiD）** 是一种流行的统计方法，用于通过比较两组在处理前后的结果差异来估计干预的因果影响。大多数 DiD 指南只关注经典的 DiD 设置，其中只有两个时间段和两组（处理组和未处理组）。

然而，在许多实际应用中的 DiD 中，存在多个时间段和处理时间的变异。**近期关于 DiD 的研究显示，在这些情况下，DiD 可能会给出显著误导的处理效应估计**。在某些场景中，处理效应估计可能与实际处理效应的符号相反。

**在本文中，我将讨论在经典 DiD 设置中，当存在错位的处理时间和多个时间段时，可能出现的一个重要问题**。我还会提出解决这一问题的方案。值得注意的是，虽然我将重点讨论 DiD 中的这一问题，但要获得其他潜在挑战的更全面概述，你可以参考我之前的文章。此外，我还将在本文的末尾提供进一步的资源，供那些希望深入了解的读者…
