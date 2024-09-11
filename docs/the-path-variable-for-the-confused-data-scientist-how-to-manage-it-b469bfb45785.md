# 混乱的数据科学家的 PATH 变量：如何管理它

> 原文：[`towardsdatascience.com/the-path-variable-for-the-confused-data-scientist-how-to-manage-it-b469bfb45785?source=collection_archive---------9-----------------------#2023-06-26`](https://towardsdatascience.com/the-path-variable-for-the-confused-data-scientist-how-to-manage-it-b469bfb45785?source=collection_archive---------9-----------------------#2023-06-26)

## 了解 PATH 的作用以及如何在 Windows 和类 Unix 系统中添加路径

[](https://ibexorigin.medium.com/?source=post_page-----b469bfb45785--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----b469bfb45785--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b469bfb45785--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b469bfb45785--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----b469bfb45785--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-path-variable-for-the-confused-data-scientist-how-to-manage-it-b469bfb45785&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----b469bfb45785---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b469bfb45785--------------------------------) ·6 分钟阅读·2023 年 6 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb469bfb45785&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-path-variable-for-the-confused-data-scientist-how-to-manage-it-b469bfb45785&user=Bex+T.&userId=39db050c2ac2&source=-----b469bfb45785---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb469bfb45785&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-path-variable-for-the-confused-data-scientist-how-to-manage-it-b469bfb45785&source=-----b469bfb45785---------------------bookmark_footer-----------)![](img/a9244c84c570bd3ac2c3898d6baa89aa.png)

图片由我与 Midjourney 制作

有时候我觉得 StackOverflow 上的人就像头上粘着指南针一样。他们总能找到 PATH。

我觉得它不在你的 PATH 中。

你可能弄乱了你的 PATH。

你是否将它添加到你的 PATH 中？

检查可执行文件是否在你的 PATH 中。

“我的意思是，究竟什么是 PATH？”每当我读到这种句子时，总是这样说，脸红着尝试修复错误。现在，在我的数据科学旅程中已经过去三年，我完全知道它是什么了。几乎。

在本文中，我打算教你如何在 Windows 和类 Unix 系统中管理这个令人困惑的环境变量。

让我们开始吧！

## 命令也有路径

你使用最多的终端命令是什么？毫无疑问，我的是`git`，因为我在写文章时经常进行提交。

我之所以问这个，是因为大多数终端命令在操作系统中也有自己的路径。要找到那个路径，你只需…
