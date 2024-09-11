# 机器学习插图：使用 SHAP 打开黑箱模型

> 原文：[https://towardsdatascience.com/machine-learning-illustrated-opening-black-box-models-with-shap-9e92d0400680?source=collection_archive---------10-----------------------#2023-05-08](https://towardsdatascience.com/machine-learning-illustrated-opening-black-box-models-with-shap-9e92d0400680?source=collection_archive---------10-----------------------#2023-05-08)

## 如何使用 SHAP 解释和说明任何机器学习模型

[](https://medium.com/@shreya.rao?source=post_page-----9e92d0400680--------------------------------)[![Shreya Rao](../Images/03f13be6f5f67783d32f0798f09a4f86.png)](https://medium.com/@shreya.rao?source=post_page-----9e92d0400680--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9e92d0400680--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9e92d0400680--------------------------------) [Shreya Rao](https://medium.com/@shreya.rao?source=post_page-----9e92d0400680--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F99b63de2f2c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-illustrated-opening-black-box-models-with-shap-9e92d0400680&user=Shreya+Rao&userId=99b63de2f2c3&source=post_page-99b63de2f2c3----9e92d0400680---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9e92d0400680--------------------------------) ·10分钟阅读·2023年5月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9e92d0400680&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-illustrated-opening-black-box-models-with-shap-9e92d0400680&user=Shreya+Rao&userId=99b63de2f2c3&source=-----9e92d0400680---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9e92d0400680&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-illustrated-opening-black-box-models-with-shap-9e92d0400680&source=-----9e92d0400680---------------------bookmark_footer-----------)

Shapley 值是源自经济学中合作博弈论的一个概念，它根据每个玩家对游戏的贡献为他们分配一个值。在机器学习领域，这一概念被引入到 SHAP（SHapley Additive exPlanations）框架中，成为解释模型工作原理的有效技术。

如果你对了解更多关于 Shapley 值的内容感兴趣，我强烈建议你查看我之前的[文章](https://medium.com/@shreya.rao/economics-illustrated-shapley-values-7d33df43ada8)，该文章讨论了 Shapley 值背后的数学和直觉。尽管它已被修改为机器学习的用途，但理解其基本原理仍然很有帮助。

[](https://medium.com/@shreya.rao/economics-illustrated-shapley-values-7d33df43ada8?source=post_page-----9e92d0400680--------------------------------) [## 经济学插图：Shapley 值

### Shapley 值是博弈论中广泛使用的一个概念，它提供了一种公平分配总收益的方法。

medium.com](https://medium.com/@shreya.rao/economics-illustrated-shapley-values-7d33df43ada8?source=post_page-----9e92d0400680--------------------------------)

SHAP 框架与 Shapley 值类似，它计算游戏（即机器学习模型）中特征的单独影响。然而，机器学习模型是非合作博弈，这意味着特征之间不一定像在合作博弈中那样相互作用。相反，每个特征独立地对模型的输出做出贡献。虽然可以使用 Shapley 值公式，但它可能会...
