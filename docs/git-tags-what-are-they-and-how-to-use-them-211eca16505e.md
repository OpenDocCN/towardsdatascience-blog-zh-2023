# Git 标签：它们是什么以及如何使用

> 原文：[`towardsdatascience.com/git-tags-what-are-they-and-how-to-use-them-211eca16505e`](https://towardsdatascience.com/git-tags-what-are-they-and-how-to-use-them-211eca16505e)

## 它们的用途是什么，如何创建本地标签，它们如何显示以及如何删除它们

[](https://philip-wilkinson.medium.com/?source=post_page-----211eca16505e--------------------------------)![Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----211eca16505e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----211eca16505e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----211eca16505e--------------------------------) [Philip Wilkinson, Ph.D.](https://philip-wilkinson.medium.com/?source=post_page-----211eca16505e--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----211eca16505e--------------------------------) ·阅读时间 4 分钟·2023 年 7 月 4 日

--

![](img/d199d497c549db03b5f6b8c2fd3966e7.png)

图片由 [Louie Martinez](https://unsplash.com/@thetalkinglens?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在 Git 中，标签是一种标记版本库历史中特定点的方式。它通常用于标记重要的里程碑或发布，如版本、发布或主要项目更新。它们通常有三个主要目的：

1.  **发布版本**：标签通常用于标记特定版本的软件发布。例如，`v1.0` 标签可以用来表示你软件的第一个正式版本。

1.  **稳定点**：标签可以标记开发过程中的稳定点。当你希望突出标记已知稳定且可靠的提交时，这很有用，例如完成一个主要功能准备发布后，或修复关键错误后。

1.  **文档**：标签可以作为文档标记。通过标记项目历史中的重要点，你可以创建用于理解和分析代码库演变的参考。

## 本地标签

当涉及到本地应用标签时，Git 提供了两种类型的标签：**轻量标签** 和 **注释标签**。

**轻量标签**：这些是指向特定提交的简单指针。它们是通过 `git tag` 命令加上标签名称来创建的。例如：

```py
git tag v1.0
```

这在当前提交处创建了一个轻量标签名称“v1.0”。轻量标签易于创建，不存储任何额外的信息，如创建标签的用户的姓名或当前日期。

**注释标签**：注释标签存储额外的信息，如标签创建者的姓名、电子邮件、日期和消息。它们是通过 `-a` 或 `— annotate` 选项与 `git tag` 命令一起创建的。例如：

```py
$ git tag -a v1.0 -m "Release version 1.0"
```

这个命令创建了一个名为“v1.0”的注释标签，并附上了提供的消息。注释标签通常用于记录发布或重要里程碑，因为它们提供了更多的背景和信息。

轻量标签和注释标签都是在本地创建的，可以用于个人参考或与项目中的其他开发者共享。

## 推送标签

当你将提交推送到远程仓库时，标签不会被自动推送。相反，你可以通过在`git push`命令后指定标签的名称，单独或批量推送标签：

```py
git push origin V1.0 V1.1 V1.2
```

使用`--tags`标志将所有本地仓库的标签推送到远程仓库：

```py
git push origin --tags
```

或使用`--follow-tags`选项将标志附加到当前分支中的相关提交上：

```py
git push --follow-tags
```

## 标签可见性

当你将标签推送到远程仓库时，它对克隆或从该仓库获取的其他人可见。然而，默认情况下，标签不会在进行`git clone`或`git fetch`操作时自动获取。要从远程仓库检索标签，用户需要明确指定他们想要获取的标签，或者使用`git fetch — tags`命令获取所有标签。

## 删除标签

如果你已将标签推送到远程仓库，并且后来决定删除它，你可以在本地和远程仓库中删除该标签。要删除本地标签，你可以使用以下命令：

```py
git tag -d <tag-name>
```

删除远程标签时，你可以使用以下命令：

```py
git push --delete <remote-name> <tag-name
```

## 总结

在 Git 中，标签对于标记项目历史中的重要点、记录发布和提供重要里程碑的清晰参考非常有价值。了解如何创建、管理和删除标签可以提升你的 Git 工作流和与其他开发者的协作。

如果你喜欢你所阅读的内容且尚未成为 Medium 会员，请随时通过下面的推荐链接注册 Medium，支持我和平台上的其他优秀作者！提前感谢。

[## 使用我的推荐链接加入 Medium — Philip Wilkinson](https://philip-wilkinson.medium.com/membership?source=post_page-----211eca16505e--------------------------------)

### 作为 Medium 会员，你的会员费用的一部分会分配给你阅读的作者，你可以完全访问每一个故事…

[philip-wilkinson.medium.com](https://philip-wilkinson.medium.com/membership?source=post_page-----211eca16505e--------------------------------)

或者可以随时查看我在 Medium 上的其他文章：

[](/eight-data-structures-every-data-scientist-should-know-d178159df252?source=post_page-----211eca16505e--------------------------------) ## 每个数据科学家都应该知道的八种数据结构

### 从基本数据结构到 Python 中的抽象数据类型

[towardsdatascience.com [](/a-complete-data-science-curriculum-for-beginners-825a39915b54?source=post_page-----211eca16505e--------------------------------) ## 完整的数据科学入门课程

### UCL 数据科学学会: Python 入门，数据科学家工具包，使用 Python 的数据科学

[towardsdatascience.com [](https://python.plainenglish.io/a-practical-introduction-to-random-forest-classifiers-from-scikit-learn-536e305d8d87?source=post_page-----211eca16505e--------------------------------) [## 来自 scikit-learn 的随机森林分类器实用介绍

### UCL 数据科学学会研讨会 14: 随机森林分类器是什么，实施，评估和改进

[python.plainenglish.io](https://python.plainenglish.io/a-practical-introduction-to-random-forest-classifiers-from-scikit-learn-536e305d8d87?source=post_page-----211eca16505e--------------------------------)
