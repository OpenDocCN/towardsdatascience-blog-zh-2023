# 你不能两次踏入同一条河流

> 原文：[`towardsdatascience.com/you-cant-step-in-the-same-river-twice-cfacf7cee305?source=collection_archive---------7-----------------------#2023-12-20`](https://towardsdatascience.com/you-cant-step-in-the-same-river-twice-cfacf7cee305?source=collection_archive---------7-----------------------#2023-12-20)

![](img/5a6342f9f1812f624416fcc0889cb8bd.png)

图片来源：[Vladislav Babienko](https://unsplash.com/@garri?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 《为何之书》第七章和第八章，《跟我读》系列

[](https://zzhu17.medium.com/?source=post_page-----cfacf7cee305--------------------------------)![Zijing Zhu, PhD](https://zzhu17.medium.com/?source=post_page-----cfacf7cee305--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cfacf7cee305--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cfacf7cee305--------------------------------) [Zijing Zhu, PhD](https://zzhu17.medium.com/?source=post_page-----cfacf7cee305--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d83c09fb5d4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyou-cant-step-in-the-same-river-twice-cfacf7cee305&user=Zijing+Zhu%2C+PhD&userId=7d83c09fb5d4&source=post_page-7d83c09fb5d4----cfacf7cee305---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cfacf7cee305--------------------------------) ·18 分钟阅读·2023 年 12 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcfacf7cee305&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyou-cant-step-in-the-same-river-twice-cfacf7cee305&user=Zijing+Zhu%2C+PhD&userId=7d83c09fb5d4&source=-----cfacf7cee305---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcfacf7cee305&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyou-cant-step-in-the-same-river-twice-cfacf7cee305&source=-----cfacf7cee305---------------------bookmark_footer-----------)

在我的 [上一篇文章](https://zzhu17.medium.com/list/causality-book-club-def5f1fd00fe) 中，我们探讨了观察数据中的混淆因素和碰撞体，这些因素阻碍了可靠因果关系的建立。Pearl 提供的解决方案是绘制因果图，并使用 [反向准则](https://medium.com/towards-data-science/causal-diagram-confronting-the-achilles-heel-in-observational-data-a69cdb1c4818) 找出需要阻断的混淆因素集合，同时保留碰撞体和中介变量。

然而，在处理那些无法观察或测量的混杂变量时，从观察数据中估计因果关系变得困难。为应对这一问题，在《为什么的书》第七章中，**朱迪亚·珀尔**介绍了**do-calculus 规则**。这些规则对于**前门准则**和**工具变量**特别有用。即使存在不可观察的混杂变量，它们也可以用来建立因果关系。

在第八章中，我们将探索**反事实**的奇妙世界。开篇引用了诗人罗伯特·弗罗斯特的名句：

> “对不起，我不能同时旅行两个方向”
> 
> “成为一个旅行者，我站了很久……”

珀尔指出，尽管不可能同时走两条路或踏入同一条河流两次，我们的大脑可以想象如果我们走了另一条路会发生什么。在……
