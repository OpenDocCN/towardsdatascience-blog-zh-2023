# 使用sysargv、argparse、docopts和Typer的命令行接口

> 原文：[https://towardsdatascience.com/command-line-interface-with-sysargv-argparse-docopts-and-typer-e876f577a5d6?source=collection_archive---------7-----------------------#2023-11-24](https://towardsdatascience.com/command-line-interface-with-sysargv-argparse-docopts-and-typer-e876f577a5d6?source=collection_archive---------7-----------------------#2023-11-24)

## 4种将参数传递给你的Python脚本的方法

[](https://kayjanwong.medium.com/?source=post_page-----e876f577a5d6--------------------------------)[![Kay Jan Wong](../Images/28e803eca6327d97b6aa97ee4095d7bd.png)](https://kayjanwong.medium.com/?source=post_page-----e876f577a5d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e876f577a5d6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e876f577a5d6--------------------------------) [Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----e876f577a5d6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffee8693930fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcommand-line-interface-with-sysargv-argparse-docopts-and-typer-e876f577a5d6&user=Kay+Jan+Wong&userId=fee8693930fb&source=post_page-fee8693930fb----e876f577a5d6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e876f577a5d6--------------------------------) ·9分钟阅读·2023年11月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe876f577a5d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcommand-line-interface-with-sysargv-argparse-docopts-and-typer-e876f577a5d6&user=Kay+Jan+Wong&userId=fee8693930fb&source=-----e876f577a5d6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe876f577a5d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcommand-line-interface-with-sysargv-argparse-docopts-and-typer-e876f577a5d6&source=-----e876f577a5d6---------------------bookmark_footer-----------)![](../Images/25677cbf2ad7407485c144f23c0c8104.png)

图片由 [Florian Olivo](https://unsplash.com/@florianolv?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

要部署一个管道，通常需要一个*主*脚本，或一个单一的入口点来运行整个管道。例如，在数据科学管道中，代码仓库的入口点应该协调并顺序运行数据、特征工程、建模和评估管道。

> 有时，你可能需要运行不同类型的管道或对管道进行临时调整。

调整可能包括省略代码的某些部分或使用不同的参数值运行管道。在数据科学中，可能会有一个训练和评分管道，或者某些运行需要完全或部分刷新数据。

**平凡的解决方案是创建多个主脚本**。然而，这将导致代码重复，并且长时间维护多个脚本会很困难——考虑到可能有许多调整组合。**更好的解决方案是让主脚本接受参数**，以值或标志的形式，并随后通过命令行界面（CLI）运行适当类型的管道。
