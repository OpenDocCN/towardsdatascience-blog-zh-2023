# 用两行代码实现你的Python程序的线程化

> 原文：[https://towardsdatascience.com/thread-your-python-program-with-two-lines-of-code-3b474407dbb8?source=collection_archive---------17-----------------------#2023-01-10](https://towardsdatascience.com/thread-your-python-program-with-two-lines-of-code-3b474407dbb8?source=collection_archive---------17-----------------------#2023-01-10)

## 通过同时执行多个任务来加快程序的运行速度

[](https://mikehuls.medium.com/?source=post_page-----3b474407dbb8--------------------------------)[![Mike Huls](../Images/8f9f55a0d25db00799c5d37383b7f5b6.png)](https://mikehuls.medium.com/?source=post_page-----3b474407dbb8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3b474407dbb8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3b474407dbb8--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----3b474407dbb8--------------------------------)

·

[阅读原文](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthread-your-python-program-with-two-lines-of-code-3b474407dbb8&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----3b474407dbb8---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b474407dbb8--------------------------------) ·8分钟阅读·2023年1月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3b474407dbb8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthread-your-python-program-with-two-lines-of-code-3b474407dbb8&user=Mike+Huls&userId=7ffb62c607ee&source=-----3b474407dbb8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3b474407dbb8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthread-your-python-program-with-two-lines-of-code-3b474407dbb8&source=-----3b474407dbb8---------------------bookmark_footer-----------)![](../Images/a45a0f3dc3164d2dedb79a814dca0124.png)

更好地组织我们的线程（图片由 [Karen Penroz](https://unsplash.com/@penrosekaren) 提供，来源于 [Unsplash](https://unsplash.com/photos/06ZTGDcAQFs)）

当你的程序有很多任务需要等待时，你可以通过同时执行这些任务来加快程序的运行速度，而不是一个一个地执行。当你做早餐时，你不会在咖啡机煮好咖啡之前再去煮鸡蛋。相反，你会启动咖啡机，同时给自己倒一杯橙汁，并加热煎锅来煎炒鸡蛋。

本文会准确展示如何做到这一点。到最后，你将能够在**两行代码中安全地应用线程**并在你的程序中实现**巨大的加速**。开始编码吧！

# 但首先……

本文将详细介绍如何通过对整组参数应用相同的函数来应用线程。接着，我们将探索如何以线程方式应用不同的函数。

[](/cython-for-absolute-beginners-30x-faster-code-in-two-simple-steps-bbb6c10d06ad?source=post_page-----3b474407dbb8--------------------------------) [## Cython 完全入门：两步实现30倍速度提升

### 快速应用程序的简易 Python 代码编译

[towardsdatascience.com](/cython-for-absolute-beginners-30x-faster-code-in-two-simple-steps-bbb6c10d06ad?source=post_page-----3b474407dbb8--------------------------------)

## 线程会解决我的问题吗？了解并发
