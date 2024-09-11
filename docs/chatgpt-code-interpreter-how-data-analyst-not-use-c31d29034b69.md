# 你不能用 ChatGPT 代码解释器的 4 种方式，这将扰乱你的分析

> 原文：[`towardsdatascience.com/chatgpt-code-interpreter-how-data-analyst-not-use-c31d29034b69?source=collection_archive---------6-----------------------#2023-07-31`](https://towardsdatascience.com/chatgpt-code-interpreter-how-data-analyst-not-use-c31d29034b69?source=collection_archive---------6-----------------------#2023-07-31)

## 我认为代码解释器并没有像人们宣传的那样具有革命性。数据专业人士应该注意这些问题。是否有解决办法？

[](https://medium.com/@kenneth.b.jee?source=post_page-----c31d29034b69--------------------------------)![Ken Jee](https://medium.com/@kenneth.b.jee?source=post_page-----c31d29034b69--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c31d29034b69--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c31d29034b69--------------------------------) [Ken Jee](https://medium.com/@kenneth.b.jee?source=post_page-----c31d29034b69--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6ee1f7466557&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-code-interpreter-how-data-analyst-not-use-c31d29034b69&user=Ken+Jee&userId=6ee1f7466557&source=post_page-6ee1f7466557----c31d29034b69---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c31d29034b69--------------------------------) ·7 分钟阅读·2023 年 7 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc31d29034b69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-code-interpreter-how-data-analyst-not-use-c31d29034b69&user=Ken+Jee&userId=6ee1f7466557&source=-----c31d29034b69---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc31d29034b69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-code-interpreter-how-data-analyst-not-use-c31d29034b69&source=-----c31d29034b69---------------------bookmark_footer-----------)![](img/f0ef911266288b4fee0c9088a9f2f811.png)

图片由 [作者](https://youtu.be/lqYxmYMkfK8) 提供

# 目录

• 代码解释器的威力

• 代码解释器的局限性

• • • 数据库不可访问性

• • • Python 版本限制

• • • 不可用的库

• • • GPU 限制

• 解决这些问题

• • • 临时混合修复

• 结论

ChatGPT 将会取代数据分析师。至少，这似乎是基于大家对代码解释器推出的讨论。我在过去一个月左右有机会使用它。老实说，我并没有特别感到惊艳。

![](img/e8cea426fc28a0691ae5d5ebd013b33a.png)

图片来源：[作者](https://youtu.be/lqYxmYMkfK8)
