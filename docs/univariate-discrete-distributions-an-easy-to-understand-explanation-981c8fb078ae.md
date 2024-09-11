# 单变量离散分布：易于理解的解释

> 原文：[https://towardsdatascience.com/univariate-discrete-distributions-an-easy-to-understand-explanation-981c8fb078ae?source=collection_archive---------8-----------------------#2023-10-20](https://towardsdatascience.com/univariate-discrete-distributions-an-easy-to-understand-explanation-981c8fb078ae?source=collection_archive---------8-----------------------#2023-10-20)

## 从数学和视觉上理解单变量离散分布

[](https://tinztwinspro.medium.com/?source=post_page-----981c8fb078ae--------------------------------)[![Janik 和 Patrick Tinz](../Images/a08aa54f553f606ef5df86f9411c36ac.png)](https://tinztwinspro.medium.com/?source=post_page-----981c8fb078ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----981c8fb078ae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----981c8fb078ae--------------------------------) [Janik 和 Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----981c8fb078ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4eb5d9652d9e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funivariate-discrete-distributions-an-easy-to-understand-explanation-981c8fb078ae&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=post_page-4eb5d9652d9e----981c8fb078ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----981c8fb078ae--------------------------------) ·9 分钟阅读·2023年10月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F981c8fb078ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funivariate-discrete-distributions-an-easy-to-understand-explanation-981c8fb078ae&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=-----981c8fb078ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F981c8fb078ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funivariate-discrete-distributions-an-easy-to-understand-explanation-981c8fb078ae&source=-----981c8fb078ae---------------------bookmark_footer-----------)![](../Images/26d786d83a235bec5295e1f58eb6c955.png)

图片由 [unDraw](https://undraw.co/license) 提供

**你有过这种感觉吗？你想学习一些新东西，但不知道从哪里开始。这正是我们想要以数学方式理解分布时的感受。是的，我们的教授给我们讲解了这些分布，但只是用数学公式！我们的教授没有用易于理解的方式和视觉化的方式来讲解。**

这就是为什么我们编写了这篇关于最重要的单变量离散分布的文章。我们希望通过数学和视觉的方式向你解释这些分布。我们的目标是让你理解数学与分布图之间的关系。此外，我们还为每种分布提供了一个示例。

作为数据科学家，了解分布如何工作是非常重要的。分布假设是一些机器学习算法的基础，对于解决统计问题也是必不可少的，例如在保险行业中。

我们将讨论以下分布：

+   **伯努利分布**

+   **二项分布**

+   **几何分布**

+   **泊松分布**
