# 使用Matplotlib创建*3-D*银河艺术

> 原文：[https://towardsdatascience.com/create-3-d-galactic-art-with-matplotlib-a7534148a319?source=collection_archive---------8-----------------------#2023-10-04](https://towardsdatascience.com/create-3-d-galactic-art-with-matplotlib-a7534148a319?source=collection_archive---------8-----------------------#2023-10-04)

## 更多有趣的对数螺旋

[](https://medium.com/@lee_vaughan?source=post_page-----a7534148a319--------------------------------)[![Lee Vaughan](../Images/9f6b90bb76102f438ab0b9a4a62ffa3f.png)](https://medium.com/@lee_vaughan?source=post_page-----a7534148a319--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a7534148a319--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a7534148a319--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----a7534148a319--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-3-d-galactic-art-with-matplotlib-a7534148a319&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----a7534148a319---------------------post_header-----------) 发表在[数据科学前沿](https://towardsdatascience.com/?source=post_page-----a7534148a319--------------------------------) ·11分钟阅读·2023年10月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa7534148a319&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-3-d-galactic-art-with-matplotlib-a7534148a319&user=Lee+Vaughan&userId=5d604015c08b&source=-----a7534148a319---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa7534148a319&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-3-d-galactic-art-with-matplotlib-a7534148a319&source=-----a7534148a319---------------------bookmark_footer-----------)![](../Images/a088a775770e8db4d621ccb4aa4398ed.png)

一个螺旋星系的*3-D*模拟（作者提供）

在[上一篇文章](https://medium.com/towards-data-science/create-galactic-art-with-tkinter-e0418a59b215)中，我展示了如何利用Python的Tkinter GUI模块，使用对数螺旋方程来制作*2-D*银河艺术。在这篇文章中，我们将更进一步，使用Python的主要绘图库matplotlib，制作迷人的*3-D*螺旋星系模拟。除了有趣，这个项目还是向学生展示对数螺旋概念的绝佳方式。

# 对数螺旋的极坐标方程

*对数*螺旋是自然物体几何生长时产生的。例如，银河系、飓风和松果。以银河系为例，这些螺旋由星星密集的*螺旋臂*表示。

因为螺旋从一个中心点或*极点*辐射出来，所以可以使用*极坐标*进行绘图。在这种系统中，熟悉的笛卡尔坐标系中的（*x, y*）坐标被（*r,* Ɵ）所替代，其中 *r* 是距离极点（0, 0）的距离，Ɵ（theta）是从 x 轴逆时针测量的角度。

![](../Images/e69f4225cac1548df350025c279c3e95.png)

示例极坐标系统（来自“《非实用的Python项目》”[2]）
