# 使用 Python 可视化卫星图像的 RGB 通道

> 原文：[https://towardsdatascience.com/visualising-the-rgb-channels-of-satellite-images-with-python-6d541af1f98d?source=collection_archive---------6-----------------------#2023-04-11](https://towardsdatascience.com/visualising-the-rgb-channels-of-satellite-images-with-python-6d541af1f98d?source=collection_archive---------6-----------------------#2023-04-11)

## 如何处理在可视化卫星图像时出现的多个光谱波段、大像素值和偏斜的 RGB 通道

[](https://conorosullyds.medium.com/?source=post_page-----6d541af1f98d--------------------------------)[![Conor O'Sullivan](../Images/2dc50a24edb12e843651d01ed48a3c3f.png)](https://conorosullyds.medium.com/?source=post_page-----6d541af1f98d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6d541af1f98d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6d541af1f98d--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----6d541af1f98d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ae48256fb37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualising-the-rgb-channels-of-satellite-images-with-python-6d541af1f98d&user=Conor+O%27Sullivan&userId=4ae48256fb37&source=post_page-4ae48256fb37----6d541af1f98d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d541af1f98d--------------------------------) · 6 分钟阅读 · 2023年4月11日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6d541af1f98d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualising-the-rgb-channels-of-satellite-images-with-python-6d541af1f98d&source=-----6d541af1f98d---------------------bookmark_footer-----------)![](../Images/a9459dc9abec413cf09d12e5685b4600.png)

(来源: [SWED](https://openmldata.ukho.gov.uk/))

卫星图像包含大量信息。缺点是这些信息的可视化并不简单。数据与普通图像不同，因为它们可能具有：

+   **超过 12 个频道**

+   **大的像素值**

+   **偏斜的像素值**

我们将讨论这些关键考虑因素。然后，我们将它们纳入一个 Python 函数，该函数在组合 RGB 通道时为你提供灵活性。具体而言，它允许你调整图像的**亮度**和**色调**。代码已给出，你可以在[GitHub](https://github.com/conorosully/medium-articles/blob/master/src/remote%20sensing/visualise_rgb_bands.ipynb)上找到完整项目。

# 导入和数据集

我们的第一个导入是[地理空间数据抽象库](https://gdal.org/index.html)（gdal）。这在处理遥感数据时非常有用。我们还使用了更多标准的 Python 包（第 4–5 行）。最后，glob 用于处理文件路径（第 7 行）。

```py
# Imports
from osgeo import gdal

import numpy as np
import matplotlib.pyplot as plt

import glob
```
