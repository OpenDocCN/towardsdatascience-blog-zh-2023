# 从 SQL 到 Julia：数据科学的其他编程语言

> 原文：[`towardsdatascience.com/from-sql-to-julia-data-sciences-other-programming-languages-ff11ae2f0c20?source=collection_archive---------3-----------------------#2023-07-27`](https://towardsdatascience.com/from-sql-to-julia-data-sciences-other-programming-languages-ff11ae2f0c20?source=collection_archive---------3-----------------------#2023-07-27)

[](https://towardsdatascience.medium.com/?source=post_page-----ff11ae2f0c20--------------------------------)![TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----ff11ae2f0c20--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ff11ae2f0c20--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff11ae2f0c20--------------------------------) [TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----ff11ae2f0c20--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-sql-to-julia-data-sciences-other-programming-languages-ff11ae2f0c20&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----ff11ae2f0c20---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff11ae2f0c20--------------------------------) · 发送为 新闻通讯 · 3 分钟阅读 · 2023 年 7 月 27 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fff11ae2f0c20&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-sql-to-julia-data-sciences-other-programming-languages-ff11ae2f0c20&user=TDS+Editors&userId=7e12c71dfa81&source=-----ff11ae2f0c20---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fff11ae2f0c20&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-sql-to-julia-data-sciences-other-programming-languages-ff11ae2f0c20&source=-----ff11ae2f0c20---------------------bookmark_footer-----------)

Python 可能是数据科学家和机器学习从业者的主流编程语言，但成为编程多语种高手的好处越来越明显。数据团队所做的项目跨越多个平台和架构，能够切换到不那么常见但有时更高效的语言，可以成为一个强大的优势。

本周的重点文章涵盖了广泛的内容：它们关注多种语言、项目和工作流程。这些帖子邀请你卷起袖子，开始尝试一些代码——它们可能会激发你进一步扩展知识。

+   数据库查询语言 SQL 并不陌生；它已经存在了几十年，并且仍然是许多数据工作者进入编码的常见途径。[伊法特·马利克·戈尔](https://medium.com/u/88491120e677?source=post_page-----ff11ae2f0c20--------------------------------) 对窗口函数的概述是**SQL 词汇的核心元素的极佳介绍（或复习）**。

+   如果你的工作涉及大量统计数据，你可能已经在实际工作中遇到过 R。尽管 R 的使用场景可能比你想象的更为广泛，[珍娜·伊格尔森](https://medium.com/u/8300cae51c6c?source=post_page-----ff11ae2f0c20--------------------------------) 通过**一个有用的实操教程，展示了如何使用 R**来解决人力分析中的常见挑战。

+   Python 库在数据可视化领域占据主导地位，以至于你可能会忘记其他语言也能创建精美的图表。[马哈茂德·哈穆赫](https://medium.com/u/b15db3da5667?source=post_page-----ff11ae2f0c20--------------------------------) 对**Rust 的可视化选项进行了深入探讨，重点关注 Plotters 库**及其强大的功能。

![](img/92bebc685e905a41662fee7bd399c963.png)

图片由 [马克斯·斯皮斯基](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   无论你使用什么编程语言，准备数据以便于分析或建模项目都是至关重要的步骤。[艾玛·布德罗](https://medium.com/u/ea170050148c?source=post_page-----ff11ae2f0c20--------------------------------) 带我们了解了**在 Julia 中进行数据筛选的过程——一种在近年来数据科学家中爆炸性增长的相对小众语言**。

+   版本控制是任何软件开发过程中的重要组成部分，而 Git 尽管不是编程语言本身，却是处理和协作代码的首选系统。如果您还处于编码旅程的早期阶段，阅读 [Khuyen Tran](https://medium.com/u/84a02493194a?source=post_page-----ff11ae2f0c20--------------------------------) 的 **数据科学家的全面 Git 使用指南** 是一个明智的下一步。

为了拓宽您的视野，我们很高兴分享我们近期的几篇亮点文章：

+   初创企业生态系统如何应对 AI 快速增长的影响？[Viggy Balagopalakrishnan](https://medium.com/u/b3366eb9a0cf?source=post_page-----ff11ae2f0c20--------------------------------) 提供了基于最新 Y Combinator 批次的 敏锐洞察。

+   [Lambert T Leong 博士](https://medium.com/u/60c1d532d2c0?source=post_page-----ff11ae2f0c20--------------------------------) 展示了 在医疗保健与深度学习交汇处的最新研究，旨在基于体成分成像预测全因死亡率。

+   如果您听说过 Code Interpreter 这个 ChatGPT 插件，且 想看看所有的热议内容，[Natassha Selvaraj](https://medium.com/u/6a2ef1b1f09d?source=post_page-----ff11ae2f0c20--------------------------------) 的项目演示是必看的。

+   如需了解 大数法则的清晰、透彻且引人入胜的解释 及其工作原理，不要错过 [Sachin Date](https://medium.com/u/b75b5b1730f3?source=post_page-----ff11ae2f0c20--------------------------------) 的新深度解析。

+   如果每天有数十亿人使用生成 AI 工具，会带来 什么样的环境影响？[Kasper Groes Albin Ludvigsen](https://medium.com/u/ba0b31bed21a?source=post_page-----ff11ae2f0c20--------------------------------) 探讨了一个棘手、及时且重要的问题。

感谢您支持我们的作者！如果您喜欢在 TDS 阅读的文章，可以考虑 [成为 Medium 会员](https://bit.ly/tds-membership) —— 这将解锁我们的所有档案（以及 Medium 上的其他所有文章）。

我们希望许多人也在 [计划参加 Medium Day](https://blog.medium.com/youre-invited-to-medium-day-526193e251b2#:~:text=On%20August%2012th%2C%202023%2C%20we,share%20their%20lives%20on%20Medium.)，在 8 月 12 日庆祝社区及其使之特别的故事——[注册（免费）现已开放](https://hopin.com/events/medium-day-2023/registration)。

直到下次 Variable，

TDS 编辑部
