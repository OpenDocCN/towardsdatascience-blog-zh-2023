# 使用 Tkinter 创建银河艺术

> 原文：[`towardsdatascience.com/create-galactic-art-with-tkinter-e0418a59b215?source=collection_archive---------4-----------------------#2023-09-30`](https://towardsdatascience.com/create-galactic-art-with-tkinter-e0418a59b215?source=collection_archive---------4-----------------------#2023-09-30)

## 用对数螺旋模拟大自然

[](https://medium.com/@lee_vaughan?source=post_page-----e0418a59b215--------------------------------)![李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----e0418a59b215--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e0418a59b215--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e0418a59b215--------------------------------) [李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----e0418a59b215--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d604015c08b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-galactic-art-with-tkinter-e0418a59b215&user=Lee+Vaughan&userId=5d604015c08b&source=post_page-5d604015c08b----e0418a59b215---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e0418a59b215--------------------------------) ·11 分钟阅读·2023 年 9 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe0418a59b215&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-galactic-art-with-tkinter-e0418a59b215&user=Lee+Vaughan&userId=5d604015c08b&source=-----e0418a59b215---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe0418a59b215&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-galactic-art-with-tkinter-e0418a59b215&source=-----e0418a59b215---------------------bookmark_footer-----------)![](img/fa440313af850b9e4e0de5318f23ce2e.png)

一个螺旋星系的模型（作者提供）

我们世界的奇迹之一是它可以用数学来描述。这种联系如此强烈，以至于麻省理工学院的物理学家马克斯·泰格马克相信，宇宙不仅仅是*用数学描述*的，而是*就是*数学，从这个意义上说，我们都是一个巨大数学对象的部分。[1]

这意味着许多看似复杂的对象——跨越令人难以置信的尺度——可以被简化为简单的方程。为什么飓风看起来像一个银河系？为什么鹦鹉螺壳中的图案在松果中重复？答案是数学。

![](img/2879ee1043c113f3360e152192001e61.png)

自然界中的对数螺旋实例（摘自《Python 工具为科学家》[2]）

除了它们的外观，上面图片中的对象还有一个共同点：它们都*生长*，而自然界中的生长是*几何级数*。几何增长的螺旋被称为*对数螺旋*，因为描述它们的方程中使用了自然对数的底数（*e*）。虽然通常被称为*对数螺旋*，但它们在自然界中的普遍性使得它们获得了另一个称号：*奇迹螺旋*。

在这个*快速成功的数据科学*项目中，我们将使用对数螺旋和 Python 的 *Tkinter* GUI 模块来…
