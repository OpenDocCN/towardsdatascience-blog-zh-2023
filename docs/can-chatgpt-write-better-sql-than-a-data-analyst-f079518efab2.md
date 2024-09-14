# ChatGPT 能写出比数据分析师更好的 SQL 吗？

> 原文：[`towardsdatascience.com/can-chatgpt-write-better-sql-than-a-data-analyst-f079518efab2`](https://towardsdatascience.com/can-chatgpt-write-better-sql-than-a-data-analyst-f079518efab2)

[](https://medium.com/@marietruong?source=post_page-----f079518efab2--------------------------------)![Marie Truong](https://medium.com/@marietruong?source=post_page-----f079518efab2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f079518efab2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f079518efab2--------------------------------) [Marie Truong](https://medium.com/@marietruong?source=post_page-----f079518efab2--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f079518efab2--------------------------------) ·6 分钟阅读·2023 年 1 月 5 日

--

![](img/f2655bf17162524ed1547496c3c4b170.png)

[DeepMind](https://unsplash.com/@deepmind?utm_source=medium&utm_medium=referral)拍摄的照片在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我尝试了[ChatGPT](https://chat.openai.com/)，这是 GPT-3 语言模型的一个变体，专门设计用于在对话上下文中生成类似人类的文本。当然，就像大多数人一样，我也好奇：人工智能能做我的工作吗？它能做得比我好吗？

我有 2 年的数据分析师和分析工程师工作经验。根据[BBC Science Focus](https://www.sciencefocus.com/future-technology/gpt-3/)，ChatGPT 已吸收了 570 GB 的数据。那么谁写的 SQL 更好呢？

# 来玩吧！

这个游戏将基于 3 个 LeetCode SQL 挑战（一个简单，两个中等）。我会先写出每个解决方案，然后将练习发给 ChatGPT，看看哪个解决方案效果最好。

我将提供每个挑战的链接，供你也尝试击败 ChatGPT。

## 挑战 1（简单）

这个挑战叫做[客户下的最大订单数量](https://leetcode.com/problems/customer-placing-the-largest-number-of-orders/)。

![](img/0ec1b96ddbf576a2d6c76fb843c70491.png)

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
ORDER BY order_number DESC 
LIMIT 1
```

它通过了，并且运行时间正确：

![](img/75b5c9b6e291d0dea7aa3695f4a59d5a.png)

作者截图

现在让我们看看 ChatGPT 在这方面的表现。

这是 ChatGPT 的回答：

![](img/ac75a691af623b0303e4b053eb75d730.png)

作者截图

ChatGPT 甚至解释了它所做的工作。我觉得这个查询的可读性不高——这也是我喜欢公共表表达式的原因之一——但让我们看看它的表现如何。

![](img/41c06d21e8cae27c840c9f1cc7f5fb93.png)

令人印象深刻的是，它有效，但速度比我的结果慢。尽管我很高兴能做得比 ChatGPT 更好，但我希望能知道如何改进这个查询。

## 挑战 2（中等）

下一个挑战叫做 [树节点](https://leetcode.com/problems/tree-node/)。

![](img/7a4f164d806385bfe3e058a5ee756a0d.png)

作者截图

我写的第一个查询是这个：

```py
# Write your MySQL query statement below
WITH l1 AS (
SELECT 
    t.id, 
    c.id AS c_id,
    t.p_id
FROM Tree t
LEFT JOIN Tree c
ON c.p_id = t.id
), 
l2 AS (
SELECT 
    id, 
    COUNT(DISTINCT c_id) AS nb_childrens, 
    COUNT(DISTINCT p_id) AS nb_parents
FROM l1
GROUP BY id
)
SELECT id, 
    CASE 
        WHEN nb_childrens >0 AND nb_parents >0 THEN "Inner"
        WHEN nb_childrens > 0 THEN "Root"
        ELSE "Leaf"
    END AS type
FROM l2
```

我在提交之前运行了它，它得到了错误的结果…

![](img/dfc0882e59ed813ed3fbacfd3885808a.png)

作者截图

结果我没有足够注意示例，特别是第二个：

![](img/0f80c2ce4f7be72ebc34935c069bd992.png)

作者截图

当一个节点既是叶子又是根节点时，它应该被输出为根节点。我将 CASE WHEN 的顺序改为：

```py
CASE 
        WHEN nb_childrens >0 AND nb_parents >0 THEN "Inner"
        WHEN nb_parents > 0 THEN "Leaf"
        ELSE "Root"
    END
```

而这次，它通过了！

我的查询得到了平均结果：

![](img/4f27f92a48e4a3f25777333bd8b4c7db.png)

作者截图

现在是 ChatGPT 玩游戏的时候了！

ChatGPT 阅读了所有示例，并且没有在既是根节点又是叶节点的节点上犯我的错误：

![](img/fbf217774e0e7c72081c3e00851f7e6d.png)

作者截图

但它仍然给了我一个错误的答案：

![](img/cfd932af855fe4fdb0096174a7632fd2.png)

作者截图

ChatGPT 没有按每个 id 返回一行。

所以我决定给 ChatGPT 一个提示：

![](img/13d29a10b7d68e364fd0d7e59166fbc4.png)

作者截图

而且它能够纠正它！

![](img/c07374a6cc891bd1c1b16cf176e5643a.png)

作者截图

我们在这个挑战中都犯了一个错误，但都能纠正它（尽管我不得不给 ChatGPT 提示）。在运行时间方面我有稍微更好的结果，所以得分给我，但差距很小！

## 挑战 3（中等）

最后的挑战叫做 [资本收益/亏损](https://leetcode.com/problems/capital-gainloss/)。

![](img/5751d0eb2a65226a2eef11f9356ea05d.png)

作者截图

这是我写的查询：

```py
SELECT 
  stock_name, 
  SUM(
    CASE 
      WHEN operation = "Buy" THEN -1*price 
      ELSE price
    END
    ) 
  AS  capital_gain_loss
FROM Stocks
GROUP BY stock_name
```

![](img/19932aa1c08426f372e794b9a7a92345.png)

作者截图

它通过了，但运行时间相当差，超过 90% 的玩家在这方面表现得比我好。

让我们看看 ChatGPT 是否其中之一。

![](img/332d69a37cb812ef6b02498c0d1bfad4.png)

作者截图

让我们尝试这个解决方案：

![](img/5584cb1710d6e08c45369031e8d938a4.png)

作者截图

再次，它得到了错误的结果。ChatGPT 将一个买入与每一个未来的卖出连接，而不仅仅是对应的一个，因此它的解决方案只在只有一对买入/卖出操作时有效。

我试图告诉 ChatGPT 纠正它的错误，而没有给它任何提示：

![](img/ae6fcb9e1d109919d54a63a454f95d5c.png)

作者截图

不幸的是，它给了我完全相同的查询。所以我抱怨了：

![](img/6fa457ec958761e22d4c1b3e004ebaf7.png)

作者截图

![](img/46ad31bb200fc87515ef303973e01a98.png)

作者截图

这一次，ChatGPT 和我使用了类似的结构，查询通过了。然而，我不确定 ChatGPT 是否真的理解了它一开始错在哪里。

![](img/7aaa930367d61792b2536aa4970efe23.png)

作者截图

ChatGPT 的运行时间稍微比我差。

# **结果**

```py
 Challenge    ChatGPT 🤖      Data Analyst 👩‍💻          Winner        
 ----------- --------------- --------------------- --------------------- 
          1    ✅ (22%)         ✅ (62%)               Data Analyst 👩‍💻  
          2    ❌ ✅ (36%)      ❌ ✅ (54%)             Data Analyst 👩‍💻   
          3    ❌ ❌ ✅ (8%)    ✅ (5%)                 Data Analyst 👩‍💻
```

我认为可以公平地说，我在这次 SQL 挑战中“战胜”了 ChatGPT。我仍然对它的能力感到印象深刻，并惊讶于它能够纠正自己的错误！ChatGPT 在速度上绝对超过我；它能在几秒钟内写出有效的 SQL 语法，而我需要几分钟。

但它仍然有 50%的错误。即使它成功完成了每一个挑战，我也不会担心我的工作。利益相关者从不会向分析师提出如此明确的请求和输出示例。他们带来的是业务问题，我们必须考虑如何利用现有数据来找到最佳答案。ChatGPT 能做到这一点吗？

我问了 ChatGPT 对这个问题的看法：

![](img/e0edd8c7fd6bf821297d055a713e4510.png)

截图作者

这就总结完了！

希望你喜欢这篇文章！如果喜欢的话，请关注我，获取更多关于 SQL 和数据分析的内容。
