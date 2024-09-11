# 计算机视觉中的**前向映射**与**后向映射**

> 原文：[`towardsdatascience.com/forward-and-backward-mapping-for-computer-vision-833436e2472?source=collection_archive---------6-----------------------#2023-05-25`](https://towardsdatascience.com/forward-and-backward-mapping-for-computer-vision-833436e2472?source=collection_archive---------6-----------------------#2023-05-25)

## 前向映射和后向映射应用于图像转换

[](https://medium.com/@JavierMtz5?source=post_page-----833436e2472--------------------------------)![Javier Martínez Ojeda](https://medium.com/@JavierMtz5?source=post_page-----833436e2472--------------------------------)[](https://towardsdatascience.com/?source=post_page-----833436e2472--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----833436e2472--------------------------------) [Javier Martínez Ojeda](https://medium.com/@JavierMtz5?source=post_page-----833436e2472--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74d7213a71a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforward-and-backward-mapping-for-computer-vision-833436e2472&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=post_page-74d7213a71a8----833436e2472---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----833436e2472--------------------------------) ·8 分钟阅读·2023 年 5 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F833436e2472&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforward-and-backward-mapping-for-computer-vision-833436e2472&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=-----833436e2472---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F833436e2472&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforward-and-backward-mapping-for-computer-vision-833436e2472&source=-----833436e2472---------------------bookmark_footer-----------)![](img/70e8ea23d3113c4bc92f03ce050b98d4.png)

照片由 [Vadim Bogulov](https://unsplash.com/@franku84?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 如果你想在没有 Premium Medium 账户的情况下阅读这篇文章，你可以通过这个朋友链接来阅读 :)
> 
> [`www.learnml.wiki/forward-and-backward-mapping-for-computer-vision/`](https://www.learnml.wiki/forward-and-backward-mapping-for-computer-vision/)

在本文中，将介绍和解释两种图像变形算法：**前向映射**和**后向映射**。除了在理论层面介绍这些算法之外，还将把它们应用于实际图像，以查看每种算法的结果和能力。

为了充分理解本文所解释的内容，有必要熟悉二维变换矩阵，这些内容在[上一篇文章](https://medium.com/@JavierMtz5/2d-matrix-transformations-for-computer-vision-80b4a4f2120f)中已介绍和解释。

[](https://medium.com/@JavierMtz5/2d-matrix-transformations-for-computer-vision-80b4a4f2120f?source=post_page-----833436e2472--------------------------------) [## 计算机视觉中的二维矩阵变换

### 通过变换矩阵实现的计算机视觉中的缩放、旋转和位移

medium.com](https://medium.com/@JavierMtz5/2d-matrix-transformations-for-computer-vision-80b4a4f2120f?source=post_page-----833436e2472--------------------------------)

# 介绍

如前一篇文章所述，对图像应用变换的方法是迭代图像的每一个像素，并对每一个像素单独应用变换。然而，还有…
