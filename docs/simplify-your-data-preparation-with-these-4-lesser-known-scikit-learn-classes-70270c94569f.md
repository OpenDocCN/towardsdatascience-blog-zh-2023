# 简化数据准备：这四个鲜为人知的 Scikit-Learn 类

> 原文：[`towardsdatascience.com/simplify-your-data-preparation-with-these-4-lesser-known-scikit-learn-classes-70270c94569f?source=collection_archive---------0-----------------------#2023-06-01`](https://towardsdatascience.com/simplify-your-data-preparation-with-these-4-lesser-known-scikit-learn-classes-70270c94569f?source=collection_archive---------0-----------------------#2023-06-01)

## 忘掉 `train_test_split` 吧：即使你使用 XGBoost 或 LGBM，Pipeline、ColumnTransformer、FeatureUnion 和 FunctionTransformer 也是不可或缺的。

[](https://medium.com/@mattchapmanmsc?source=post_page-----70270c94569f--------------------------------)![Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----70270c94569f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----70270c94569f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----70270c94569f--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----70270c94569f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf7d13fc53db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-your-data-preparation-with-these-4-lesser-known-scikit-learn-classes-70270c94569f&user=Matt+Chapman&userId=bf7d13fc53db&source=post_page-bf7d13fc53db----70270c94569f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----70270c94569f--------------------------------) ·10 min read·2023 年 6 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F70270c94569f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-your-data-preparation-with-these-4-lesser-known-scikit-learn-classes-70270c94569f&user=Matt+Chapman&userId=bf7d13fc53db&source=-----70270c94569f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F70270c94569f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-your-data-preparation-with-these-4-lesser-known-scikit-learn-classes-70270c94569f&source=-----70270c94569f---------------------bookmark_footer-----------)![](img/4d616242f15fc7f0163616870c608e44.png)

图片来源于 [Victor](https://unsplash.com/@victor_g) 在 [Unsplash](https://unsplash.com/photos/UoIiVYka3VY)

数据准备在数据科学中被广泛认为是最不受欢迎的方面。然而，如果做对了，它其实不必如此令人头疼。

虽然由于 PyTorch、LightGBM 和 XGBoost 的迅猛崛起，scikit-learn 近年来作为一个*建模*库已经不再流行，但它仍然是最好的*数据准备*库之一。

我并不仅仅是在谈论那个老掉牙的`train_test_split`。如果你准备深入探索，你会发现一个对更高级数据准备技术非常有帮助的工具宝库，这些工具与`lightgbm`、`xgboost` 和 `catboost`等其他库进行后续建模时完全兼容。

在这篇文章中，我将介绍四个 scikit-learn 类，这些类显著加速了我在日常工作中作为数据科学家的数据准备工作流程。

# 1\. 管道：无缝地结合预处理步骤
