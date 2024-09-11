# 用 Pydeck 告别平面地图

> 原文：[https://towardsdatascience.com/say-goodbye-to-flat-maps-with-pydeck-5ce440177bcd?source=collection_archive---------9-----------------------#2023-07-18](https://towardsdatascience.com/say-goodbye-to-flat-maps-with-pydeck-5ce440177bcd?source=collection_archive---------9-----------------------#2023-07-18)

## 提升你的地图绘制技能，通过3D可视化

[](https://medium.com/@lee_vaughan?source=post_page-----5ce440177bcd--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----5ce440177bcd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ce440177bcd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5ce440177bcd--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----5ce440177bcd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsay-goodbye-to-flat-maps-with-pydeck-5ce440177bcd&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----5ce440177bcd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ce440177bcd--------------------------------) ·9分钟阅读·2023年7月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5ce440177bcd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsay-goodbye-to-flat-maps-with-pydeck-5ce440177bcd&user=Lee+Vaughan&userId=5d604015c08b&source=-----5ce440177bcd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ce440177bcd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsay-goodbye-to-flat-maps-with-pydeck-5ce440177bcd&source=-----5ce440177bcd---------------------bookmark_footer-----------)![](../Images/6a954c5dab844b92ab97e64cc81a7215.png)

图片由 Google-Deepmind 提供，来源于 Unsplash

*3D挤出地图* 是一种数据可视化类型，其中3D条形图或柱状图基于其地理坐标放置在地图上。每个条形图的高度表示与该特定位置相关的 *数值*，如人口或温度。以下是一个示例，展示了夏威夷群岛的城市人口密度：

![](../Images/7acc43ad9c0b782ab41528f83d97a031.png)

夏威夷的人口密度（人/平方公里）（所有其他图像由作者提供）

这种类型的地图以“倾斜”的视角呈现，以便条形的高度明显。通过将地图提供的地理信息与条形所代表的垂直维度结合，3D 拉伸地图可以在有趣的空间上下文中传达信息和模式。*相对关系*往往比*绝对值*更为重要。

在这个*快速成功数据科学*项目中，我们将使用 Python 和 pydeck 库轻松创建美国和澳大利亚人口分布的 3D 拉伸地图。在完成这个简短的教程后，你将能够毫不费力地创建你自己的地理空间数据集的惊人可视化。

# 人口数据集
