# 使用 Python 和 OpenCV 进行图像标注

> 原文：[`towardsdatascience.com/performing-image-annotation-using-python-and-opencv-f0124746613c?source=collection_archive---------3-----------------------#2023-04-27`](https://towardsdatascience.com/performing-image-annotation-using-python-and-opencv-f0124746613c?source=collection_archive---------3-----------------------#2023-04-27)

## 了解如何为你的图像创建边界框

[](https://weimenglee.medium.com/?source=post_page-----f0124746613c--------------------------------)![Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----f0124746613c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0124746613c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0124746613c--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----f0124746613c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperforming-image-annotation-using-python-and-opencv-f0124746613c&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----f0124746613c---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0124746613c--------------------------------) ·6 分钟阅读·2023 年 4 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff0124746613c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperforming-image-annotation-using-python-and-opencv-f0124746613c&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----f0124746613c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff0124746613c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperforming-image-annotation-using-python-and-opencv-f0124746613c&source=-----f0124746613c---------------------bookmark_footer-----------)![](img/36465bcf9bbb42baa466e690d7543696.png)

照片由[Héctor J. Rivas](https://unsplash.com/es/@hjrc33?utm_source=medium&utm_medium=referral)提供，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

深度学习中的常见任务之一是***目标检测***，这是一个在给定图像中定位特定对象的过程。目标检测的一个示例是检测图像中的汽车，你可以统计图像中检测到的汽车总数。这在需要分析特定交叉口的交通流量时可能会很有用。

为了训练深度学习模型以检测特定对象，您需要向模型提供一组训练图像，并在图像中标出特定对象的坐标。这个过程被称为***图像标注***。图像标注为图像中存在的对象分配标签，并将所有对象标出。

在本文中，我将展示如何使用 Python 和 OpenCV 来标注您的图像 — 您将使用鼠标标出要标注的对象，并且应用程序将在对象周围绘制一个边界框。然后您可以看到您标注的对象的坐标，并可选择将其保存到日志文件中。

# 使用 OpenCV 显示图像

首先，创建一个文本文件，命名为**bounding.py**。然后，填入以下语句：
