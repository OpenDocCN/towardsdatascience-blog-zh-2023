# ChatGPT 能写出比数据分析师更好的 SQL 吗？

> 原文：[https://towardsdatascience.com/can-chatgpt-write-better-sql-than-a-data-analyst-f079518efab2?source=collection_archive---------0-----------------------#2023-01-05](https://towardsdatascience.com/can-chatgpt-write-better-sql-than-a-data-analyst-f079518efab2?source=collection_archive---------0-----------------------#2023-01-05)

[](https://medium.com/@marietruong?source=post_page-----f079518efab2--------------------------------)[![Marie Truong](../Images/2816e49beef958724dc0f38cfa49c4be.png)](https://medium.com/@marietruong?source=post_page-----f079518efab2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f079518efab2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f079518efab2--------------------------------) [Marie Truong](https://medium.com/@marietruong?source=post_page-----f079518efab2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4cfa1d0b321f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chatgpt-write-better-sql-than-a-data-analyst-f079518efab2&user=Marie+Truong&userId=4cfa1d0b321f&source=post_page-4cfa1d0b321f----f079518efab2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f079518efab2--------------------------------) · 6 分钟阅读 · 2023年1月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff079518efab2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chatgpt-write-better-sql-than-a-data-analyst-f079518efab2&user=Marie+Truong&userId=4cfa1d0b321f&source=-----f079518efab2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff079518efab2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-chatgpt-write-better-sql-than-a-data-analyst-f079518efab2&source=-----f079518efab2---------------------bookmark_footer-----------)![](../Images/f2655bf17162524ed1547496c3c4b170.png)

图片由 [DeepMind](https://unsplash.com/@deepmind?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我尝试了 [ChatGPT](https://chat.openai.com/)，这是一个专为在对话环境中生成类人文本而设计的 GPT-3 语言模型变体。当然，像大多数人一样，我想知道：AI 能做我的工作吗？而且它能比我做得更好吗？

我有2年的数据分析师和分析工程师的工作经验。根据 [BBC Science Focus](https://www.sciencefocus.com/future-technology/gpt-3/)，ChatGPT 已经摄取了570 GB的数据。那么谁写的 SQL 更好呢？

# 来玩吧！

这个游戏将基于 3 个 LeetCode SQL 挑战（一个简单的，两个中等的）。我会先写出每个解决方案，然后将练习题发送给 ChatGPT，看看哪个解决方案效果最好。

我将提供每个挑战的链接，供你尝试超越 ChatGPT。

## 挑战 1（简单）

这个挑战名为[客户下单数量最多](https://leetcode.com/problems/customer-placing-the-largest-number-of-orders/)。

![](../Images/0ec1b96ddbf576a2d6c76fb843c70491.png)

作者截图

这是我写的查询：

```py
WITH layer_1 AS (
  SELECT 
    customer_number, COUNT(DISTINCT order_number) AS order_number
  FROM orders
  GROUP BY customer_number
)
SELECT customer_number 
FROM layer_1
ORDER BY order_number…
```
