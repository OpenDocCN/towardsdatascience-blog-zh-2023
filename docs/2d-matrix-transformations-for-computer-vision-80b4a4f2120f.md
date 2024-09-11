# 计算机视觉中的二维矩阵变换

> 原文：[`towardsdatascience.com/2d-matrix-transformations-for-computer-vision-80b4a4f2120f?source=collection_archive---------18-----------------------#2023-05-23`](https://towardsdatascience.com/2d-matrix-transformations-for-computer-vision-80b4a4f2120f?source=collection_archive---------18-----------------------#2023-05-23)

## 计算机视觉中的缩放、旋转和通过变换矩阵进行平移

[](https://medium.com/@JavierMtz5?source=post_page-----80b4a4f2120f--------------------------------)![Javier Martínez Ojeda](https://medium.com/@JavierMtz5?source=post_page-----80b4a4f2120f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----80b4a4f2120f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----80b4a4f2120f--------------------------------) [Javier Martínez Ojeda](https://medium.com/@JavierMtz5?source=post_page-----80b4a4f2120f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74d7213a71a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2d-matrix-transformations-for-computer-vision-80b4a4f2120f&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=post_page-74d7213a71a8----80b4a4f2120f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----80b4a4f2120f--------------------------------) ·7 min read·2023 年 5 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F80b4a4f2120f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2d-matrix-transformations-for-computer-vision-80b4a4f2120f&user=Javier+Mart%C3%ADnez+Ojeda&userId=74d7213a71a8&source=-----80b4a4f2120f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F80b4a4f2120f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2d-matrix-transformations-for-computer-vision-80b4a4f2120f&source=-----80b4a4f2120f---------------------bookmark_footer-----------)![](img/53cd95c16308884aad48a8493ac3d1c4.png)

图片由 [Elen Sher](https://unsplash.com/@lenochka210292?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 如果你想在没有 Premium Medium 账户的情况下阅读这篇文章，你可以通过这个朋友链接 :)
> 
> [`www.learnml.wiki/2d-matrix-transformations-for-computer-vision/`](https://www.learnml.wiki/2d-matrix-transformations-for-computer-vision/)

在计算机视觉中，经常需要对图像进行缩放、平移或旋转。诸如 OpenCV 的库可以轻松执行这些操作，然而，理解这些变换在低层次上如何工作对于能够应用更复杂的变换以及优化使用计算机视觉库至关重要。因此，本文将解释三种最简单的**2D 刚体变换**类型：**缩放**、**旋转**和**平移**，并提供它们应用的示例。

# 引言

当你想要对图像进行缩放、旋转或平移时，即时的解决方案是将这些变换应用到每个像素上。通过对所有像素应用相同的变换，变换后的像素将创建一个新图像，即为原始图像经过变换后的结果。

这可以在下面的图像中看到，我们有一个由 4 个像素/顶点形成的正方形。当我们想要围绕…（原文接下来内容已省略）
