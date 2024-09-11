# 作为混合整数规划（MIP）模型的热启动启发式方法

> 原文：[`towardsdatascience.com/heuristics-as-warm-start-for-mixed-integer-programming-mip-models-9046781dd21f?source=collection_archive---------14-----------------------#2023-04-06`](https://towardsdatascience.com/heuristics-as-warm-start-for-mixed-integer-programming-mip-models-9046781dd21f?source=collection_archive---------14-----------------------#2023-04-06)

## 在 MIP 模型中设置初始解：一个调度应用

[](https://medium.com/@bernardovf?source=post_page-----9046781dd21f--------------------------------)![Bernardo Furtado](https://medium.com/@bernardovf?source=post_page-----9046781dd21f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9046781dd21f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9046781dd21f--------------------------------) [Bernardo Furtado](https://medium.com/@bernardovf?source=post_page-----9046781dd21f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F220a6eda5891&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fheuristics-as-warm-start-for-mixed-integer-programming-mip-models-9046781dd21f&user=Bernardo+Furtado&userId=220a6eda5891&source=post_page-220a6eda5891----9046781dd21f---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9046781dd21f--------------------------------) ·10 分钟阅读·2023 年 4 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9046781dd21f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fheuristics-as-warm-start-for-mixed-integer-programming-mip-models-9046781dd21f&user=Bernardo+Furtado&userId=220a6eda5891&source=-----9046781dd21f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9046781dd21f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fheuristics-as-warm-start-for-mixed-integer-programming-mip-models-9046781dd21f&source=-----9046781dd21f---------------------bookmark_footer-----------)![](img/dddb71a353bfd16b1c89773175b62f3d.png)

图片由[Nils Geldner](https://unsplash.com/@n_geldner?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/nZf1_8WPIQk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在计算机科学中，启发式方法是用来寻找给定问题可行解的技术，通常比精确方法更快，但无法保证最优性。另一方面，精确方法计算开销更大，但可以保证找到最优解。

将问题建模为混合整数规划（*MIP*）并使用求解器求解可能会给出最优解。通常，这些求解器在后台使用分支定界算法。分支定界（*B&B*）被认为是一种精确的优化问题解决方法。

顾名思义，它将解决方案枚举为一个树状结构（分支）。*B&B*与通常称为暴力搜索的穷举搜索之间的主要区别在于界定阶段。在过程中，每个节点（解决方案）都与解决方案的下界和上界进行比较。如果某个分支被证明不会比已找到的最佳结果更好，则该分支会被剪枝，算法将转向另一个分支。

本文的目标不是详细介绍分支定界算法；更深入的解释可以在[这里](https://www.gurobi.com/resources/mixed-integer-programming-mip-a-primer-on-the-basics/)找到。但我们需要定义几个概念：
