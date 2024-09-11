# AI对裁员的真实影响是什么？深入分析

> 原文：[https://towardsdatascience.com/what-is-the-real-impact-of-ai-on-job-cuts-7197ad1377f9?source=collection_archive---------3-----------------------#2023-11-06](https://towardsdatascience.com/what-is-the-real-impact-of-ai-on-job-cuts-7197ad1377f9?source=collection_archive---------3-----------------------#2023-11-06)

[](https://medium.com/@AhmedF?source=post_page-----7197ad1377f9--------------------------------)[![Ahmed Fessi](../Images/bd26738b0d8a0d5b465af62ad849b4bd.png)](https://medium.com/@AhmedF?source=post_page-----7197ad1377f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7197ad1377f9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7197ad1377f9--------------------------------) [Ahmed Fessi](https://medium.com/@AhmedF?source=post_page-----7197ad1377f9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F37fb6215fe2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-the-real-impact-of-ai-on-job-cuts-7197ad1377f9&user=Ahmed+Fessi&userId=37fb6215fe2c&source=post_page-37fb6215fe2c----7197ad1377f9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7197ad1377f9--------------------------------) ·8 min read·2023年11月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7197ad1377f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-the-real-impact-of-ai-on-job-cuts-7197ad1377f9&user=Ahmed+Fessi&userId=37fb6215fe2c&source=-----7197ad1377f9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7197ad1377f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-the-real-impact-of-ai-on-job-cuts-7197ad1377f9&source=-----7197ad1377f9---------------------bookmark_footer-----------)![](../Images/461771988d9ee0ddd38fa5e3d4b48abb.png)

AI替代人类，未来主义，油画风格——由作者使用Dall-E 3生成

自从ChatGPT发布以来，随着生成性AI的普及，许多人开始关注AI对就业市场的影响。

多家公司宣布裁员，因为他们的团队采用了AI，从而“需要更少的劳动力”。没有行业似乎幸免，没有职能似乎幸免。

这些公告涵盖了像[英国电信](https://medium.com/@AhmedF/is-ai-already-massively-replacing-humans-british-telecom-ai-and-layoffs-8defaaf1b6b5)这样的 大公司，也涵盖了像这家法国公关公司[用人工智能替换一半员工](https://www.ouest-france.fr/high-tech/intelligence-artificielle/cette-entreprise-francaise-vire-217-salaries-pour-les-remplacer-par-une-intelligence-artificielle-1dea7af2-560d-11ee-ba56-a7ce59b686ce)这样的较小公司。

这显然引发了与**工作安全**相关的担忧，因为不同规模、不同领域和不同职能的公司似乎正在大规模地用人工智能取代人类。

这些担忧甚至引起了拜登政府的关注。在总统拜登签署的关于安全、可靠且值得信赖的人工智能的[行政命令](https://www.whitehouse.gov/briefing-room/statements-releases/2023/10/30/fact-sheet-president-biden-issues-executive-order-on-safe-secure-and-trustworthy-artificial-intelligence/)中，声明中提到，“*制作一份关于人工智能的* ***潜在劳动市场影响***的报告，并研究和确定加强对* ***面临劳动中断的工人，包括来自人工智能的***联邦支持的选项。”

因此，作为一名数据分析师，我想用数字和数据评估人工智能对裁员和失业的真实影响。

# 方法论

## **数据来源**
