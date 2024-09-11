# 使用公共表表达式改善你的SQL逻辑

> 原文：[https://towardsdatascience.com/improving-your-sql-logic-with-common-table-expressions-26f04967fc86?source=collection_archive---------3-----------------------#2023-12-05](https://towardsdatascience.com/improving-your-sql-logic-with-common-table-expressions-26f04967fc86?source=collection_archive---------3-----------------------#2023-12-05)

## 让逻辑变得更容易编写、阅读、解释和维护

[](https://medium.com/@elliottstam?source=post_page-----26f04967fc86--------------------------------)[![Elliott Stam](../Images/5469da79b12b9d5842df36d381415033.png)](https://medium.com/@elliottstam?source=post_page-----26f04967fc86--------------------------------)[](https://towardsdatascience.com/?source=post_page-----26f04967fc86--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----26f04967fc86--------------------------------) [Elliott Stam](https://medium.com/@elliottstam?source=post_page-----26f04967fc86--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd193f11526c9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-your-sql-logic-with-common-table-expressions-26f04967fc86&user=Elliott+Stam&userId=d193f11526c9&source=post_page-d193f11526c9----26f04967fc86---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----26f04967fc86--------------------------------) ·6分钟阅读·2023年12月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F26f04967fc86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-your-sql-logic-with-common-table-expressions-26f04967fc86&user=Elliott+Stam&userId=d193f11526c9&source=-----26f04967fc86---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F26f04967fc86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-your-sql-logic-with-common-table-expressions-26f04967fc86&source=-----26f04967fc86---------------------bookmark_footer-----------)![](../Images/6a63bf368229748a912d02f23f3d9489.png)

图片由 [Sam Moghadam Khamseh](https://unsplash.com/@sammoghadamkhamseh?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

把你的脏衣服堆成一堆，然后希望最好的结果，这显然不是管理衣物的长期策略。这会很难：

稍后使用`SELECT socks FROM pile`。

你肯定会很难：

当你需要一件衬衫时，可以使用`SELECT shirt FROM pile WHERE wrinkles = false`。

与管理你的衣物一样，积极管理SQL逻辑的组织方式有明显的好处。

在这篇文章中，我们将探讨通用表表达式（CTEs）如何提升你的SQL逻辑。意大利面条代码很少是优质的代码，这一点在SQL中也同样适用。在数据管理和工程中，没有万灵药。然而，保持逻辑的组织性和易于审查只会对你有帮助。那么，让我们深入了解吧！

## 一个通用表表达式是什么样的？

我有时称它们为`WITH`子句。为什么？让我们看一个例子：
