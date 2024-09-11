# 使用BERT对自由文本银行交易描述进行分类

> 原文：[https://towardsdatascience.com/categorize-free-text-bank-transaction-descriptions-using-bert-44c9cc87735b?source=collection_archive---------1-----------------------#2023-01-30](https://towardsdatascience.com/categorize-free-text-bank-transaction-descriptions-using-bert-44c9cc87735b?source=collection_archive---------1-----------------------#2023-01-30)

## 我自己构建了一个开支跟踪工具

[](https://jin-cui.medium.com/?source=post_page-----44c9cc87735b--------------------------------)[![Jin Cui](../Images/e5ddcbaa6d7da38f960d2c5fea71b538.png)](https://jin-cui.medium.com/?source=post_page-----44c9cc87735b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----44c9cc87735b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----44c9cc87735b--------------------------------) [Jin Cui](https://jin-cui.medium.com/?source=post_page-----44c9cc87735b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F333e9ae026bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcategorize-free-text-bank-transaction-descriptions-using-bert-44c9cc87735b&user=Jin+Cui&userId=333e9ae026bc&source=post_page-333e9ae026bc----44c9cc87735b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----44c9cc87735b--------------------------------) ·7分钟阅读·2023年1月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F44c9cc87735b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcategorize-free-text-bank-transaction-descriptions-using-bert-44c9cc87735b&user=Jin+Cui&userId=333e9ae026bc&source=-----44c9cc87735b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F44c9cc87735b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcategorize-free-text-bank-transaction-descriptions-using-bert-44c9cc87735b&source=-----44c9cc87735b---------------------bookmark_footer-----------)![](../Images/1a38931da456d880799b9fea6b82d20c.png)

分类开支。作者提供的图表

# 情况介绍

我在2022年日历年末购买了一处房产，并且有了按揭。由于财务承诺增加，我想要掌握自己的开支情况。在这之前，我从未意识到自己实际上对开支的具体情况一无所知。弄清楚这一点可能是我个人开支管理的一个良好起点。

自然地，我转向了从在线银行门户下载的银行交易数据，这些数据以 *.csv* 格式保存。以下是2022年最后几天的数据片段。

![](../Images/2077e5d1baa9ed2ae7aa8b08a0685e57.png)

图片 1：作者的银行交易数据。图片来源于作者

根据上面的片段，看起来我在食品上的花费比例较高（如绿色突出显示）。更重要的是，交易描述是自由文本的，有没有办法自动将这些描述分类到多个预定义的费用类别中（如食品、购物、公共事业等）？

使用预训练的大型语言模型如BERT，至少有一种方法可以实现，本文提供了相应的教程！
