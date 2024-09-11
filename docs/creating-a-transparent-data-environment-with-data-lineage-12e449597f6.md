# 创建透明数据环境与数据血缘

> 原文：[https://towardsdatascience.com/creating-a-transparent-data-environment-with-data-lineage-12e449597f6?source=collection_archive---------17-----------------------#2023-04-11](https://towardsdatascience.com/creating-a-transparent-data-environment-with-data-lineage-12e449597f6?source=collection_archive---------17-----------------------#2023-04-11)

## 跨越整个技术栈的列级血缘的好处

[](https://madison-schott.medium.com/?source=post_page-----12e449597f6--------------------------------)[![Madison Schott](../Images/0b82d0dd48629641abb439cef23ebe04.png)](https://madison-schott.medium.com/?source=post_page-----12e449597f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----12e449597f6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----12e449597f6--------------------------------) [Madison Schott](https://madison-schott.medium.com/?source=post_page-----12e449597f6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3ed0ce2dcf93&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-transparent-data-environment-with-data-lineage-12e449597f6&user=Madison+Schott&userId=3ed0ce2dcf93&source=post_page-3ed0ce2dcf93----12e449597f6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----12e449597f6--------------------------------) ·5分钟阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F12e449597f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-transparent-data-environment-with-data-lineage-12e449597f6&user=Madison+Schott&userId=3ed0ce2dcf93&source=-----12e449597f6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F12e449597f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-a-transparent-data-environment-with-data-lineage-12e449597f6&source=-----12e449597f6---------------------bookmark_footer-----------)![](../Images/36bd79f779c717a98d9e4ec2b4b15c40.png)

图片由 [Bozhin Karaivanov](https://unsplash.com/@bkaraivanov?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/string?orientation=landscape&utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当我编写数据转换时，我经常注意到其中的不一致性。也许数据源中的某一列不遵循我设定的命名约定，或者时间戳没有转换为正确的数据类型。我不等待在后续中修复这些问题，而是立即进行更改。

然而，第二天，我注意到生产数据管道出现了故障。由于我昨天进行的那个“简单”的名称更改，几个下游数据模型出现了问题。现在，我需要急忙更改所有下游数据模型中对该列的引用。

不幸的是，如果没有一个能显示数据流向的工具，这种情况可能相当常见。你会被迫采取被动应对，等到出现故障时才能知道需要做出哪些更改。幸运的是，数据血缘工具可以帮助我们避免这种情况，并对依赖关系采取主动态度。

# 数据血缘是什么？

[数据血缘](https://www.y42.com/blog/data-lineage/?utm_source=medium&utm_medium=blog&utm_campaign=lineage_transparency_article) 显示了数据在管道中流动的路径。它突出了数据的来源、如何转换以及包含在什么可视化中。
