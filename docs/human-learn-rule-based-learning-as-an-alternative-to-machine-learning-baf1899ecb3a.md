# 人类学习：基于规则的学习作为机器学习的替代方案

> 原文：[https://towardsdatascience.com/human-learn-rule-based-learning-as-an-alternative-to-machine-learning-baf1899ecb3a?source=collection_archive---------2-----------------------#2023-01-01](https://towardsdatascience.com/human-learn-rule-based-learning-as-an-alternative-to-machine-learning-baf1899ecb3a?source=collection_archive---------2-----------------------#2023-01-01)

## 将领域知识融入您的模型中，通过基于规则的学习

[](https://khuyentran1476.medium.com/?source=post_page-----baf1899ecb3a--------------------------------)[![Khuyen Tran](../Images/98aa66025ad29b618e875c75f1c400a5.png)](https://khuyentran1476.medium.com/?source=post_page-----baf1899ecb3a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----baf1899ecb3a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----baf1899ecb3a--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----baf1899ecb3a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhuman-learn-rule-based-learning-as-an-alternative-to-machine-learning-baf1899ecb3a&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----baf1899ecb3a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----baf1899ecb3a--------------------------------) ·7分钟阅读·2023年1月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbaf1899ecb3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhuman-learn-rule-based-learning-as-an-alternative-to-machine-learning-baf1899ecb3a&user=Khuyen+Tran&userId=84a02493194a&source=-----baf1899ecb3a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbaf1899ecb3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhuman-learn-rule-based-learning-as-an-alternative-to-machine-learning-baf1899ecb3a&source=-----baf1899ecb3a---------------------bookmark_footer-----------)

# 动机

您获得了一个标记的数据集，并被要求预测一个新的数据集。您会怎么做？

您可能首先尝试的方法是训练一个机器学习模型来寻找标记新数据的规则。

![](../Images/1044eec31dae2c0e7f68aa7188b489cc.png)

图片由作者提供

这很方便，但很难知道机器学习模型为何会得出特定的预测结果。您也不能将您的领域知识融入模型中。

不依赖机器学习模型进行预测，是否有一种方法可以根据您的知识为数据标注设置规则？

![](../Images/cfa0724b6d540aa558c2d888cdb6878e.png)

作者提供的图片

这就是人类学习的用武之地。

# 什么是人类学习？
