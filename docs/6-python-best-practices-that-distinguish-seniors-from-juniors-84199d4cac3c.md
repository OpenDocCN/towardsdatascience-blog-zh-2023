# 区分高级开发者与初级开发者的6个Python最佳实践

> 原文：[https://towardsdatascience.com/6-python-best-practices-that-distinguish-seniors-from-juniors-84199d4cac3c?source=collection_archive---------0-----------------------#2023-04-18](https://towardsdatascience.com/6-python-best-practices-that-distinguish-seniors-from-juniors-84199d4cac3c?source=collection_archive---------0-----------------------#2023-04-18)

## 如何编写被认为来自经验丰富开发者的 Python 代码

[![Tomer Gabay](../Images/1fb1d408bc89415918c1aa6733df44e1.png)](https://medium.com/@tomergabay?source=post_page-----84199d4cac3c--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----84199d4cac3c--------------------------------) [Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----84199d4cac3c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc9c352dba00a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-python-best-practices-that-distinguish-seniors-from-juniors-84199d4cac3c&user=Tomer+Gabay&userId=c9c352dba00a&source=post_page-c9c352dba00a----84199d4cac3c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----84199d4cac3c--------------------------------) ·12 分钟阅读·2023年4月18日

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F84199d4cac3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-python-best-practices-that-distinguish-seniors-from-juniors-84199d4cac3c&source=-----84199d4cac3c---------------------bookmark_footer-----------)![](../Images/3dd47a21048817c7f706cbf2338c9456.png)

图片来源：[Desola Lanre-Ologun](https://unsplash.com/@disruptxn?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

2023年1月，我发表了[一篇关于区分高级开发者和初级开发者的5个Python技巧的文章](https://medium.com/towards-data-science/5-python-tricks-that-distinguish-senior-developers-from-juniors-826d57ab3940)。在这篇文章中，我们不是看‘技巧’，而是看6个Python最佳实践，可以区分经验丰富的开发者和初学者。通过各种例子，我们将探讨高级开发者编写的代码与初级开发者编写的代码之间的区别。

通过学习这些最佳实践，你可以编写不仅被视为由高级开发者创建的代码，而且实际上质量更高。当你例如向同事或在面试中展示你的代码时，这两种品质都会非常有优势。

# 1\. 使用正确的可迭代类型

> 可迭代对象是任何能够逐个返回其成员的Python对象，使其能够在for循环中进行迭代。([来源](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Iterables.html#:~:text=An%20iterable%20is%20any%20Python,over%20in%20a%20for%2Dloop.))

初级开发者在需要可迭代对象时往往会使用列表。然而，在Python中，不同类型的可迭代对象有不同的用途。为了总结最重要的可迭代对象：
