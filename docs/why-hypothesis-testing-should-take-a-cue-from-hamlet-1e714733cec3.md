# 为什么假设检验应该借鉴《哈姆雷特》

> 原文：[https://towardsdatascience.com/why-hypothesis-testing-should-take-a-cue-from-hamlet-1e714733cec3?source=collection_archive---------3-----------------------#2023-06-26](https://towardsdatascience.com/why-hypothesis-testing-should-take-a-cue-from-hamlet-1e714733cec3?source=collection_archive---------3-----------------------#2023-06-26)

## 统计思维

## 模拟还是不模拟，这是个问题

[](https://kozyrkov.medium.com/?source=post_page-----1e714733cec3--------------------------------)[![Cassie Kozyrkov](../Images/ad18dd12979a4a3ec130bdf8b889af23.png)](https://kozyrkov.medium.com/?source=post_page-----1e714733cec3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e714733cec3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1e714733cec3--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----1e714733cec3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fccb851bb5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-hypothesis-testing-should-take-a-cue-from-hamlet-1e714733cec3&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=post_page-2fccb851bb5e----1e714733cec3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e714733cec3--------------------------------) ·5分钟阅读·2023年6月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1e714733cec3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-hypothesis-testing-should-take-a-cue-from-hamlet-1e714733cec3&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=-----1e714733cec3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1e714733cec3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-hypothesis-testing-should-take-a-cue-from-hamlet-1e714733cec3&source=-----1e714733cec3---------------------bookmark_footer-----------)

如果你是一名 [科学家](http://bit.ly/quaesita_scientists) 或 [数据专业人士](http://bit.ly/quaesita_universe)，你的 [假设检验过程](http://bit.ly/quaesita_donttrust) 可能缺少一个至关重要的步骤，这个步骤在你的典型课程中被悲惨地——或者悲喜剧地？——忽略了。不要担心，在这篇博客文章中，我将向你展示这个缺失的部分，以及为什么你会在戏剧演员的剧本中找到解决方案。

![](../Images/82b0490af332af26e22093ab16e22148.png)

作者用 [Midjourney](http://bit.ly/quaesita_countries) 生成的凯欣德·威利风格的《哈姆雷特》。

*（注：这篇文章中的链接将带你到同一作者的解释文章。）*

# 第一幕，第一场

场景开始时，你满怀胜利感地获得了预算，以[收集一些实际数据](http://bit.ly/quaesita_srstrees2)。也许这一切都将是数字化的；你准备告诉你的工程团队应该记录哪些[变量](http://bit.ly/quaesita_gistlist)或运行哪些在线[实验](http://bit.ly/quaesita_ab)。或者你也许正在走向现实世界，设置一些传感器、准备一些移液管，或做其他任何获取数据所需的事情。（对从现实世界中获取测量的实际操作感兴趣？请查看我关于[采样树木](http://bit.ly/quaesita_srstrees1)的文章。）

但是别急！如果你完全不知道自己在做什么怎么办？搞砸数据收集的现实世界部分不仅非常尴尬，还会极大浪费团队宝贵的时间…
