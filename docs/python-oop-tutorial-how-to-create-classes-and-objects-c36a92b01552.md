# Python OOP 教程：如何创建类和对象

> 原文：[`towardsdatascience.com/python-oop-tutorial-how-to-create-classes-and-objects-c36a92b01552?source=collection_archive---------19-----------------------#2023-01-04`](https://towardsdatascience.com/python-oop-tutorial-how-to-create-classes-and-objects-c36a92b01552?source=collection_archive---------19-----------------------#2023-01-04)

## 一份关于在面向对象编程（OOP）中使用类和对象的简易指南

[](https://medium.com/@yazihejazi?source=post_page-----c36a92b01552--------------------------------)![Yasmine Hejazi](https://medium.com/@yazihejazi?source=post_page-----c36a92b01552--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c36a92b01552--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c36a92b01552--------------------------------) [Yasmine Hejazi](https://medium.com/@yazihejazi?source=post_page-----c36a92b01552--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe2d3b1ba6be6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-oop-tutorial-how-to-create-classes-and-objects-c36a92b01552&user=Yasmine+Hejazi&userId=e2d3b1ba6be6&source=post_page-e2d3b1ba6be6----c36a92b01552---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c36a92b01552--------------------------------) ·6 分钟阅读·2023 年 1 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc36a92b01552&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-oop-tutorial-how-to-create-classes-and-objects-c36a92b01552&user=Yasmine+Hejazi&userId=e2d3b1ba6be6&source=-----c36a92b01552---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc36a92b01552&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-oop-tutorial-how-to-create-classes-and-objects-c36a92b01552&source=-----c36a92b01552---------------------bookmark_footer-----------)![](img/0dde12a92beeefc82d69df98b05a3724.png)

照片由 [Taylor Heery](https://unsplash.com/@taylorheeryphoto?utm_source=medium&utm_medium=referral) 拍摄，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在 Python 编程中，一切都是对象。变量甚至函数也是对象。**类是一个模具**，用来创建对象。

*想象一个冰棒模具。* 首先，你制造冰棒模具来创建所需的尺寸、形状和深度；这就是**类**。然后，你可以决定向冰棒模具中倒入什么液体进行冷冻——也许你加水并制作冰块，或者你添加不同种类的水果和果汁来制作冰棒。你创建的每个冰棒就是一个**对象**，而对象可以有不同的“数据”或口味。

本文将通过代码演示如何创建你自己的类并在你的 Python 代码中使用它。类的不同组件可以分解为以下几部分：构造函数、获取器和设置器、属性、装饰器、隐私命名、类方法、属性和继承。

**何时使用类/对象而不是模块：**

+   当你需要多个具有相似行为但不同数据的独立实例时，请使用类。

+   当你希望支持继承时，请使用类；模块则不支持…
