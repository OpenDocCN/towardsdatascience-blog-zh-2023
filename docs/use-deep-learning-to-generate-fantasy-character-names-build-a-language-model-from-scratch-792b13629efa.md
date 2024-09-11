# 使用深度学习生成幻想名称：从零开始构建语言模型

> 原文：[`towardsdatascience.com/use-deep-learning-to-generate-fantasy-character-names-build-a-language-model-from-scratch-792b13629efa?source=collection_archive---------3-----------------------#2023-09-22`](https://towardsdatascience.com/use-deep-learning-to-generate-fantasy-character-names-build-a-language-model-from-scratch-792b13629efa?source=collection_archive---------3-----------------------#2023-09-22)

## 语言模型能否创造独特的幻想角色名称？让我们从零开始构建它

[](https://medium.com/@riccardo.andreoni?source=post_page-----792b13629efa--------------------------------)![Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----792b13629efa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----792b13629efa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----792b13629efa--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----792b13629efa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-deep-learning-to-generate-fantasy-character-names-build-a-language-model-from-scratch-792b13629efa&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----792b13629efa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----792b13629efa--------------------------------) · 11 分钟阅读 · 2023 年 9 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F792b13629efa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-deep-learning-to-generate-fantasy-character-names-build-a-language-model-from-scratch-792b13629efa&user=Riccardo+Andreoni&userId=76784541161c&source=-----792b13629efa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F792b13629efa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-deep-learning-to-generate-fantasy-character-names-build-a-language-model-from-scratch-792b13629efa&source=-----792b13629efa---------------------bookmark_footer-----------)![](img/f64a5c60ba6b75f99b1f391913f9f8be.png)

来源：[pixabay.com](https://pixabay.com/illustrations/book-old-surreal-fantasy-pages-863418/)

要真正理解[**语言模型**](https://en.wikipedia.org/wiki/Language_model#:~:text=A%20language%20model%20is%20a,feedforward%20neural%20networks%20and%20transformers.)（LM）的复杂性，并熟悉其基本原理，唯一的办法就是卷起袖子，开始编写代码。在这篇文章中，我展示了如何从头开始构建一个[**递归神经网络**](https://en.wikipedia.org/wiki/Recurrent_neural_network)（RNN），而不依赖于任何深度学习库。

Tensorflow、Keras 和 Pytorch 使得构建深度和复杂的神经网络变得轻而易举。毫无疑问，这对于机器学习从业者来说是一个巨大的优势，然而，这种方法也有一个巨大的缺点，即这些网络的功能并不清晰，因为它们在“幕后”进行操作。

这就是为什么今天我们将进行一个激动人心的练习，只使用[Numpy Python](https://numpy.org/)库来构建一个语言模型！

# 理解递归神经网络和语言模型

标准的全连接神经网络不适用于[自然语言处理](https://en.wikipedia.org/wiki/Natural_language_processing)（NLP）任务，例如文本生成。主要原因是：

+   对于 NLP 任务，输入和输出可能会有所不同……
