# 使用 GradientTape 进行 TensorFlow 模型训练

> 原文：[`towardsdatascience.com/tensorflow-model-training-using-gradienttape-f2093646ab13?source=collection_archive---------10-----------------------#2023-10-17`](https://towardsdatascience.com/tensorflow-model-training-using-gradienttape-f2093646ab13?source=collection_archive---------10-----------------------#2023-10-17)

![](img/fbe16d8537174389bce863e509126468.png)

照片由 [Sivani Bandaru](https://unsplash.com/@agni11?utm_source=medium&utm_medium=referral) 提供，拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 使用 GradientTape 更新权重

[](https://rashida00.medium.com/?source=post_page-----f2093646ab13--------------------------------)![Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----f2093646ab13--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f2093646ab13--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2093646ab13--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----f2093646ab13--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a36b941a136&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftensorflow-model-training-using-gradienttape-f2093646ab13&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=post_page-8a36b941a136----f2093646ab13---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2093646ab13--------------------------------) ·7 分钟阅读·2023 年 10 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff2093646ab13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftensorflow-model-training-using-gradienttape-f2093646ab13&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=-----f2093646ab13---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff2093646ab13&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftensorflow-model-training-using-gradienttape-f2093646ab13&source=-----f2093646ab13---------------------bookmark_footer-----------)

TensorFlow 可以说是最受欢迎的深度学习库。我之前写了很多关于 TensorFlow 的教程，并且还在继续。TensorFlow 是一个非常结构化且易于使用的包，你无需过多担心模型开发和训练。几乎大部分的工作都由这个包自己处理。这可能就是它在业界如此受欢迎的原因。但与此同时，有时能够控制幕后功能也是不错的，这能给你带来很多实验模型的动力。如果你是求职者，一些额外的知识可能会让你更具竞争力。

之前，我写了一篇关于如何开发自定义激活函数、层和损失函数的文章。在这篇文章中，我们将看到如何手动训练模型并自己更新权重。但不用担心，你不必重新记忆微积分。我们在 TensorFlow 中有 GradientTape() 方法来处理这部分。

如果 GradientTape() 对你来说完全陌生，请随时查看这个关于 GradientTape() 的练习，它展示了 GradientTape() 是如何工作的：[TensorFlow 中的 GradientTape 介绍 — Regenerative (regenerativetoday.com)](https://regenerativetoday.com/introduction-to-gradienttape-in-tensorflow/)
