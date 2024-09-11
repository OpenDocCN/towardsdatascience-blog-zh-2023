# ChatGPT 高级数据分析用于定制 Matplotlib 岩心图

> 原文：[https://towardsdatascience.com/chatgpt-advanced-data-analytics-for-custom-matplotlib-well-log-plots-b857c68fa42?source=collection_archive---------2-----------------------#2023-09-18](https://towardsdatascience.com/chatgpt-advanced-data-analytics-for-custom-matplotlib-well-log-plots-b857c68fa42?source=collection_archive---------2-----------------------#2023-09-18)

## 使用 OpenAI 的代码解释器创建岩心图用于岩石物理学和地球科学解释

[](https://andymcdonaldgeo.medium.com/?source=post_page-----b857c68fa42--------------------------------)[![安迪·麦克唐纳](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----b857c68fa42--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b857c68fa42--------------------------------)[![数据科学领域](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b857c68fa42--------------------------------) [安迪·麦克唐纳](https://andymcdonaldgeo.medium.com/?source=post_page-----b857c68fa42--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-advanced-data-analytics-for-custom-matplotlib-well-log-plots-b857c68fa42&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----b857c68fa42---------------------post_header-----------) 发表在 [数据科学领域](https://towardsdatascience.com/?source=post_page-----b857c68fa42--------------------------------) ·15分钟阅读·2023年9月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb857c68fa42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-advanced-data-analytics-for-custom-matplotlib-well-log-plots-b857c68fa42&user=Andy+McDonald&userId=9c280f85f15c&source=-----b857c68fa42---------------------clap_footer-----------)

-- 

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb857c68fa42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-advanced-data-analytics-for-custom-matplotlib-well-log-plots-b857c68fa42&source=-----b857c68fa42---------------------bookmark_footer-----------)![](../Images/825c2744d109da5e1fa631b5999278d3.png)

照片由 [D koi](https://unsplash.com/@dkoi?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

ChatGPT 的代码解释器，现在更名为 [**高级数据分析**](https://www.pluralsight.com/resources/blog/data/ChatGPT-Advanced-Data-Analytics)，已经推出一段时间了。它于 2023 年 7 月 6 日上线，是一个由 OpenAI 开发的插件，允许用户上传数据并对其进行分析。这可以包括清理数据、创建可视化图表以及总结数据。

与其依赖你编写 Python 代码来分析数据，不如利用 ChatGPT 直接用简单的英语告诉它你要做什么。它会为你完成分析工作。

正如我的常读者们所知道的，我是 [**matplotlib**](https://matplotlib.org/) 的忠实粉丝。尽管这个库看起来笨重且使用起来费时，但只要稍加努力，就能创建出令人惊艳的可视化效果。

在使用了这个新工具之后，我认为是时候看看 [**ChatGPT**](https://openai.com/chatgpt) 和 [**高级数据分析**](https://openai.com/blog/chatgpt-plugins) 插件如何用于创建自定义图表以处理井下数据了。

在继续之前，由于针对 OpenAI 的法律案件不断增加：

> **始终对上传到 ChatGPT 的数据保持谨慎，因为**…
