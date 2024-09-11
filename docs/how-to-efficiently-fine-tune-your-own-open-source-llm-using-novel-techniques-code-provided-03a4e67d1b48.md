# 使用最新技术微调您自己的开源 LLM

> 原文：[`towardsdatascience.com/how-to-efficiently-fine-tune-your-own-open-source-llm-using-novel-techniques-code-provided-03a4e67d1b48?source=collection_archive---------4-----------------------#2023-12-15`](https://towardsdatascience.com/how-to-efficiently-fine-tune-your-own-open-source-llm-using-novel-techniques-code-provided-03a4e67d1b48?source=collection_archive---------4-----------------------#2023-12-15)

## 在这篇文章中，我微调了一个基础 LLama2 LLM 以输出 SQL 代码。我使用了**参数高效微调**技术来优化这一过程。

[](https://medium.com/@christopher_karg?source=post_page-----03a4e67d1b48--------------------------------)![克里斯托弗·卡格](https://medium.com/@christopher_karg?source=post_page-----03a4e67d1b48--------------------------------)[](https://towardsdatascience.com/?source=post_page-----03a4e67d1b48--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----03a4e67d1b48--------------------------------) [克里斯托弗·卡格](https://medium.com/@christopher_karg?source=post_page-----03a4e67d1b48--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5fbda6d16c39&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-efficiently-fine-tune-your-own-open-source-llm-using-novel-techniques-code-provided-03a4e67d1b48&user=Christopher+Karg&userId=5fbda6d16c39&source=post_page-5fbda6d16c39----03a4e67d1b48---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----03a4e67d1b48--------------------------------) · 13 分钟阅读 · 2023 年 12 月 15 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F03a4e67d1b48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-efficiently-fine-tune-your-own-open-source-llm-using-novel-techniques-code-provided-03a4e67d1b48&user=Christopher+Karg&userId=5fbda6d16c39&source=-----03a4e67d1b48---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F03a4e67d1b48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-efficiently-fine-tune-your-own-open-source-llm-using-novel-techniques-code-provided-03a4e67d1b48&source=-----03a4e67d1b48---------------------bookmark_footer-----------)![](img/389987bb764f9aceba43b821ecf0128e.png)

来源: [`www.pexels.com/photo/calm-body-of-lake-between-mountains-346529/`](https://www.pexels.com/photo/calm-body-of-lake-between-mountains-346529/)

在 [上一篇文章](https://medium.com/towards-data-science/quantisation-and-co-reducing-inference-times-on-llms-by-80-671db9349bdb) 中，我开始说明为什么你可能会考虑训练自己的 LLM。我还简要介绍了硬件要求以及优化训练和推理的方法。在这篇文章中，我将详细讲解如何微调开源 LLM，并提供代码片段供你跟随和复现结果。我们将调优一个 Llama2–7B 模型，以根据自然语言输入提供 SQL 输出——换句话说，模型将把我们用自然语言提问的问题：

*“有多少顾客在十一月决定购买鸡蛋？”*

转换为一个获取相应结果的 SQL 查询：

```py
SELECT COUNT(DISTINCT customer_id) AS num_customers
FROM purchases
WHERE product_name = 'eggs'
AND EXTRACT(MONTH FROM purchase_date) = 11;
```

在每种情况下，数据库 (DB) 的模式将作为 LLM 工作的上下文提供。

```py
CREATE TABLE purchases (
    purchase_id INT PRIMARY KEY,
    customer_id INT…
```
