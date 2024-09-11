# 如何在发票上训练 LILT 模型并进行推断

> 原文：[`towardsdatascience.com/how-to-train-the-lilt-model-on-invoices-and-run-inference-8fd6b3cfae1b?source=collection_archive---------7-----------------------#2023-01-03`](https://towardsdatascience.com/how-to-train-the-lilt-model-on-invoices-and-run-inference-8fd6b3cfae1b?source=collection_archive---------7-----------------------#2023-01-03)

## 步骤教程

[](https://walidamamou.medium.com/?source=post_page-----8fd6b3cfae1b--------------------------------)![Walid Amamou](https://walidamamou.medium.com/?source=post_page-----8fd6b3cfae1b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8fd6b3cfae1b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8fd6b3cfae1b--------------------------------) [Walid Amamou](https://walidamamou.medium.com/?source=post_page-----8fd6b3cfae1b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F706f7e2641d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-train-the-lilt-model-on-invoices-and-run-inference-8fd6b3cfae1b&user=Walid+Amamou&userId=706f7e2641d7&source=post_page-706f7e2641d7----8fd6b3cfae1b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8fd6b3cfae1b--------------------------------) · 5 分钟阅读 · 2023 年 1 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8fd6b3cfae1b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-train-the-lilt-model-on-invoices-and-run-inference-8fd6b3cfae1b&user=Walid+Amamou&userId=706f7e2641d7&source=-----8fd6b3cfae1b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8fd6b3cfae1b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-train-the-lilt-model-on-invoices-and-run-inference-8fd6b3cfae1b&source=-----8fd6b3cfae1b---------------------bookmark_footer-----------)![](img/f78a852d92471c06d30e5522584f4505.png)

图片由 **[Zinkevych_D](https://elements.envato.com/user/Zinkevych_D)** 提供，来自 [Envanto](https://elements.envato.com/close-up-of-an-invoice-being-signed-HL7W2QD)

在文档理解领域，深度学习模型发挥了重要作用。这些模型能够准确解释文档的内容和结构，使它们成为发票处理、简历解析和合同分析等任务的宝贵工具。深度学习模型在文档理解中的另一个重要优势是它们能够随着时间的推移不断学习和适应。当遇到新的文档类型时，这些模型可以继续学习并提高其性能，使它们在文档分类和信息提取等任务中高度可扩展且高效。

其中一个模型是 LILT 模型（语言无关布局变换器），这是一个为文档布局分析任务开发的深度学习模型。与其前身 layoutLM 不同，LILT 最初设计为语言无关，这意味着它可以分析任何语言的文档，同时在许多下游任务应用中相比其他现有模型取得更好的性能。此外，该模型具有 MIT 许可证，这意味着它可以用于商业用途，而不像最新的 layoutLM v3 和 layoutXLM。因此，创建一个关于如何微调该模型的教程是值得的，因为它有潜力被广泛用于各种…
