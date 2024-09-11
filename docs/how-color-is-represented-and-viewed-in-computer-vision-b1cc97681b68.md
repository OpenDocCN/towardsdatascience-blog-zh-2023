# 计算机视觉中的颜色表示综合指南 (CV-02)

> 原文：[`towardsdatascience.com/how-color-is-represented-and-viewed-in-computer-vision-b1cc97681b68?source=collection_archive---------12-----------------------#2023-04-03`](https://towardsdatascience.com/how-color-is-represented-and-viewed-in-computer-vision-b1cc97681b68?source=collection_archive---------12-----------------------#2023-04-03)

## 颜色空间和颜色模型的详细解释

[](https://zubairhossain.medium.com/?source=post_page-----b1cc97681b68--------------------------------)![Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----b1cc97681b68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b1cc97681b68--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1cc97681b68--------------------------------) [Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----b1cc97681b68--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fdaeaeeea52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-color-is-represented-and-viewed-in-computer-vision-b1cc97681b68&user=Md.+Zubair&userId=2fdaeaeeea52&source=post_page-2fdaeaeeea52----b1cc97681b68---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1cc97681b68--------------------------------) ·7 min read·2023 年 4 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb1cc97681b68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-color-is-represented-and-viewed-in-computer-vision-b1cc97681b68&user=Md.+Zubair&userId=2fdaeaeeea52&source=-----b1cc97681b68---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb1cc97681b68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-color-is-represented-and-viewed-in-computer-vision-b1cc97681b68&source=-----b1cc97681b68---------------------bookmark_footer-----------)![](img/75cba015e48aa64937b4f3ffb861747e.png)

照片由[Lucas Kapla](https://unsplash.com/@aznbokchoy?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 动机

> *我们如何感知一个物体？人类和计算机视觉系统的工作方式相同吗？有多少种颜色？我们是否知道或想知道这些问题的所有答案？如果答案是‘是’，那么这篇文章将是最适合你的。*

眼睛是创造者们创造的美丽作品，它能够以令人愉悦和和谐的方式感知物体的颜色。颜色是一种基于电磁波谱的视觉感知[1]。动物视觉系统检测物体波长的吸收，不同的吸收产生不同的颜色。颜色的数量可能是无限的。但我们假设大约存在 1000 万种颜色[2]。光线是产生颜色的必需条件。在没有光的情况下，一切看起来都是黑暗的。想象一下你被锁在一个黑暗的房间里。感觉如何？所以，没有光，世界是没有意义的。光也为我们的生活带来了魅力。

计算机不能像人类那样感知物体。计算机生成和识别颜色是通过一些标准的颜色空间（表示颜色的方式）。
