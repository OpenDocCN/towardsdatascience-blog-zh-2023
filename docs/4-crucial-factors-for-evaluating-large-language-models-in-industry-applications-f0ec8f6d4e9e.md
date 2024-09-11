# 评估行业应用中大型语言模型的4个关键因素

> 原文：[https://towardsdatascience.com/4-crucial-factors-for-evaluating-large-language-models-in-industry-applications-f0ec8f6d4e9e?source=collection_archive---------8-----------------------#2023-08-01](https://towardsdatascience.com/4-crucial-factors-for-evaluating-large-language-models-in-industry-applications-f0ec8f6d4e9e?source=collection_archive---------8-----------------------#2023-08-01)

## 每个用例都是不同的——这取决于客户需求和行业特定的指南。了解如何使用4个关键标准做出正确的LLM选择。

[](https://skanda-vivek.medium.com/?source=post_page-----f0ec8f6d4e9e--------------------------------)[![Skanda Vivek](../Images/9d25bee2fb75176ca7f7ea6eff7d7ab5.png)](https://skanda-vivek.medium.com/?source=post_page-----f0ec8f6d4e9e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0ec8f6d4e9e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f0ec8f6d4e9e--------------------------------) [**Skanda Vivek**](https://skanda-vivek.medium.com/?source=post_page-----f0ec8f6d4e9e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F220d9bbb8014&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-crucial-factors-for-evaluating-large-language-models-in-industry-applications-f0ec8f6d4e9e&user=Skanda+Vivek&userId=220d9bbb8014&source=post_page-220d9bbb8014----f0ec8f6d4e9e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0ec8f6d4e9e--------------------------------) ·8分钟阅读·2023年8月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff0ec8f6d4e9e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-crucial-factors-for-evaluating-large-language-models-in-industry-applications-f0ec8f6d4e9e&user=Skanda+Vivek&userId=220d9bbb8014&source=-----f0ec8f6d4e9e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff0ec8f6d4e9e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-crucial-factors-for-evaluating-large-language-models-in-industry-applications-f0ec8f6d4e9e&source=-----f0ec8f6d4e9e---------------------bookmark_footer-----------)![](../Images/17667e0f8e19d8ed3fb3aa190bcf63ca.png)

LLM 决策指标 | **Skanda Vivek**

在过去的几个月中，我有机会与法律、医疗、金融、科技、保险行业的人们讨论大型语言模型的采用情况。每个行业都有独特的需求和挑战。例如，在医疗领域，隐私至上。在金融领域，准确的数字至关重要。律师们需要专门的、经过精细调整的模型来执行法律文件的草拟等任务。

在本文中，我将讨论那些能帮助你在特定情况下选择合适模型的关键决策因素。

# 响应质量

正如萨蒂亚·纳德拉在他2023年的[Microsoft Inspire主题演讲](https://www.youtube.com/watch?v=RhwVMt_XCUE&ab_channel=Microsoft)中所述，生成AI引入了两个主要的范式转变：

1.  更自然的语言计算机界面

1.  一个推理引擎，位于所有自定义文档的顶部

在这两种使用情况中，响应质量都至关重要。我们与计算机的界面越来越接近...
