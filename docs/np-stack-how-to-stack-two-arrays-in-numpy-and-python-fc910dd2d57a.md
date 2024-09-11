# np.stack() — 如何在 Numpy 和 Python 中堆叠两个数组

> 原文：[https://towardsdatascience.com/np-stack-how-to-stack-two-arrays-in-numpy-and-python-fc910dd2d57a?source=collection_archive---------32-----------------------#2023-01-10](https://towardsdatascience.com/np-stack-how-to-stack-two-arrays-in-numpy-and-python-fc910dd2d57a?source=collection_archive---------32-----------------------#2023-01-10)

## **初学者和高级的 Numpy 堆叠示例 — 学习如何轻松地连接数组序列**

[](https://medium.com/@radecicdario?source=post_page-----fc910dd2d57a--------------------------------)[![Dario Radečić](../Images/41882a3b30bab9da43d66a59f1df366b.png)](https://medium.com/@radecicdario?source=post_page-----fc910dd2d57a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc910dd2d57a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fc910dd2d57a--------------------------------) [Dario Radečić](https://medium.com/@radecicdario?source=post_page-----fc910dd2d57a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F689ba04bb8be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnp-stack-how-to-stack-two-arrays-in-numpy-and-python-fc910dd2d57a&user=Dario+Rade%C4%8Di%C4%87&userId=689ba04bb8be&source=post_page-689ba04bb8be----fc910dd2d57a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc910dd2d57a--------------------------------) · 阅读时间7分钟 · 2023年1月10日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffc910dd2d57a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnp-stack-how-to-stack-two-arrays-in-numpy-and-python-fc910dd2d57a&source=-----fc910dd2d57a---------------------bookmark_footer-----------)![](../Images/cc7b5581c52b6ae2e275765250b5f008.png)

图片由 [Brigitte Tohm](https://unsplash.com/@brigittetohm?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Numpy 是一个用于数据科学和机器学习的惊人库，如果你想成为数据专业人士，这是必不可少的。掌握这个库的方方面面是必须的，因为没有必要重新发明轮子——几乎你能想到的任何功能已经被实现了。

今天你将学习关于np stack——或者说Numpy的`stack()`函数。简单来说，它允许你根据指定的参数值将数组按行（默认）或按列连接起来。我们将深入探讨基础知识和函数签名，然后跳入Python中的示例。

# 什么是np stack？

Numpy的np stack函数用于沿新轴堆叠/连接数组。它将返回一个单一数组，作为堆叠多个具有相同形状的序列的结果。你也可以堆叠多维数组，你将很快学到这一点。

但首先，让我们解释一下水平堆叠和垂直堆叠之间的区别。

## 水平与垂直堆叠在Numpy中的区别
