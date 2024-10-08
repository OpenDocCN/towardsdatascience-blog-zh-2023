# 在 OpenCV Python 中进行简单的边缘检测方法

> 原文：[`towardsdatascience.com/easy-method-of-edge-detection-in-opencv-python-db26972deb2d`](https://towardsdatascience.com/easy-method-of-edge-detection-in-opencv-python-db26972deb2d)

![](img/abf1e43bd4a34fd3dc4c288984cd3c27.png)

图片由[Elijah Hiett](https://unsplash.com/@elijahdhiett?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

## 高效地使用 Canny 边缘检测

[](https://rashida00.medium.com/?source=post_page-----db26972deb2d--------------------------------)![Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----db26972deb2d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db26972deb2d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----db26972deb2d--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----db26972deb2d--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----db26972deb2d--------------------------------) ·阅读时间 4 分钟·2023 年 1 月 24 日

--

边缘检测在图像处理中非常常见且广泛应用，对于许多不同的计算机视觉应用如数据提取、图像分割、更细粒度的特征提取和模式识别等都是必要的。它减少了图像中的噪声和细节，但保留了图像的结构。

Python 中的 Canny 边缘检测是计算机视觉中最流行的边缘检测方法之一。Canny 边缘检测的步骤如下：

1.  使用高斯平滑减少噪声

1.  计算梯度

1.  应用非极大值抑制以减少噪声，并仅保留梯度方向上的局部极大值

1.  寻找上限和下限阈值

1.  应用阈值。

幸运的是，OpenCV 库有`cv2.canny()`函数可以为我们执行 Canny 边缘检测。在本文中，我们将直接进行使用 OpenCV 进行边缘检测。

```py
import cv2 
import matplotlib.pyplot as plt 
```

我们将使用以下图片作为今天教程的素材：

![](img/a6e68579f9b3c323d67b747604d91bf9.png)

图片由[Lucas Hoang](https://unsplash.com/@zuizuii?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

Canny 边缘检测的第一步是应用高斯模糊。在模糊之前，将图像转换为灰度图像也是重要的。

```py
image = cv2.imread("meter.jpg")
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
blurred = cv2.GaussianBlur(gray, (5, 5), 0)
```

现在，我们可以直接将`cv2.Canny()`方法应用到这张模糊的图片上。这很简单。它需要三个参数：图片本身、下限阈值和上限阈值。选择这些阈值是棘手的。对于每张图片，这些阈值会有所不同。

对于这张图像，我们将尝试三种不同的范围并观察会发生什么：

```py
wide = cv2.Canny(blurred, 50, 200)
mid = cv2.Canny(blurred, 30, 150)
tight = cv2.Canny(blurred, 210, 250)
```

在这里，我使用了三种不同类型的范围。在宽范围中，阈值有一个较大的范围。在中等范围中，阈值有一个中等的范围，而在紧范围中，阈值的范围非常小，仅为 210 到 250，相当接近。

为了检查这些图片，我只保存了这三张图片（宽，中和紧）。

这些是结果：

![](img/ec7428967c291adc88640b116a086903.png)

图片由作者提供

这是中等范围的结果：

![](img/246a06fbf33e0400a7dddd79c8d09fe8.png)

图片由作者提供

来自紧范围的图像：

![](img/530c07f3b23b73ffecdcc26453e27099.png)

图片由作者提供

如果我们观察这三张图片，我相信中等范围提供了更为明显的边缘。

> 请记住，你不能对这些范围进行概括。不同的图片可能需要不同的范围。这就是为什么它如此棘手的原因。

好消息是，有一些统计技巧可以用来找到下阈值和上阈值，而不是我们之前看到的试错方法。

这是自动边缘检测的函数：

```py
def auto_canny_edge_detection(image, sigma=0.33):
    md = np.median(image)
    lower_value = int(max(0, (1.0-sigma) * md))
    upper_value = int(min(255, (1.0+sigma) * md))
    return cv2.Canny(image, lower_value, upper_value)
```

在上述函数中，首先从图像数组中找到中位像素值。然后，使用这个中位值和一个常数 sigma 值，你可以找到下阈值和上阈值。在这里使用了 0.33 的 sigma 值。在大多数应用中，0.33 作为 sigma 值效果很好。但在某些情况下，如果它没有给你一个好的结果，可以尝试其他 sigma 值。

这是使用 auto_canny_edge_detection 方法处理我们之前创建的模糊图像的结果：

```py
auto_edge = auto_canny_edge_detection(blurred)
cv2.imwrite("auto.jpg", auto_edge)
```

这是 auto.jpg 的样子：

![](img/651688df8dfe198147dde57f75d4667e.png)

图片由作者提供

正如你所看到的，边缘在这里显得非常清晰，而不需要尝试太多阈值。

## 结论

在我看来，自动边缘检测函数为我们提供了最佳结果。请随意在自己的应用中尝试。这可能非常有用。

欢迎关注我的 [Twitter](https://twitter.com/rashida048) 并点赞我的 [Facebook](https://www.facebook.com/rashida.smith.161) 页面。

## 更多阅读

[](/how-to-perform-image-segmentation-with-thresholding-using-opencv-b2a78abb07ac?source=post_page-----db26972deb2d--------------------------------) ## 如何使用 OpenCV 进行阈值分割

### 简单的、大津法和自适应阈值实现示例

towardsdatascience.com [](/morphological-operations-for-image-preprocessing-in-opencv-in-detail-15fccd1e5745?source=post_page-----db26972deb2d--------------------------------) [## OpenCV 中的形态学操作，详细说明

### 腐蚀、膨胀、开运算、闭运算、形态梯度、顶帽/白帽和黑帽通过示例进行了说明

[## Python 初学者的一些基本图像预处理操作](https://towardsdatascience.com/some-basic-image-preprocessing-operations-for-beginners-in-python-7d297316853b?source=post_page-----db26972deb2d--------------------------------)

### OpenCV 初学者指南：移动或平移、调整大小和裁剪

[## 开发多输出模型的逐步教程](https://towardsdatascience.com/a-step-by-step-tutorial-to-develop-a-multi-output-model-in-tensorflow-ec9f13e5979c?source=post_page-----db26972deb2d--------------------------------)

### 包含完整代码

[## Python Matplotlib 可视化中的一些简单但高级的样式](https://pub.towardsai.net/some-simple-but-advanced-styling-in-pythons-matplotlib-visualization-107f3be56a24?source=post_page-----db26972deb2d--------------------------------)

### 为你的 Python 绘图添加一些额外的风味

[## 详细了解 OpenCV 中的形态学操作](https://towardsdatascience.com/morphological-operations-for-image-preprocessing-in-opencv-in-detail-15fccd1e5745?source=post_page-----db26972deb2d--------------------------------)
