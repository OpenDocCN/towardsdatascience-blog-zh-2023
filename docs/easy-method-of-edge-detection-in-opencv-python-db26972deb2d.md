# OpenCV Python 中的简单边缘检测方法

> 原文：[`towardsdatascience.com/easy-method-of-edge-detection-in-opencv-python-db26972deb2d?source=collection_archive---------3-----------------------#2023-01-24`](https://towardsdatascience.com/easy-method-of-edge-detection-in-opencv-python-db26972deb2d?source=collection_archive---------3-----------------------#2023-01-24)

![](img/abf1e43bd4a34fd3dc4c288984cd3c27.png)

图片由 [Elijah Hiett](https://unsplash.com/@elijahdhiett?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 高效使用 Canny 边缘检测

[](https://rashida00.medium.com/?source=post_page-----db26972deb2d--------------------------------)![Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----db26972deb2d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db26972deb2d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----db26972deb2d--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----db26972deb2d--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a36b941a136&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasy-method-of-edge-detection-in-opencv-python-db26972deb2d&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=post_page-8a36b941a136----db26972deb2d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----db26972deb2d--------------------------------) ·4 分钟阅读·2023 年 1 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdb26972deb2d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasy-method-of-edge-detection-in-opencv-python-db26972deb2d&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=-----db26972deb2d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdb26972deb2d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasy-method-of-edge-detection-in-opencv-python-db26972deb2d&source=-----db26972deb2d---------------------bookmark_footer-----------)

边缘检测在计算机视觉应用中非常常见且广泛使用，例如数据提取、图像分割、以及更细粒度的特征提取和模式识别。它减少了图像中的噪声和细节数量，但保留了图像的结构。

在 Python 中，Canny 边缘检测是计算机视觉中最受欢迎的边缘检测方法之一。以下是 Canny 边缘检测的步骤：

1.  使用高斯平滑减少噪声。

1.  计算梯度。

1.  应用非极大值抑制来减少噪声，并仅保留梯度方向上的局部极大值。

1.  找到上阈值和下阈值。

1.  应用阈值。

幸运的是，OpenCV 库提供了 `cv2.canny()` 函数来为我们执行 Canny 边缘检测。在本文中，我们将直接进行使用 OpenCV 执行边缘检测。

```py
import cv2 
import matplotlib.pyplot as plt 
```

我们将使用下面的图片进行今天的教程：
