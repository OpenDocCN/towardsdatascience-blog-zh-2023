# 什么是梯度，为什么梯度会爆炸？

> 原文：[`towardsdatascience.com/what-are-gradients-and-why-do-they-explode-add23264d24b?source=collection_archive---------8-----------------------#2023-06-12`](https://towardsdatascience.com/what-are-gradients-and-why-do-they-explode-add23264d24b?source=collection_archive---------8-----------------------#2023-06-12)

## 阅读这篇文章后，你将对深度学习中最重要的概念有一个扎实的理解。

[](https://medium.com/@danielwarfield1?source=post_page-----add23264d24b--------------------------------)![Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----add23264d24b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----add23264d24b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----add23264d24b--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----add23264d24b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-gradients-and-why-do-they-explode-add23264d24b&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----add23264d24b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----add23264d24b--------------------------------) ·10 分钟阅读·2023 年 6 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fadd23264d24b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-gradients-and-why-do-they-explode-add23264d24b&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----add23264d24b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fadd23264d24b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-gradients-and-why-do-they-explode-add23264d24b&source=-----add23264d24b---------------------bookmark_footer-----------)![](img/bad1bd95b279ce0cae1fa6328cb658d5.png)

“梯度爆炸”，由 MidJourney 制作。所有图片均为作者所用，除非另有说明。

梯度可以说是机器学习中最重要的基本概念。在这篇文章中，我们将探讨梯度的概念，什么因素导致梯度消失和爆炸，以及如何控制它们。

**这对谁有用？** 对于初学者到中级数据科学家非常有用。我会包含一些数学参考资料，这些资料可能对更高级的读者有所帮助。

**你将从这篇文章中获得什么？** 对梯度的深入概念理解，梯度与机器学习的关系，梯度带来的问题以及解决这些问题的方法。

# 目录

点击链接以导航到具体章节

**1)** **什么是梯度？** **2)** **实际梯度（数学上）** **3)** **简单模型中的梯度（示例）** **4)** **什么是梯度爆炸和梯度消失？** **5)** **为什么梯度爆炸和梯度消失很糟糕？** **6)** **我们如何解决梯度爆炸和梯度消失？**

# 什么是梯度？
