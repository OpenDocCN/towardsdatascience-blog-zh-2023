# 使用 Python 微调大型语言模型

> 原文：[https://towardsdatascience.com/fine-tune-a-large-language-model-with-python-b1c09dbc58b2?source=collection_archive---------6-----------------------#2023-04-18](https://towardsdatascience.com/fine-tune-a-large-language-model-with-python-b1c09dbc58b2?source=collection_archive---------6-----------------------#2023-04-18)

![](../Images/1d8727d8fba25a5fd2ed429d267bf0c1.png)

照片由 [Manouchehr Hejazi](https://unsplash.com/@patrol?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 学习如何从头开始在自定义数据集上微调 BERT

[](https://medium.com/@marcellopoliti?source=post_page-----b1c09dbc58b2--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----b1c09dbc58b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b1c09dbc58b2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b1c09dbc58b2--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----b1c09dbc58b2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-a-large-language-model-with-python-b1c09dbc58b2&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----b1c09dbc58b2---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1c09dbc58b2--------------------------------) ·4 分钟阅读·2023年4月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb1c09dbc58b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-a-large-language-model-with-python-b1c09dbc58b2&user=Marcello+Politi&userId=7390355d40fe&source=-----b1c09dbc58b2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb1c09dbc58b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-a-large-language-model-with-python-b1c09dbc58b2&source=-----b1c09dbc58b2---------------------bookmark_footer-----------)

在这篇文章中，我们将探讨使用 PyTorch 对 **BERT 进行情感分类的微调**。BERT 是一种大型语言模型，在流行度和模型大小之间提供了良好的平衡，可以 **使用简单的 GPU** 进行微调。我们可以从 Hugging Face (HF) 下载 **预训练的 BERT**，因此不需要从头开始训练。特别地，我们将使用 BERT 的精简版，即 **Distil-BERT**。

[Distil-BERT](https://huggingface.co/docs/transformers/model_doc/distilbert#:~:text=DistilBERT%20is%20a%20small%2C%20fast,the%20GLUE%20language%20understanding%20benchmark.) 在生产中被广泛使用，因为它比 BERT uncased **减少了 40% 的参数**。它运行 **快 60% 并在 [GLUE](https://arxiv.org/abs/1804.07461) 语言理解基准中保留了 95% 的性能**。

我们首先安装所有必要的库。第一行代码用于捕获安装的输出，以保持你的笔记本整洁。

我将使用 Deepnote 来运行本文中的代码，但如果你愿意，也可以使用 Google Colab。
