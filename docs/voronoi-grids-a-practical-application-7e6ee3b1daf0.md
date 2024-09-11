# Voronoi 网格：一种实际应用

> 原文：[https://towardsdatascience.com/voronoi-grids-a-practical-application-7e6ee3b1daf0?source=collection_archive---------11-----------------------#2023-11-28](https://towardsdatascience.com/voronoi-grids-a-practical-application-7e6ee3b1daf0?source=collection_archive---------11-----------------------#2023-11-28)

## 快速成功数据科学

## 映射墨尔本，澳大利亚的学校区域

[](https://medium.com/@lee_vaughan?source=post_page-----7e6ee3b1daf0--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----7e6ee3b1daf0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7e6ee3b1daf0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7e6ee3b1daf0--------------------------------) [李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----7e6ee3b1daf0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvoronoi-grids-a-practical-application-7e6ee3b1daf0&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----7e6ee3b1daf0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7e6ee3b1daf0--------------------------------) ·10 分钟阅读·2023年11月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7e6ee3b1daf0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvoronoi-grids-a-practical-application-7e6ee3b1daf0&user=Lee+Vaughan&userId=5d604015c08b&source=-----7e6ee3b1daf0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7e6ee3b1daf0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvoronoi-grids-a-practical-application-7e6ee3b1daf0&source=-----7e6ee3b1daf0---------------------bookmark_footer-----------)![](../Images/749f1c178903f8991aec8a771abd436e.png)

墨尔本作为一个由 Leonardo.ai DreamShaper v7 设想的彩绘玻璃窗

*Voronoi 网格*，也称为 Voronoi *图*，用于将平面划分为围绕给定的一组 *种子点* 的离散区域。对于每个种子点，有一个相应的区域，称为 Voronoi *单元*，其中平面上的所有点都比其他任何点更接近 *该* 种子点。

Voronoi 图在许多领域都有应用，包括计算机科学、地理学、生物学和城市规划。一个特别重要的应用是为需要紧急降落的飞机绘制最近的机场位置。

澳大利亚墨尔本的政府使用这个工具来制作*学校学区*地图。“学区”指的是那些居住在特定区域的学生，并且在特定学校中有保证的位置。由于学生有资格就读于离他们居住地最近的初中或高中——以欧几里得距离测量——学校区域的地图默认就是Voronoi图。

![](../Images/64a322ea08319cfba5b42537ad497b98.png)

墨尔本学校学区地图（[维多利亚州教育部](https://www.findmyschool.vic.gov.au/)，CC-BY 4.0）

在这个*快速成功的数据科学*项目中，我们将探索Voronoi图的概念……
