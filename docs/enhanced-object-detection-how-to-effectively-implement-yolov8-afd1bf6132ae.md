# 增强的目标检测：如何有效实施 YOLOv8

> 原文：[https://towardsdatascience.com/enhanced-object-detection-how-to-effectively-implement-yolov8-afd1bf6132ae?source=collection_archive---------1-----------------------#2023-03-23](https://towardsdatascience.com/enhanced-object-detection-how-to-effectively-implement-yolov8-afd1bf6132ae?source=collection_archive---------1-----------------------#2023-03-23)

## 这是一本实用指南，介绍如何使用 CLI 和 Python 在图像、视频和实时摄像头视频流中进行目标检测

[](https://thomasdorfer.medium.com/?source=post_page-----afd1bf6132ae--------------------------------)[![Thomas A Dorfer](../Images/9258a1735cee805f1d9b02e2adf01096.png)](https://thomasdorfer.medium.com/?source=post_page-----afd1bf6132ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afd1bf6132ae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----afd1bf6132ae--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----afd1bf6132ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c54f9b62b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhanced-object-detection-how-to-effectively-implement-yolov8-afd1bf6132ae&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=post_page-7c54f9b62b90----afd1bf6132ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----afd1bf6132ae--------------------------------) ·7分钟阅读·2023年3月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fafd1bf6132ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhanced-object-detection-how-to-effectively-implement-yolov8-afd1bf6132ae&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=-----afd1bf6132ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fafd1bf6132ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhanced-object-detection-how-to-effectively-implement-yolov8-afd1bf6132ae&source=-----afd1bf6132ae---------------------bookmark_footer-----------)![](../Images/7648a5890412b71ee953a2a079374602.png)

视频由 [Camilo Calderón](https://www.pexels.com/@camilo-calderon-3343529/) 提供，发布于 [Pexels](https://www.pexels.com/video/a-video-footage-of-busy-street-4997787/)。作者将其转换为 GIF 格式。

# 引言

目标检测是计算机视觉的一个子领域，主要关注于在图像或视频中以一定的置信度识别和定位对象。一个被识别的对象通常会用边界框标注，这为观众提供有关对象性质和在场景中位置的信息。

在2015年，[**YOLO**](https://arxiv.org/abs/1506.02640)的首发，或称为**You Only Look Once**，震撼了计算机视觉领域，因为它的系统能够以惊人的准确性和速度进行实时目标检测。从那时起，YOLO经历了几次迭代的改进，最终推出了其最新成员：由[Ultralytics](https://github.com/ultralytics/ultralytics)发布的**YOLOv8**。

YOLOv8有五个版本：nano (n)、small (s)、medium (m)、large (l) 和 extra large (x)。它们各自的改进可以通过它们的平均精度均值（mAP）和延迟来展示，这些是通过[COCO val2017](http://cocodataset.org/)数据集评估的。
