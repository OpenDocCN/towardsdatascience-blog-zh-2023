# 优化探索性数据分析中的重复任务

> 原文：[https://towardsdatascience.com/streamlining-repetitive-tasks-during-exploratory-data-analysis-46a40fe1d588?source=collection_archive---------7-----------------------#2023-10-24](https://towardsdatascience.com/streamlining-repetitive-tasks-during-exploratory-data-analysis-46a40fe1d588?source=collection_archive---------7-----------------------#2023-10-24)

## 数据科学中的自动化

## 邀请您识别重复的 EDA 任务并创建自动化工作流，通过示例工具进行说明。

[](https://medium.com/@christabellecp?source=post_page-----46a40fe1d588--------------------------------)[![Christabelle Pabalan](../Images/24187865b6e9d03ae1aabf873ce1e67c.png)](https://medium.com/@christabellecp?source=post_page-----46a40fe1d588--------------------------------)[](https://towardsdatascience.com/?source=post_page-----46a40fe1d588--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----46a40fe1d588--------------------------------) [Christabelle Pabalan](https://medium.com/@christabellecp?source=post_page-----46a40fe1d588--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4200eb8e8b26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamlining-repetitive-tasks-during-exploratory-data-analysis-46a40fe1d588&user=Christabelle+Pabalan&userId=4200eb8e8b26&source=post_page-4200eb8e8b26----46a40fe1d588---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----46a40fe1d588--------------------------------) ·7分钟阅读·2023年10月24日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F46a40fe1d588&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamlining-repetitive-tasks-during-exploratory-data-analysis-46a40fe1d588&source=-----46a40fe1d588---------------------bookmark_footer-----------)![](../Images/e5031188e988019bb8a2ffc2e978e858.png)

图片由作者提供（DALL-E 生成）

## 编程原则：自动化琐碎任务

人们常说懒惰的程序员是最好的程序员。然而，更准确的说法是，那些没有耐心处理重复工作流程的程序员会投入时间来自动化他们能做的所有事情，从而避免这些任务。简而言之，最好的程序员不会耐心地重复琐碎的任务——他们会自动化这些任务。熟练的程序员是“懒惰”的，因为他们会在前期投入时间来创建能够在后续节省他们精力的工具。这可能意味着学习键盘快捷键、创建自定义模块或寻找智能软件来自动化工作流程。

在一篇标题为“*为什么优秀的程序员既懒惰又愚蠢*”的文章中，Philipp Lenssen 说：

> “只有懒惰的程序员才会避免编写单调、重复的代码——从而避免冗余，这对软件维护和灵活重构是个敌人……”
