# 30 个 SQL 查询通过其 Pandas 等价物解释

> 原文：[https://towardsdatascience.com/sql-for-people-who-love-pandas-a-10-minute-yet-thorough-tutorial-c189de9d417d?source=collection_archive---------5-----------------------#2023-06-09](https://towardsdatascience.com/sql-for-people-who-love-pandas-a-10-minute-yet-thorough-tutorial-c189de9d417d?source=collection_archive---------5-----------------------#2023-06-09)

## SQL 变得对喜爱 Pandas 的人更加容易

[](https://ibexorigin.medium.com/?source=post_page-----c189de9d417d--------------------------------)[![Bex T.](../Images/516496f32596e8ad56bf07f178a643c6.png)](https://ibexorigin.medium.com/?source=post_page-----c189de9d417d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c189de9d417d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c189de9d417d--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----c189de9d417d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-for-people-who-love-pandas-a-10-minute-yet-thorough-tutorial-c189de9d417d&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----c189de9d417d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c189de9d417d--------------------------------) · 10 分钟阅读 · 2023年6月9日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc189de9d417d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-for-people-who-love-pandas-a-10-minute-yet-thorough-tutorial-c189de9d417d&user=Bex+T.&userId=39db050c2ac2&source=-----c189de9d417d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc189de9d417d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-for-people-who-love-pandas-a-10-minute-yet-thorough-tutorial-c189de9d417d&source=-----c189de9d417d---------------------bookmark_footer-----------)![](../Images/4804e3d33ecae544cceeee4a0f1df5f4.png)

图片由我与 Midjourney 制作

## 动机

自1974年以来，SQL 主导了数据世界，直到2008年 Pandas 的出现，提供了如内置可视化和灵活的数据处理等吸引人的功能。它迅速成为数据探索的首选工具，超越了 SQL。

不过不要被迷惑，SQL 仍然占据重要地位。它是数据科学领域第二大需求的语言，也是第三大增长最快的语言（见 [这里](/the-most-in-demand-skills-for-data-scientists-in-2021-4b2a808f4005)）。因此，尽管 Pandas 扮演了主角，SQL 仍然是任何数据科学家必备的技能。

让我们来看看当你已经掌握 Pandas 时，学习 SQL 会有多简单。

## 连接到数据库

设置 SQL 工作区并连接到示例数据库可能会非常麻烦。首先，你需要安装你喜欢的 SQL 类型（PostgreSQL、MySQL 等）并下载一个 SQL IDE。在这里进行这些操作会使我们偏离文章的目的，因此我们将使用一个快捷方法。

具体来说，我们将直接在 Jupyter Notebook 中运行 SQL 查询，而无需额外的步骤。我们需要做的就是使用 pip 安装 `ipython-sql` 包：

```py
pip install ipython-sql
```
