# 理解数据管理的多层次以推动产品

> 原文：[`towardsdatascience.com/understanding-the-multiple-layers-of-data-management-enabling-products-ce8033f3ad0d?source=collection_archive---------4-----------------------#2023-12-19`](https://towardsdatascience.com/understanding-the-multiple-layers-of-data-management-enabling-products-ce8033f3ad0d?source=collection_archive---------4-----------------------#2023-12-19)

## 产品领导者需要了解的内容，以便解除数据瓶颈

[](https://medium.com/@elliottstam?source=post_page-----ce8033f3ad0d--------------------------------)![Elliott Stam](https://medium.com/@elliottstam?source=post_page-----ce8033f3ad0d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce8033f3ad0d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce8033f3ad0d--------------------------------) [Elliott Stam](https://medium.com/@elliottstam?source=post_page-----ce8033f3ad0d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd193f11526c9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-multiple-layers-of-data-management-enabling-products-ce8033f3ad0d&user=Elliott+Stam&userId=d193f11526c9&source=post_page-d193f11526c9----ce8033f3ad0d---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce8033f3ad0d--------------------------------) ·11 分钟阅读·2023 年 12 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fce8033f3ad0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-multiple-layers-of-data-management-enabling-products-ce8033f3ad0d&user=Elliott+Stam&userId=d193f11526c9&source=-----ce8033f3ad0d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fce8033f3ad0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-multiple-layers-of-data-management-enabling-products-ce8033f3ad0d&source=-----ce8033f3ad0d---------------------bookmark_footer-----------)![](img/7f52411efbaa12ec419b40f5fe3c9023.png)

照片由 [美国传统巧克力](https://unsplash.com/@americanheritagechocolate?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

当我看到上面那层层蛋糕时，我立刻想吃掉它，但我也意识到，这与推动现代数据产品的数据管理层次有许多有用的相似之处。

就像蛋糕的每一层都涉及巧克力一样，数据管理的每一层都涉及数据，这一点并不意外。

但每一层都是独特的。蛋糕有黑巧克力、牛奶巧克力和白巧克力层。成功执行蛋糕的每一层需要不同的配料，就像成功执行我们数据管理流程的每一层需要不同的配料一样。

因此，我们来到本文的重点：实现产品愿景需要在每一个开发层面上拥有正确的配料。对于数据密集型产品来说，不理解交付产品所需的不同数据管理层，就像不了解我们试图烘焙的蛋糕的各层一样令人沮丧。

如果我们（产品负责人）只理解最上层（白巧克力），当团队在其他层面遇到阻碍时，我们将难以解开瓶颈。我们可能会搞不清楚*为什么*我们会遇到阻碍……
