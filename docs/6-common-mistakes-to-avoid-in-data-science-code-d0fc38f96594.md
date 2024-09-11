# 数据科学代码中需要避免的 6 个常见错误

> 原文：[`towardsdatascience.com/6-common-mistakes-to-avoid-in-data-science-code-d0fc38f96594?source=collection_archive---------2-----------------------#2023-12-21`](https://towardsdatascience.com/6-common-mistakes-to-avoid-in-data-science-code-d0fc38f96594?source=collection_archive---------2-----------------------#2023-12-21)

## 以及如何克服它们

[](https://khuyentran1476.medium.com/?source=post_page-----d0fc38f96594--------------------------------)![Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----d0fc38f96594--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d0fc38f96594--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d0fc38f96594--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----d0fc38f96594--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-common-mistakes-to-avoid-in-data-science-code-d0fc38f96594&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----d0fc38f96594---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d0fc38f96594--------------------------------) ·10 min 阅读·2023 年 12 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd0fc38f96594&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-common-mistakes-to-avoid-in-data-science-code-d0fc38f96594&user=Khuyen+Tran&userId=84a02493194a&source=-----d0fc38f96594---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd0fc38f96594&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F6-common-mistakes-to-avoid-in-data-science-code-d0fc38f96594&source=-----d0fc38f96594---------------------bookmark_footer-----------)

# 激励

数据科学家常在迭代和探索性的环境中工作。因此，往往更注重快速结果，而不是创建可维护或可扩展的代码。

然而，数据科学家必须避免编写不良代码，原因如下：

1.  **降低代码可读性：** 编写不良的代码可能难以阅读和理解，使得原作者和其他团队成员在未来维护或修改代码时更加困难。

1.  **增加引入错误的可能性：** 结构不佳或效率低下的代码更容易出错，可能影响分析或模型的准确性。

1.  **集成挑战：** 编写不良的代码会阻碍与生产系统的集成以及与其他团队成员（包括数据工程师和机器学习工程师）的交接。

在数据科学项目中编写更好的代码至关重要，必须识别并解决常见的不良实践，其中可能包括：

1.  过度使用 Jupyter Notebooks

1.  模糊的变量名称

1.  冗余代码
