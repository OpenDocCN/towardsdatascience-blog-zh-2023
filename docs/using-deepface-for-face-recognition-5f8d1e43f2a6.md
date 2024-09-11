# 使用 DeepFace 进行面部识别

> 原文：[https://towardsdatascience.com/using-deepface-for-face-recognition-5f8d1e43f2a6?source=collection_archive---------3-----------------------#2023-04-18](https://towardsdatascience.com/using-deepface-for-face-recognition-5f8d1e43f2a6?source=collection_archive---------3-----------------------#2023-04-18)

## 了解如何在不需要训练自己模型的情况下进行面部识别

[![Wei-Meng Lee](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----5f8d1e43f2a6--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5f8d1e43f2a6--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----5f8d1e43f2a6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-deepface-for-face-recognition-5f8d1e43f2a6&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----5f8d1e43f2a6---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5f8d1e43f2a6--------------------------------) · 10 min 阅读 · 2023年4月18日

--

[![](../Images/be2885cc45ece3179a64488ba98914e9.png)](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5f8d1e43f2a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-deepface-for-face-recognition-5f8d1e43f2a6&source=-----5f8d1e43f2a6---------------------bookmark_footer-----------)

图片来源 [fran innocenti](https://unsplash.com/es/@frani?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我之前的文章中，我讨论了如何利用 OpenCV 在你的网络摄像头中检测面部：

[## 使用 Python 进行面部检测 — 面部识别的前奏](https://towardsdatascience.com/face-detection-using-python-the-precursor-to-face-recognition-316ded4d116f?source=post_page-----5f8d1e43f2a6--------------------------------)

### 通过使用你的网络摄像头进行面部检测，享受 Python 的乐趣

[面部检测使用 Python — 面部识别的前奏](https://towardsdatascience.com/face-detection-using-python-the-precursor-to-face-recognition-316ded4d116f?source=post_page-----5f8d1e43f2a6--------------------------------)

检测面孔通常是您执行的第一步，接着是*人脸识别*。

> 人脸识别是指从数字图像或视频帧中匹配人类面孔与面部数据库的过程。

有几种深度学习模型可用于进行人脸识别，但所有这些都要求您具有神经网络的一些知识，并且您还需要使用自己的数据集对其进行训练。对于想要进行人脸识别但不想深入了解神经网络工作原理的人，有一个可以简化人脸识别的 API — **DeepFace**。

# 什么是 DeepFace？
