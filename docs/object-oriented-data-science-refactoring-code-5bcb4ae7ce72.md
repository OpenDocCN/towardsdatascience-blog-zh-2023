# 面向对象的数据科学：重构代码

> 原文：[https://towardsdatascience.com/object-oriented-data-science-refactoring-code-5bcb4ae7ce72?source=collection_archive---------3-----------------------#2023-08-24](https://towardsdatascience.com/object-oriented-data-science-refactoring-code-5bcb4ae7ce72?source=collection_archive---------3-----------------------#2023-08-24)

## 通过高效的代码和Python类提升机器学习模型和数据科学产品。

[莫莉·鲁比](https://medium.com/@molly.ruby?source=post_page-----5bcb4ae7ce72--------------------------------)[![莫莉·鲁比](../Images/2a493bd01057722138857a90035347cd.png)](https://medium.com/@molly.ruby?source=post_page-----5bcb4ae7ce72--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5bcb4ae7ce72--------------------------------)[![走向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5bcb4ae7ce72--------------------------------) [莫莉·鲁比](https://medium.com/@molly.ruby?source=post_page-----5bcb4ae7ce72--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7a38f8e9fb80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fobject-oriented-data-science-refactoring-code-5bcb4ae7ce72&user=Molly+Ruby&userId=7a38f8e9fb80&source=post_page-7a38f8e9fb80----5bcb4ae7ce72---------------------post_header-----------) 发表在 [走向数据科学](https://towardsdatascience.com/?source=post_page-----5bcb4ae7ce72--------------------------------) ·7 min read·Aug 24, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5bcb4ae7ce72&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fobject-oriented-data-science-refactoring-code-5bcb4ae7ce72&user=Molly+Ruby&userId=7a38f8e9fb80&source=-----5bcb4ae7ce72---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5bcb4ae7ce72&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fobject-oriented-data-science-refactoring-code-5bcb4ae7ce72&source=-----5bcb4ae7ce72---------------------bookmark_footer-----------)![](../Images/5a7acfc2f7f532c4ce9e966a929810ad.png)

作者创建的图片。

对于数据科学家来说，代码是分析和决策的核心。随着数据科学应用变得越来越复杂，从嵌入在软件中的机器学习模型到协调大量信息的复杂数据管道，开发清晰、有序且易于维护的代码变得至关重要。面向对象编程（OOP）解锁了灵活性和效率，使数据科学家能够灵活应对不断变化的需求。OOP引入了类的概念，这些类作为创建对象的蓝图，封装了数据以及操作这些数据的操作。这一范式转变使数据科学家能够超越传统的函数方法，促进模块化设计和代码重用。

在本文中，我们将探讨通过创建类和采用面向对象技术来重构数据科学代码的好处，以及这种方法如何提升模块化和重用性。

# 面向数据科学的类的力量

在传统的数据科学工作流中，函数一直是封装逻辑的方法。这通常是足够的，因为…
