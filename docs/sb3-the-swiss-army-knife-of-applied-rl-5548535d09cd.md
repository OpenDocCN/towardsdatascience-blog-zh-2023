# Sb3，应用强化学习的瑞士军刀

> 原文：[`towardsdatascience.com/sb3-the-swiss-army-knife-of-applied-rl-5548535d09cd?source=collection_archive---------14-----------------------#2023-10-26`](https://towardsdatascience.com/sb3-the-swiss-army-knife-of-applied-rl-5548535d09cd?source=collection_archive---------14-----------------------#2023-10-26)

## 选择适合你的模型，适用于任何环境

[](https://medium.com/@byjameskoh?source=post_page-----5548535d09cd--------------------------------)![James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----5548535d09cd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5548535d09cd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5548535d09cd--------------------------------) [James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----5548535d09cd--------------------------------)

·

[点击此处](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F780706b02d58&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsb3-the-swiss-army-knife-of-applied-rl-5548535d09cd&user=James+Koh%2C+PhD&userId=780706b02d58&source=post_page-780706b02d58----5548535d09cd---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5548535d09cd--------------------------------) · 8 分钟阅读 · 2023 年 10 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5548535d09cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsb3-the-swiss-army-knife-of-applied-rl-5548535d09cd&user=James+Koh%2C+PhD&userId=780706b02d58&source=-----5548535d09cd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5548535d09cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsb3-the-swiss-army-knife-of-applied-rl-5548535d09cd&source=-----5548535d09cd---------------------bookmark_footer-----------)![](img/3359413fa03ba1b4537646411e4dea66.png)

图像由 DALL·E 3 根据提示“创建一张逼真的瑞士军刀打开的图像”生成

Stablebaseline3 (sb3) 就像一把瑞士军刀。它是一个多功能工具，可以用于许多目的。就像瑞士军刀可以在你被困在丛林中时救你一命，sb3 也可以在你面临看似不可能完成的截止日期时拯救你。

> 本指南使用 gymnasium=0.28.1 和 stable-baselines=2.1.0。如果你使用不同版本，或者参考其他旧指南，可能无法获得下面的结果。不过不要担心，这里也提供了安装指南。如果你按照我的说明操作，保证可以获得预期结果。

# [1] 你将在这里获得什么

Stablebaseline3 使用起来很简单。它的文档也很详尽，你可以自己跟随教程。但…

+   你是否参考过旧版指南（也许是使用`gym`的那些），结果在你的机器上发现错误？

+   你是否能够始终确保兼容性？

+   如果你想使用`gymnasium`的环境并修改例如奖励的话怎么办？

+   你知道如何包装你自己的任务，以便在几行代码中应用 SOTA 模型吗？
