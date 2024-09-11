# PyScript.com：云中的 PyScript IDE

> 原文：[https://towardsdatascience.com/pyscript-com-a-pyscript-ide-in-the-cloud-2b5bde6f0342?source=collection_archive---------10-----------------------#2023-04-13](https://towardsdatascience.com/pyscript-com-a-pyscript-ide-in-the-cloud-2b5bde6f0342?source=collection_archive---------10-----------------------#2023-04-13)

## PyScript.com 是 Anaconda 推出的一个新的在线 IDE，可以让你创建、运行和托管 PyScript 应用

[](https://medium.com/@alan-jones?source=post_page-----2b5bde6f0342--------------------------------)[![Alan Jones](../Images/359379fab1d6685ff08080b98173e67c.png)](https://medium.com/@alan-jones?source=post_page-----2b5bde6f0342--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2b5bde6f0342--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2b5bde6f0342--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----2b5bde6f0342--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d3f5fb94faa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpyscript-com-a-pyscript-ide-in-the-cloud-2b5bde6f0342&user=Alan+Jones&userId=7d3f5fb94faa&source=post_page-7d3f5fb94faa----2b5bde6f0342---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2b5bde6f0342--------------------------------) · 12 分钟阅读 · 2023年4月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2b5bde6f0342&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpyscript-com-a-pyscript-ide-in-the-cloud-2b5bde6f0342&user=Alan+Jones&userId=7d3f5fb94faa&source=-----2b5bde6f0342---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2b5bde6f0342&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpyscript-com-a-pyscript-ide-in-the-cloud-2b5bde6f0342&source=-----2b5bde6f0342---------------------bookmark_footer-----------)![](../Images/07925010c17760f2045486aee7a5dd6b.png)

哇！如果他需要那么大的屏幕，他一定是一个非常认真的重要程序员 —— 我想知道为什么它们大多数都是空白的。照片由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*注意：2023年末发布了一个全新的重写版本的 PyScript，这可能使得这里描述的一些语法过时 —— 请参见* [PyScript 正在成长](https://medium.com/codefile/pyscript-is-growing-up-9df5c51fdf51) *以获取更新。*

嗯，这是个好消息！目前尚不清楚现有的IDE或编辑器哪一个适合构建PyScript应用程序，但现在有了PyScript.com，我们拥有了一个专用的在线IDE。

它到底有多好呢？我们将会揭晓。

我们将会看看Anaconda推出的新PyScript在线IDE：我会介绍这个新平台，看看如何开始使用它编写PyScript应用程序，并最终完成一个功能齐全并已部署的PyScript应用。

## PyScript.com

Anaconda Inc. 对其新产品的评价毫不掩饰。

> *“这个革命性的平台使99%的人都能编程，推进了Anaconda的使命，即普及数据科学和Python开发。” — Anaconda Inc.*
