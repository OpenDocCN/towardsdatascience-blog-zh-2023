# 我的 Python 脚本如何听起来更像自然对话

> 原文：[`towardsdatascience.com/python-code-like-natural-english-ff5cb09e97b9?source=collection_archive---------6-----------------------#2023-03-22`](https://towardsdatascience.com/python-code-like-natural-english-ff5cb09e97b9?source=collection_archive---------6-----------------------#2023-03-22)

## 管道是一种极为出色的技术，可以使代码更加人性化

[](https://thuwarakesh.medium.com/?source=post_page-----ff5cb09e97b9--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----ff5cb09e97b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ff5cb09e97b9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff5cb09e97b9--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----ff5cb09e97b9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-code-like-natural-english-ff5cb09e97b9&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----ff5cb09e97b9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff5cb09e97b9--------------------------------) · 5 分钟阅读 · 2023 年 3 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fff5cb09e97b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-code-like-natural-english-ff5cb09e97b9&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----ff5cb09e97b9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fff5cb09e97b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-code-like-natural-english-ff5cb09e97b9&source=-----ff5cb09e97b9---------------------bookmark_footer-----------)![](img/5ac47c52ddaf312fc0a3540f558cc343.png)

图片由 [Pavel Danilyuk](https://www.pexels.com/photo/close-up-shot-of-white-toy-robot-on-blue-and-pink-background-8294666/) 提供，来源于 Pexels。

你的代码也是你的文档。

人们常说优秀的程序员不会在代码中添加注释。他们认为如果代码难以编写，那么其他人理解和修改这段代码也应该是有挑战的。因此，他们编写简单明了的代码。

尽管我不主张完全不加注释，但我不能否认这个说法中确实有一定的真理。代码应该是任何人都能阅读的！

这就是为什么 SQL 代码很棒的原因。声明式语法比任何通用编程语言都要易读。我们确切知道我们选择了什么、过滤条件、如何聚合等。

[这 5 种 SQL 技巧涵盖了 ~80% 的实际项目](https://towardsdatascience.com/5-advanced-sql-techniques-for-real-life-projects-f2db9b6680e2?source=post_page-----ff5cb09e97b9--------------------------------)

### 加速你的 SQL 学习曲线。

[towardsdatascience.com](https://towardsdatascience.com/5-advanced-sql-techniques-for-real-life-projects-f2db9b6680e2?source=post_page-----ff5cb09e97b9--------------------------------)

我们能否组织我们的 Python 脚本以提高可读性？如果我们能使 Python 代码看起来更具声明性，是否会提升代码质量？这会更有趣吗？
