# 如何自动导入和合并多个 R 文件

> 原文：[`towardsdatascience.com/how-to-automatically-import-and-combine-multiple-files-in-r-30a77c0a7732?source=collection_archive---------6-----------------------#2023-09-20`](https://towardsdatascience.com/how-to-automatically-import-and-combine-multiple-files-in-r-30a77c0a7732?source=collection_archive---------6-----------------------#2023-09-20)

## 不要再浪费时间手动导入多个文件

[](https://alanarister.medium.com/?source=post_page-----30a77c0a7732--------------------------------)![Alana Rister, Ph.D.](https://alanarister.medium.com/?source=post_page-----30a77c0a7732--------------------------------)[](https://towardsdatascience.com/?source=post_page-----30a77c0a7732--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----30a77c0a7732--------------------------------) [Alana Rister, Ph.D.](https://alanarister.medium.com/?source=post_page-----30a77c0a7732--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa2d36236daea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automatically-import-and-combine-multiple-files-in-r-30a77c0a7732&user=Alana+Rister%2C+Ph.D.&userId=a2d36236daea&source=post_page-a2d36236daea----30a77c0a7732---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----30a77c0a7732--------------------------------) ·6 min read·Sep 20, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F30a77c0a7732&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automatically-import-and-combine-multiple-files-in-r-30a77c0a7732&user=Alana+Rister%2C+Ph.D.&userId=a2d36236daea&source=-----30a77c0a7732---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F30a77c0a7732&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-automatically-import-and-combine-multiple-files-in-r-30a77c0a7732&source=-----30a77c0a7732---------------------bookmark_footer-----------)![](img/4faf273a525b691b00abfba4f6517fd3.png)

图片由 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我的数据科学家工作中，由于不同软件中的导出限制，我经常需要导入包含相同类型信息的多个不同文件。如果你也面临类似情况，下面是一种清晰且简单的方法，可以自动将文件导入为单独的数据框或将它们合并为一个数据框。

# 准备你的文件

在我们开始编写代码之前，我们首先必须准备好文件。我们需要一种程序化选择要导入到 R 中的文件的方法。虽然你可以选择任何方法来区分这些文件，但这里有两种最简单的方法：

1.  为所有要一次导入的文件创建一个独特的前缀。

1.  在你的工作目录中创建一个单独的文件夹，并只将这些文件放入该文件夹。

例如，如果我有一组名为“SA#.xlsx”的 Excel 文件。如果我没有其他以 SA 开头的类似文件，那么我已经有了我的前缀。如果文件夹中还有其他以 SA 开头的文件，比如“SAT.xlsx”，我可以很容易地创建一个文件夹，并将其命名为“SA”。然后，我只会包含我想导入的文件作为 SA…
