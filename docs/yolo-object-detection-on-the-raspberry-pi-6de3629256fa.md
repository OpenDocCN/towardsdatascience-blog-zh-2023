# YOLO在树莓派上的目标检测

> 原文：[https://towardsdatascience.com/yolo-object-detection-on-the-raspberry-pi-6de3629256fa?source=collection_archive---------3-----------------------#2023-07-11](https://towardsdatascience.com/yolo-object-detection-on-the-raspberry-pi-6de3629256fa?source=collection_archive---------3-----------------------#2023-07-11)

## 在低功耗设备上运行目标检测模型

[](https://dmitryelj.medium.com/?source=post_page-----6de3629256fa--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----6de3629256fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6de3629256fa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6de3629256fa--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----6de3629256fa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyolo-object-detection-on-the-raspberry-pi-6de3629256fa&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----6de3629256fa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6de3629256fa--------------------------------) ·9分钟阅读·2023年7月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6de3629256fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyolo-object-detection-on-the-raspberry-pi-6de3629256fa&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----6de3629256fa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6de3629256fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyolo-object-detection-on-the-raspberry-pi-6de3629256fa&source=-----6de3629256fa---------------------bookmark_footer-----------)![](../Images/3cd8e63bae4b714e54aa6398a1a9e5c3.png)

YOLO目标检测结果，图片由作者提供

在这篇文章的[第一部分](/retro-data-science-testing-the-first-versions-of-yolo-799b9c1835d7)中，我测试了“复古”版本的YOLO（You Only Look Once），这是一个流行的目标检测库。仅使用OpenCV而不依赖“沉重”的框架如PyTorch或Keras来运行深度学习模型，对于低功耗设备来说非常有前景，因此我决定深入探讨这一主题，看看最新的YOLO v8模型在树莓派上的表现如何。

让我们开始吧。

## 硬件

在云端运行任何模型通常不会成问题，因为资源几乎是无限的。但对于“现场”的硬件来说，约束条件则要多得多。有限的RAM、CPU性能，甚至不同的CPU架构、旧版或不兼容的软件版本、缺乏高速互联网连接等等。云基础设施的另一个大问题是其成本。假设我们正在制作一个智能门铃，并且我们希望为其添加人员检测功能。我们可以在云端运行一个模型，但每次API调用都会花费金钱，那么谁来支付这些费用呢？不是每个客户都会愿意为门铃或其他类似的“智能”设备支付每月订阅费，因此即使结果可能不那么理想，能够在本地运行模型也是至关重要的。
