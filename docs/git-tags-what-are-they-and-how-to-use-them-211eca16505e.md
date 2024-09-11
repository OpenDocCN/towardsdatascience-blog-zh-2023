# Git 标签：它们是什么以及如何使用它们

> 原文：[https://towardsdatascience.com/git-tags-what-are-they-and-how-to-use-them-211eca16505e?source=collection_archive---------3-----------------------#2023-07-04](https://towardsdatascience.com/git-tags-what-are-they-and-how-to-use-them-211eca16505e?source=collection_archive---------3-----------------------#2023-07-04)

## 它们用于什么，如何创建本地标签，如何查看以及如何删除它们

[](https://philip-wilkinson.medium.com/?source=post_page-----211eca16505e--------------------------------)[![Philip Wilkinson, Ph.D.](../Images/9811fa38963c29193b01a5cf856d014f.png)](https://philip-wilkinson.medium.com/?source=post_page-----211eca16505e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----211eca16505e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----211eca16505e--------------------------------) [Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----211eca16505e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fec0e018f30da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgit-tags-what-are-they-and-how-to-use-them-211eca16505e&user=Philip+Wilkinson%2C+Ph.D.&userId=ec0e018f30da&source=post_page-ec0e018f30da----211eca16505e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----211eca16505e--------------------------------) ·4 min read·2023年7月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F211eca16505e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgit-tags-what-are-they-and-how-to-use-them-211eca16505e&user=Philip+Wilkinson%2C+Ph.D.&userId=ec0e018f30da&source=-----211eca16505e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F211eca16505e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgit-tags-what-are-they-and-how-to-use-them-211eca16505e&source=-----211eca16505e---------------------bookmark_footer-----------)![](../Images/d199d497c549db03b5f6b8c2fd3966e7.png)

照片由 [Louie Martinez](https://unsplash.com/@thetalkinglens?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在 Git 中，标签是一种标记仓库历史中特定点的方法。它通常用于标记重要的里程碑或发布，例如一个版本、发布或一个重要的项目更新。它们通常有三个主要目的：

1.  **发布版本**：标签通常用于标记软件发布的特定版本。例如，`v1.0` 标签可以用来表示软件的第一个正式版本。

1.  **稳定点**：标签可以标记开发过程中的稳定点。这在你想要突出已知稳定可靠的提交时非常有用，比如在完成要发布的主要功能后，或修复关键漏洞后。

1.  **文档**：标签可以作为文档标记。通过标记项目历史中的重要点，你创建了可以用来理解和分析代码库演变的参考。

## 本地标签

当涉及到本地应用标签时，Git 提供了两种类型的标签：**轻量标签**和**注解标签**。

**轻量标签**：这些是指向特定提交的简单指针。它们被创建…
