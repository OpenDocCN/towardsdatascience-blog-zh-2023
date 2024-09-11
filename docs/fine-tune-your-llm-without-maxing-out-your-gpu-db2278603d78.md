# 在不超负荷 GPU 的情况下微调你的大型语言模型

> 原文：[`towardsdatascience.com/fine-tune-your-llm-without-maxing-out-your-gpu-db2278603d78?source=collection_archive---------5-----------------------#2023-08-01`](https://towardsdatascience.com/fine-tune-your-llm-without-maxing-out-your-gpu-db2278603d78?source=collection_archive---------5-----------------------#2023-08-01)

## 如何在有限的硬件和紧张的预算下微调你的大型语言模型

[](https://johnadeojo.medium.com/?source=post_page-----db2278603d78--------------------------------)![John Adeojo](https://johnadeojo.medium.com/?source=post_page-----db2278603d78--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db2278603d78--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----db2278603d78--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----db2278603d78--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-your-llm-without-maxing-out-your-gpu-db2278603d78&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----db2278603d78---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----db2278603d78--------------------------------) ·8 min read·2023 年 8 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdb2278603d78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-your-llm-without-maxing-out-your-gpu-db2278603d78&user=John+Adeojo&userId=f933e1637e40&source=-----db2278603d78---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdb2278603d78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffine-tune-your-llm-without-maxing-out-your-gpu-db2278603d78&source=-----db2278603d78---------------------bookmark_footer-----------)![](img/ebaa7adc1f03dc619e66b6d330a31748.png)

作者图片：由 Midjourney 生成

# 定制大型语言模型的需求

随着 ChatGPT 的成功，我们见证了对定制大型语言模型需求的激增。

然而，这种模型的庞大规模也造成了采纳上的障碍。由于这些模型非常大，对于预算有限的企业、研究人员或爱好者来说，定制它们以适应自己的数据集一直是个挑战。

现在，随着参数高效微调（PEFT）方法的创新，完全可以在相对较低的成本下微调大型语言模型。在本文中，我将展示如何在 [Google Colab](https://colab.research.google.com/) 中实现这一目标。

我预期这篇文章对从业者、爱好者、学习者，甚至是动手实践的初创公司创始人都会有很大的价值。

所以，如果你需要制作一个便宜的原型、测试一个想法，或者创建一个酷炫的数据科学项目来脱颖而出——继续阅读。

# 我们为什么要进行微调？

企业通常拥有驱动其某些流程的私有数据集。

举个例子，我曾在一家银行工作，我们将客户投诉记录在一个 Excel 电子表格中。一个……
