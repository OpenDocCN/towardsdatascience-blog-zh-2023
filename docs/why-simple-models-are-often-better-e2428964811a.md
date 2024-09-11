# 为什么简单模型往往更好

> 原文：[https://towardsdatascience.com/why-simple-models-are-often-better-e2428964811a?source=collection_archive---------7-----------------------#2023-01-26](https://towardsdatascience.com/why-simple-models-are-often-better-e2428964811a?source=collection_archive---------7-----------------------#2023-01-26)

## 奥卡姆剃刀在数据科学和机器学习中的重要性

[](https://thomasdorfer.medium.com/?source=post_page-----e2428964811a--------------------------------)[![Thomas A Dorfer](../Images/9258a1735cee805f1d9b02e2adf01096.png)](https://thomasdorfer.medium.com/?source=post_page-----e2428964811a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e2428964811a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e2428964811a--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----e2428964811a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c54f9b62b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-simple-models-are-often-better-e2428964811a&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=post_page-7c54f9b62b90----e2428964811a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e2428964811a--------------------------------) ·8分钟阅读·2023年1月26日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe2428964811a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-simple-models-are-often-better-e2428964811a&source=-----e2428964811a---------------------bookmark_footer-----------)![](../Images/e1dc823456e970d8e19dbd386739e884.png)

图片来源：[Pablo Arroyo](https://unsplash.com/@pablogamedev) 通过 [Unsplash](https://unsplash.com/photos/_SEbdtH4ZLM)

在数据科学和机器学习中，简单性是一个重要的概念，它对模型的特性，如性能和可解释性，有着显著的影响。过度复杂的解决方案往往会通过增加过拟合的可能性、降低计算效率以及降低模型输出的透明度，来对这些特性产生不利影响。

后者对于需要一定解释性的领域尤为重要，如医学和医疗保健、金融或法律。无法解释和信任模型的决策——以及确保这些决策是公平和无偏的——可能对那些命运依赖于此的个人产生严重后果。

本文旨在强调在实施数据科学或机器学习解决方案时优先考虑简单性的的重要性。我们将首先介绍**奥卡姆剃刀**原则，然后深入探讨简单性的优势，并最终确定何时需要增加复杂性。

# 奥卡姆剃刀

奥卡姆剃刀，也被称为*节俭法则*，是一个哲学性的问题解决原则，归因于威廉·...
