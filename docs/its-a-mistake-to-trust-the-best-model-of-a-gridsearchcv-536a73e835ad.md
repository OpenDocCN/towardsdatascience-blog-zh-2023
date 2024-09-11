# 盲目相信 GridSearchCV 的最佳模型是一种错误

> 原文：[`towardsdatascience.com/its-a-mistake-to-trust-the-best-model-of-a-gridsearchcv-536a73e835ad?source=collection_archive---------5-----------------------#2023-01-04`](https://towardsdatascience.com/its-a-mistake-to-trust-the-best-model-of-a-gridsearchcv-536a73e835ad?source=collection_archive---------5-----------------------#2023-01-04)

## 通过四个例子解释了“最佳模型”实际上并不是最佳模型的情况

[](https://medium.com/@tomergabay?source=post_page-----536a73e835ad--------------------------------)![Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----536a73e835ad--------------------------------)[](https://towardsdatascience.com/?source=post_page-----536a73e835ad--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----536a73e835ad--------------------------------) [Tomer Gabay](https://medium.com/@tomergabay?source=post_page-----536a73e835ad--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc9c352dba00a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fits-a-mistake-to-trust-the-best-model-of-a-gridsearchcv-536a73e835ad&user=Tomer+Gabay&userId=c9c352dba00a&source=post_page-c9c352dba00a----536a73e835ad---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----536a73e835ad--------------------------------) ·6 分钟阅读·2023 年 1 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F536a73e835ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fits-a-mistake-to-trust-the-best-model-of-a-gridsearchcv-536a73e835ad&user=Tomer+Gabay&userId=c9c352dba00a&source=-----536a73e835ad---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F536a73e835ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fits-a-mistake-to-trust-the-best-model-of-a-gridsearchcv-536a73e835ad&source=-----536a73e835ad---------------------bookmark_footer-----------)![](img/e21cbcd59ca104b1b7dcf6a964510800.png)

图片由 [Choong Deng Xiang](https://unsplash.com/@dengxiangs?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Scikit-learn 的 `GridSearchCV` 是一个常用的工具，用于优化机器学习模型的超参数。不幸的是，并不是每个人都对其输出进行彻底分析，而是简单地使用 `GridSearchCV` 的最佳估计器。这意味着在很多情况下，你可能并没有使用真正的 *最佳* 估计器。我们首先来了解如何运行网格搜索，并检测它选择的最佳估计器。

一个非常基础的`GridSearchCV`可能如下所示：

```py
import pandas as pd
from sklearn.model_selection import GridSearchCV, train_test_split
from sklearn.tree import DecisionTreeRegressor

df = pd.read_csv('data.csv')
X_train, X_test, y_train, y_test = train_test_split(df.drop('target'),
                                                    df['target'],
                                                    random_state=42)

model = DecisionTreeRegressor()
gridsearch = GridSearchCV(model, 
                          param_grid={'max_depth': [None, 2, 50]},
                          scoring='mean_absolute_error')

gridsearch.fit(X_train, y_train)
gridsearch.train(X_train, y_train)

# get the results. This code outputs the tables we'll see in the article
results =…
```
