# 使用 KNIME 进行数据可视化

> 原文：[https://towardsdatascience.com/data-visualization-with-knime-2b86548736d7?source=collection_archive---------6-----------------------#2023-11-17](https://towardsdatascience.com/data-visualization-with-knime-2b86548736d7?source=collection_archive---------6-----------------------#2023-11-17)

## 一步一步的综合指南

[](https://medium.com/@michalszudejko?source=post_page-----2b86548736d7--------------------------------)[![Michal Szudejko](../Images/d4c303d02a79ad29df193ed3b25910d9.png)](https://medium.com/@michalszudejko?source=post_page-----2b86548736d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2b86548736d7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2b86548736d7--------------------------------) [Michal Szudejko](https://medium.com/@michalszudejko?source=post_page-----2b86548736d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd3b37fc311f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-visualization-with-knime-2b86548736d7&user=Michal+Szudejko&userId=d3b37fc311f7&source=post_page-d3b37fc311f7----2b86548736d7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2b86548736d7--------------------------------) ·14 分钟阅读·2023年11月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2b86548736d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-visualization-with-knime-2b86548736d7&user=Michal+Szudejko&userId=d3b37fc311f7&source=-----2b86548736d7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2b86548736d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-visualization-with-knime-2b86548736d7&source=-----2b86548736d7---------------------bookmark_footer-----------)![](../Images/7611d7c67910c61087517565d72a5ebf.png)

来源：作者在 ChatGPT 和 DALL-E 3 中生成的图像。

2007 年诺贝尔和平奖颁给了艾尔·戈尔和政府间气候变化专门委员会（IPCC），以表彰他们提高了对气候变化危害的意识。**《不便的真相》**这部由[戴维斯·古根海姆](https://en.wikipedia.org/wiki/Davis_Guggenheim)导演的纪录片，在将气候变化问题推向公众意识的前沿方面发挥了重要作用。影片中展示了戈尔多次向全球观众展示的幻灯片¹。

**特别值得注意的是阿尔·戈尔在演讲中使用的图表和数据。尽管有一些批评意见，但这些可视化在向广泛观众传达气候危机的紧迫性方面发挥了关键作用。** 戈尔巧妙地使用了引人入胜的视觉效果，有效提高了意识，并强调了对抗气候变化的必要性。**这些可视化将抽象概念转化为清晰、易于理解的信息，对于讨论复杂问题极具价值²。**

> *我建议任何对* ***数据可视化*** *感兴趣的人观看这部影片，或许更重要的是，了解气候变化。*

# 那么，这篇文章讲的是什么？

**惊讶，惊讶，这关乎可视化。** 更准确地说，这关乎制作…
