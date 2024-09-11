# 在机器学习系统中维护数据质量

> 原文：[`towardsdatascience.com/upholding-data-quality-in-machine-learning-systems-d77a7d06f02e?source=collection_archive---------17-----------------------#2023-06-29`](https://towardsdatascience.com/upholding-data-quality-in-machine-learning-systems-d77a7d06f02e?source=collection_archive---------17-----------------------#2023-06-29)

## 数据 | 机器学习 | 质量保证

## 对机器学习中未被察觉的基石的推荐

[](https://david-farrugia.medium.com/?source=post_page-----d77a7d06f02e--------------------------------)![David Farrugia](https://david-farrugia.medium.com/?source=post_page-----d77a7d06f02e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d77a7d06f02e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d77a7d06f02e--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----d77a7d06f02e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3916826092a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fupholding-data-quality-in-machine-learning-systems-d77a7d06f02e&user=David+Farrugia&userId=3916826092a6&source=post_page-3916826092a6----d77a7d06f02e---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d77a7d06f02e--------------------------------) ·4 分钟阅读·2023 年 6 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd77a7d06f02e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fupholding-data-quality-in-machine-learning-systems-d77a7d06f02e&user=David+Farrugia&userId=3916826092a6&source=-----d77a7d06f02e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd77a7d06f02e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fupholding-data-quality-in-machine-learning-systems-d77a7d06f02e&source=-----d77a7d06f02e---------------------bookmark_footer-----------)![](img/00e8f1eff37346506cb9e807b936fcd3.png)

图片由 [Battlecreek Coffee Roasters](https://unsplash.com/@battlecreekcoffeeroasters?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在耀眼的机器学习（ML）世界中，沉浸于设计复杂算法、迷人的可视化和令人印象深刻的预测模型的刺激感中，实在是太容易了。

然而，就像建筑物的耐久性不仅取决于可见的结构，还取决于隐蔽的基础，机器学习系统的有效性也取决于一个常被忽视但至关重要的方面：**数据质量**。

# 上游数据质量保证的必要性

可以把你的机器学习训练和推理管道想象成一列蒸汽火车的旅程。

维护火车本身——即机器学习系统——的健康至关重要，但如果轨道出现问题呢？

如果供应给系统的数据质量在上游没有得到保证，这就像受损的铁轨——***你的火车迟早会脱轨***，尤其是在大规模操作时。

因此，从一开始，就需要从源头监控数据质量。
