# 为什么 SOLID 设计很重要：避免代码异味并编写可维护的代码

> 原文：[`towardsdatascience.com/why-solid-design-matters-avoid-code-smells-and-write-maintainable-code-553b3c6c0ca8?source=collection_archive---------21-----------------------#2023-01-06`](https://towardsdatascience.com/why-solid-design-matters-avoid-code-smells-and-write-maintainable-code-553b3c6c0ca8?source=collection_archive---------21-----------------------#2023-01-06)

## 使用 SOLID 原则来设计可读、可扩展和可维护的软件

[](https://medium.knulst.de/?source=post_page-----553b3c6c0ca8--------------------------------)![Paul Knulst](https://medium.knulst.de/?source=post_page-----553b3c6c0ca8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----553b3c6c0ca8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----553b3c6c0ca8--------------------------------) [Paul Knulst](https://medium.knulst.de/?source=post_page-----553b3c6c0ca8--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1282c85b5cbc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-solid-design-matters-avoid-code-smells-and-write-maintainable-code-553b3c6c0ca8&user=Paul+Knulst&userId=1282c85b5cbc&source=post_page-1282c85b5cbc----553b3c6c0ca8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----553b3c6c0ca8--------------------------------) · 11 分钟阅读 · 2023 年 1 月 6 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F553b3c6c0ca8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-solid-design-matters-avoid-code-smells-and-write-maintainable-code-553b3c6c0ca8&user=Paul+Knulst&userId=1282c85b5cbc&source=-----553b3c6c0ca8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F553b3c6c0ca8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-solid-design-matters-avoid-code-smells-and-write-maintainable-code-553b3c6c0ca8&source=-----553b3c6c0ca8---------------------bookmark_footer-----------)![](img/b4c330587de200d1eb6c4001a63fe8ba.png)

图片由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) / [Unsplash](https://unsplash.com/s/photos/code-javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

# 介绍

SOLID 是面向对象设计的五项原则的缩写，最早由 Robert C. Martin 在 2000 年代初期提出。这些原则可以帮助开发者创建更具可维护性和可扩展性的软件系统。五项原则包括：

![](img/90521a414adf6b65d9386cf3bcc7250f.png)

所有五个 SOLID 原则——由作者可视化

在本文中，我想展示 SOLID 原则在构建可读且可维护的软件应用中的强大力量。此外，我将提供如何使用 TypeScript 应用 SOLID 原则的实际示例。

这些示例将演示如何利用 SOLID 原则设计和构建更好、更具可扩展性的应用。

无论你是开发者、数据科学家，还是去中心化应用的用户，这些示例将为你提供应用 SOLID 原则的宝贵起点。

# 单一职责原则
