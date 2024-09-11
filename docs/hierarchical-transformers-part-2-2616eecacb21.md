# 层次化变换器 — 第二部分

> 原文：[https://towardsdatascience.com/hierarchical-transformers-part-2-2616eecacb21?source=collection_archive---------14-----------------------#2023-10-07](https://towardsdatascience.com/hierarchical-transformers-part-2-2616eecacb21?source=collection_archive---------14-----------------------#2023-10-07)

## 层次化注意力更快

[](https://medium.com/@mina.ghashami?source=post_page-----2616eecacb21--------------------------------)[![Mina Ghashami](../Images/745f53b94f5667a485299b49913c7a21.png)](https://medium.com/@mina.ghashami?source=post_page-----2616eecacb21--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2616eecacb21--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2616eecacb21--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----2616eecacb21--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhierarchical-transformers-part-2-2616eecacb21&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----2616eecacb21---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2616eecacb21--------------------------------) ·6分钟阅读·2023年10月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2616eecacb21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhierarchical-transformers-part-2-2616eecacb21&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----2616eecacb21---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2616eecacb21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhierarchical-transformers-part-2-2616eecacb21&source=-----2616eecacb21---------------------bookmark_footer-----------)

这篇文章要求你具备标准变换器及其工作原理的知识。如果你是初学者，想了解变换器，请查看[初学者变换器](https://medium.com/p/4deaf9b199f9/edit)文章。

在[层次化变换器 — 第一部分](https://medium.com/towards-data-science/hierarchical-transformers-54f6d59fa8fc)中，我们定义了“层次化变换器”的含义，并回顾了该领域的一项重要工作，即*Hourglass*。

在本文中，我们将继续探讨另一个著名工作，即*层次化注意力变换器（HAT）*。

让我们开始吧。

# 层次化注意力变换器（HAT）

该方法最初是为了对长文档进行分类而提出的，这些文档通常长达数千字。一个应用实例是对法律文档或生物医学文档进行分类，这些文档通常非常长。

## 标记化和分割

**HAT** 方法通过处理输入文档，并使用字节对编码（BPE）标记器将文本拆分为子词/标记来工作。这个标记器被许多著名的大型语言模型使用，如 BERT、RoBERTA 和 GPT 系列。

然后，它将标记化后的文档拆分成 *N* 个大小相等的块；即，如果 *S* 表示输入文档，则 *S = [C1, …., CN]* 是 *N* 个大小相等的块。 （在本文中，我们…
