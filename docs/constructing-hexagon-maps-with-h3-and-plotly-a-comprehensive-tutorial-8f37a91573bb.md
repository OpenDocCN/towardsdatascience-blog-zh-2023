# 使用 H3 和 Plotly 构建六边形地图：全面教程

> 原文：[`towardsdatascience.com/constructing-hexagon-maps-with-h3-and-plotly-a-comprehensive-tutorial-8f37a91573bb?source=collection_archive---------0-----------------------#2023-10-31`](https://towardsdatascience.com/constructing-hexagon-maps-with-h3-and-plotly-a-comprehensive-tutorial-8f37a91573bb?source=collection_archive---------0-----------------------#2023-10-31)

## 解锁六边形地图在数据分析中的潜力

[](https://amandaiglesiasmoreno.medium.com/?source=post_page-----8f37a91573bb--------------------------------)![Amanda Iglesias Moreno](https://amandaiglesiasmoreno.medium.com/?source=post_page-----8f37a91573bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f37a91573bb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f37a91573bb--------------------------------) [Amanda Iglesias Moreno](https://amandaiglesiasmoreno.medium.com/?source=post_page-----8f37a91573bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1bace2932c65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstructing-hexagon-maps-with-h3-and-plotly-a-comprehensive-tutorial-8f37a91573bb&user=Amanda+Iglesias+Moreno&userId=1bace2932c65&source=post_page-1bace2932c65----8f37a91573bb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f37a91573bb--------------------------------) ·7 分钟阅读·2023 年 10 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8f37a91573bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstructing-hexagon-maps-with-h3-and-plotly-a-comprehensive-tutorial-8f37a91573bb&user=Amanda+Iglesias+Moreno&userId=1bace2932c65&source=-----8f37a91573bb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8f37a91573bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconstructing-hexagon-maps-with-h3-and-plotly-a-comprehensive-tutorial-8f37a91573bb&source=-----8f37a91573bb---------------------bookmark_footer-----------)![](img/58bb2917e3305811b11d631a267130ed.png)

[Sam Balye](https://unsplash.com/es/@sambalye) 在 Unsplash

通常，当我们想要使用色斑图可视化一个区域内的变量时，我们会使用广为人知的行政几何形状。例如，如果我们想查看欧洲的失业率，我们可以通过各国的相应州进行可视化。

然而，行政区域通常是不规则的，并且相互之间的大小差异很大。因此，**一个有用的替代方案是使用六边形来划分区域，以可视化任何变量**。优点包括拥有平衡的几何形状，以便更好地进行区域比较和改善区域覆盖。此外，六边形地图具有最小化视觉偏差的优势，因为它们提供了区域的平等表示，而不像传统的行政边界，后者由于其不规则的形状和大小，有时可能扭曲数据的感知。

在本文中，**我们将提供如何在 Python 中创建六边形地图的逐步说明**。为此，我们将使用两个简化地图构建过程的库：H3 和 Plotly。

# 分析数据：巴塞罗那市酒店数据集
