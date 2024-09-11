# 使用 OpenCLIP 进行图像搜索和自动标注

> 原文：[`towardsdatascience.com/using-openclip-for-image-search-and-automatic-captioning-fa1cbbd48ce4?source=collection_archive---------7-----------------------#2023-03-07`](https://towardsdatascience.com/using-openclip-for-image-search-and-automatic-captioning-fa1cbbd48ce4?source=collection_archive---------7-----------------------#2023-03-07)

## 如何利用更多的数据和新的机器学习训练技术来改进图像和文本嵌入，以用于各种应用

[](https://robgon.medium.com/?source=post_page-----fa1cbbd48ce4--------------------------------)![Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----fa1cbbd48ce4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fa1cbbd48ce4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa1cbbd48ce4--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----fa1cbbd48ce4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-openclip-for-image-search-and-automatic-captioning-fa1cbbd48ce4&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----fa1cbbd48ce4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa1cbbd48ce4--------------------------------) ·12 分钟阅读·2023 年 3 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffa1cbbd48ce4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-openclip-for-image-search-and-automatic-captioning-fa1cbbd48ce4&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=-----fa1cbbd48ce4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffa1cbbd48ce4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-openclip-for-image-search-and-automatic-captioning-fa1cbbd48ce4&source=-----fa1cbbd48ce4---------------------bookmark_footer-----------)![](img/01279abccfb8391b5c22a466bcb77154.png)

“**高科技计算机系统用于在大型库中查找图片，**”我*使用 AI 图像生成程序 Midjourney 创建的图像*，并由作者编辑

自从 2021 年 OpenAI 的 CLIP 系统发布以来，我一直在使用和撰写关于该系统的内容[1]。该系统包括图像和文本编码模型，可用于各种形式的跨模态比较，例如使用文本查询快速找到库中最匹配的图像。

在 2022 年 12 月，一个独立的研究小组 LAION 发布了一篇名为“可重复的对比语言-图像学习扩展规律”[2]的论文，描述了他们如何首先重新实现和训练一个类似于 CLIP 的模型，然后通过训练更大的数据集和使用新的机器学习技术来改进系统。他们将他们的新模型称为 OpenCLIP。

在这篇文章中，我将提供一些关于原始 CLIP 的背景信息，描述 LAION 如何改进模型，并展示我在使用[国会图书馆 Flickr 照片流](https://www.flickr.com/photos/library_of_congress/)的图像进行的两个系统实验的结果。我还实现了 Meta AI 的一个叫做“嵌入算术”的酷技术，以通过图像和文本提示[3]搜索照片。
