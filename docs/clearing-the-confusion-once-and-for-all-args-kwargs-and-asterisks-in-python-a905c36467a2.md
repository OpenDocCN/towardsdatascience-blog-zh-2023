# 一劳永逸地解开疑惑：Python中的`args`、`kwargs`和星号

> 原文：[https://towardsdatascience.com/clearing-the-confusion-once-and-for-all-args-kwargs-and-asterisks-in-python-a905c36467a2?source=collection_archive---------4-----------------------#2023-05-30](https://towardsdatascience.com/clearing-the-confusion-once-and-for-all-args-kwargs-and-asterisks-in-python-a905c36467a2?source=collection_archive---------4-----------------------#2023-05-30)

## 并且幸福快乐地生活下去

[](https://ibexorigin.medium.com/?source=post_page-----a905c36467a2--------------------------------)[![Bex T.](../Images/516496f32596e8ad56bf07f178a643c6.png)](https://ibexorigin.medium.com/?source=post_page-----a905c36467a2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a905c36467a2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a905c36467a2--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----a905c36467a2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclearing-the-confusion-once-and-for-all-args-kwargs-and-asterisks-in-python-a905c36467a2&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----a905c36467a2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a905c36467a2--------------------------------) · 9分钟阅读 · 2023年5月30日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa905c36467a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclearing-the-confusion-once-and-for-all-args-kwargs-and-asterisks-in-python-a905c36467a2&user=Bex+T.&userId=39db050c2ac2&source=-----a905c36467a2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa905c36467a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclearing-the-confusion-once-and-for-all-args-kwargs-and-asterisks-in-python-a905c36467a2&source=-----a905c36467a2---------------------bookmark_footer-----------)![](../Images/49a1db551c8f925a358bd20c9e9f7d04.png)

图片由我使用Midjourney制作

## 动机

每次看到有人在函数中使用`*args`、`**kwargs`或者星号运算符用于除乘法以外的其他目的时，我总是感到烦恼。我的意思是，他们难道不能停下来片刻，使用一些其他人都能理解的东西吗？

但在了解它们的用途之后，我意识到 `*args`、`**kwargs` 和前缀星号的使用源于对灵活性和优雅的追求。尽管这可能感觉像是对[Python 之禅](https://peps.python.org/pep-0020/)中“可读性重要”这句话的一个反击，但它们仍然是美丽代码的强大工具。

因此，在这篇文章中，我打算澄清所有与这些神秘关键字和星号表达式相关的困惑，并展示几乎所有可以使用它们的场景。

让我们开始吧。

## 解开可迭代对象

当星号用于两个变量或 Python 对象之间时，通常是用于乘法或幂运算。但当它用在变量或可迭代对象*之前*时，它就变成了一个完全不同的东西。

> 可迭代对象是你可以迭代的 Python 对象，例如字符串、元组、列表、字典等……
