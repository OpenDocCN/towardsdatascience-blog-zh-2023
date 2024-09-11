# **《Python 类别轻松入门：面向对象编程的权威指南》**

> 原文：[https://towardsdatascience.com/python-classes-made-easy-the-definitive-guide-to-object-oriented-programming-881ed609fb6?source=collection_archive---------3-----------------------#2023-03-13](https://towardsdatascience.com/python-classes-made-easy-the-definitive-guide-to-object-oriented-programming-881ed609fb6?source=collection_archive---------3-----------------------#2023-03-13)

## 提升你的Python技能，借助这本全面的类别参考书

[](https://federicotrotta.medium.com/?source=post_page-----881ed609fb6--------------------------------)[![Federico Trotta](../Images/e997e3a96940c16ab5071629016d82fd.png)](https://federicotrotta.medium.com/?source=post_page-----881ed609fb6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----881ed609fb6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----881ed609fb6--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----881ed609fb6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-classes-made-easy-the-definitive-guide-to-object-oriented-programming-881ed609fb6&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----881ed609fb6---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----881ed609fb6--------------------------------) ·18分钟阅读·2023年3月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F881ed609fb6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-classes-made-easy-the-definitive-guide-to-object-oriented-programming-881ed609fb6&user=Federico+Trotta&userId=654cd4bbe899&source=-----881ed609fb6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F881ed609fb6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-classes-made-easy-the-definitive-guide-to-object-oriented-programming-881ed609fb6&source=-----881ed609fb6---------------------bookmark_footer-----------)![](../Images/243dd67262bbc8d50ada6050e9cb8725.png)

图片由[Lukas Bieri](https://pixabay.com/it/users/lukasbieri-4664461/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2838945)提供，发布在[Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2838945)。

当谈到类时，许多 Python 开发者面临困难，原因有很多。首先——在我看来——因为面向对象编程的概念并不总是明确的。其次，因为类和面向对象编程（OOP）背后的思想很多，我们可能在这里和那里（主要是在线上）找到的解释可能比较肤浅。

在本文中，我想介绍 Python 类的最重要概念，以及如何使用它们（带有编码示例）。

但首先，我们将从讨论面向对象编程开始这篇文章。

```py
**Table of Contents**

[Object Oriented Programming](#2502)
[Classes in Python](#aa37)
  The "self" Parameter
  The "__init__" Method
  if __name__ == "__main__"
[Type Hints](#fcad)
[Docstrings (and how to invoke them](#d8d0))
[Inheritance](#f2a2)
[Pro tip on how to use Python classes](#a506)
```

# 面向对象编程

引用和改述参考文献[1]，我们可以说，作为人类，我们完全清楚物体是什么：它们是所有可以用我们的感官感受到并且可以操控的有形事物。在我们的成长过程中，我们学会了...
