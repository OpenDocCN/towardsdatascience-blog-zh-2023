# 使用NumPy和OpenCV开始计算机视觉（CV-01）

> 原文：[https://towardsdatascience.com/getting-started-with-numpy-and-opencv-for-computer-vision-555f88536f68?source=collection_archive---------5-----------------------#2023-03-15](https://towardsdatascience.com/getting-started-with-numpy-and-opencv-for-computer-vision-555f88536f68?source=collection_archive---------5-----------------------#2023-03-15)

## 用Python开始你的计算机视觉编码之旅

[](https://zubairhossain.medium.com/?source=post_page-----555f88536f68--------------------------------)[![Md. Zubair](../Images/1b983a23226ce7561796fa5b28c00d65.png)](https://zubairhossain.medium.com/?source=post_page-----555f88536f68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----555f88536f68--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----555f88536f68--------------------------------) [Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----555f88536f68--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fdaeaeeea52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-numpy-and-opencv-for-computer-vision-555f88536f68&user=Md.+Zubair&userId=2fdaeaeeea52&source=post_page-2fdaeaeeea52----555f88536f68---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----555f88536f68--------------------------------) ·8分钟阅读·2023年3月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F555f88536f68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-numpy-and-opencv-for-computer-vision-555f88536f68&user=Md.+Zubair&userId=2fdaeaeeea52&source=-----555f88536f68---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F555f88536f68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-numpy-and-opencv-for-computer-vision-555f88536f68&source=-----555f88536f68---------------------bookmark_footer-----------)![](../Images/a1f29b1c045ba48aeb7b58ca7d9aece1.png)

照片由 [Dan Smedley](https://unsplash.com/@nadyeldems?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 动机

我们人类通过视觉系统感知环境和周围事物。人的眼睛、大脑和四肢协同工作，以感知环境并做出相应的动作。一个智能系统可以执行那些需要一定智能的任务。为了执行智能任务，人工视觉系统是计算机的重要组成部分。通常，摄像头和图像用于收集完成工作的必要信息。计算机视觉和图像处理技术帮助我们执行类似于人类的任务，比如图像识别、对象跟踪等。

在计算机视觉中，摄像头作为人类眼睛来捕捉图像，处理器则作为大脑来处理捕获的图像并生成有意义的结果。但人类和计算机之间存在一个基本的差异。人脑是自动工作的，智能是一种天生的能力。相反，计算机没有智能，除非有人工指令（程序）。计算机视觉是提供适当指令的方式，以便它能够与人类视觉系统兼容。但其能力是有限的。
