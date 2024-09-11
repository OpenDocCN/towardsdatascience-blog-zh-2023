# 使用机器学习创建自定义颜色调色板

> 原文：[`towardsdatascience.com/using-machine-learning-to-create-custom-color-palettes-acb4eeaa06aa?source=collection_archive---------10-----------------------#2023-02-16`](https://towardsdatascience.com/using-machine-learning-to-create-custom-color-palettes-acb4eeaa06aa?source=collection_archive---------10-----------------------#2023-02-16)

## 幕后探访 Streamlit 的本月应用

[](https://medium.com/@siavashyasini?source=post_page-----acb4eeaa06aa--------------------------------)![Siavash Yasini](https://medium.com/@siavashyasini?source=post_page-----acb4eeaa06aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----acb4eeaa06aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----acb4eeaa06aa--------------------------------) [Siavash Yasini](https://medium.com/@siavashyasini?source=post_page-----acb4eeaa06aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F17613cac9c65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-machine-learning-to-create-custom-color-palettes-acb4eeaa06aa&user=Siavash+Yasini&userId=17613cac9c65&source=post_page-17613cac9c65----acb4eeaa06aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----acb4eeaa06aa--------------------------------) ·12 分钟阅读·2023 年 2 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Facb4eeaa06aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-machine-learning-to-create-custom-color-palettes-acb4eeaa06aa&user=Siavash+Yasini&userId=17613cac9c65&source=-----acb4eeaa06aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Facb4eeaa06aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-machine-learning-to-create-custom-color-palettes-acb4eeaa06aa&source=-----acb4eeaa06aa---------------------bookmark_footer-----------)![](img/1f09de41ab54258a22eed6764d2b2464.png)

照片由 [Ricardo Gomez Angel](https://unsplash.com/@rgaleriacom?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **简介**

我们都喜欢接触新的数据集，对其进行探索并从中学习。但仅凭原始数据本身并不能讲好故事。我们的原始大脑更倾向于线条、形状和颜色。这就是为什么数字需要被可视化来讲述一个好的故事。

然而，你的数据可视化的颜色调色板可能会影响你的数据故事的成功与否。虽然为你的数据可视化找出完美的颜色组合可能是一个严谨且耗时的任务，但你不必完全依靠自己。从头开始之前，你可以从历史上最伟大的画家和艺术家那里获取灵感。

![](img/d50d19ef7363c358c55970608f26affd.png)

作者提供的图片

从头创建一个颜色调色板通常是可视化工程师和设计师的专长领域，他们使用颜色…
