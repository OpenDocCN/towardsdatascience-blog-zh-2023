# 在 Kubernetes 上运行代码的智能、灵活的方法

> 原文：[`towardsdatascience.com/the-smart-flexible-way-to-run-code-on-kubernetes-94d1ae6c46f3?source=collection_archive---------19-----------------------#2023-01-23`](https://towardsdatascience.com/the-smart-flexible-way-to-run-code-on-kubernetes-94d1ae6c46f3?source=collection_archive---------19-----------------------#2023-01-23)

[](https://pascaljanetzky.medium.com/?source=post_page-----94d1ae6c46f3--------------------------------)![Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----94d1ae6c46f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94d1ae6c46f3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----94d1ae6c46f3--------------------------------) [Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----94d1ae6c46f3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F672b95fdf976&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-smart-flexible-way-to-run-code-on-kubernetes-94d1ae6c46f3&user=Pascal+Janetzky&userId=672b95fdf976&source=post_page-672b95fdf976----94d1ae6c46f3---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----94d1ae6c46f3--------------------------------) ·6 min read·2023 年 1 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F94d1ae6c46f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-smart-flexible-way-to-run-code-on-kubernetes-94d1ae6c46f3&user=Pascal+Janetzky&userId=672b95fdf976&source=-----94d1ae6c46f3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F94d1ae6c46f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-smart-flexible-way-to-run-code-on-kubernetes-94d1ae6c46f3&source=-----94d1ae6c46f3---------------------bookmark_footer-----------)

当我还是一个 Kubernetes 初学者时，我主要关注的是如何在集群上运行代码。进入一个全新的世界，我看到这些令人困惑的 YAML 文件，每一行和缩进都带来新的含义。

![](img/77a4045112438f4e57e21a6fb9839dc5.png)

图片由[Thais Morais](https://unsplash.com/@tata_morais?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

一旦我学会了将代码快速写入文件的方法，我很快就用绝对路径填满了它。你可以看到下面的截断示例，我称之为*初学者的方法*。请注意，这是一种完全有效的运行代码的方法，只是缺少了即将介绍的功能。

## 快速且容易出错的方法：代码目录的完整路径

让我们深入了解一下 .yaml 文件，并突出显示与将代码（和数据）放入容器中最相关的部分。简单来说，容器仅仅是一个包含相关内容的盒子。这个盒子由 Pod 使用，我们可以将其视为一个虚拟计算机。这个虚拟计算机帮助我们运行容器中的内容。

（如果你对容器化更为熟悉，你可能会注意到这个描述是一种简化。a）…
