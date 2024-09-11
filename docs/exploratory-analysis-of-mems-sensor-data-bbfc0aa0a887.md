# MEMS 传感器数据的探索性分析

> 原文：[https://towardsdatascience.com/exploratory-analysis-of-mems-sensor-data-bbfc0aa0a887?source=collection_archive---------5-----------------------#2023-08-19](https://towardsdatascience.com/exploratory-analysis-of-mems-sensor-data-bbfc0aa0a887?source=collection_archive---------5-----------------------#2023-08-19)

## 读取、收集和分析来自 MPU6050 传感器的数据

[](https://dmitryelj.medium.com/?source=post_page-----bbfc0aa0a887--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----bbfc0aa0a887--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bbfc0aa0a887--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bbfc0aa0a887--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----bbfc0aa0a887--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-analysis-of-mems-sensor-data-bbfc0aa0a887&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----bbfc0aa0a887---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bbfc0aa0a887--------------------------------) ·13分钟阅读·2023年8月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbbfc0aa0a887&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-analysis-of-mems-sensor-data-bbfc0aa0a887&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----bbfc0aa0a887---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbbfc0aa0a887&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-analysis-of-mems-sensor-data-bbfc0aa0a887&source=-----bbfc0aa0a887---------------------bookmark_footer-----------)

MEMS（微电机械系统）传感器广泛应用于各种场景，从游戏控制器和智能手机到无人驾驶飞行器。在本文中，我将展示如何连接陀螺仪和加速度计传感器，能够从中获取什么数据，以及如何处理和可视化这些数据。

开始吧。

## 硬件

MPU-6050 是一个6轴传感器，结合了3轴陀螺仪、3轴加速度计和 I2C 接口。正如数据表中所述，它被广泛用于平板电脑和智能手机。当我们的智能手机或智能手表在运动时计算步数和卡路里时，实际上使用了 MEMS 传感器的数据。但像这样的传感器不仅仅可以用于运动。我决定将传感器放置在我的公寓里几天，看看是否能够检测和分析我居住建筑中的不同振动。

如果我们想在几天内收集数据，树莓派是一个很好的解决方案。树莓派是一款便宜（30–50美元）的单板计算机；它功耗低，拥有许多接口可以连接不同类型的硬件。可以在亚马逊上以3–5美元的价格订购一个 MPU-6050 原型板。传感器本身使用 I2C 总线进行数据传输，它只需通过4根线即可连接到树莓派：
