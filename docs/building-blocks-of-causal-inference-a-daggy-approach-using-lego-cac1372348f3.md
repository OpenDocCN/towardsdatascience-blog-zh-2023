# 因果推断的构建块——使用乐高的DAGgy方法

> 原文：[https://towardsdatascience.com/building-blocks-of-causal-inference-a-daggy-approach-using-lego-cac1372348f3?source=collection_archive---------6-----------------------#2023-02-04](https://towardsdatascience.com/building-blocks-of-causal-inference-a-daggy-approach-using-lego-cac1372348f3?source=collection_archive---------6-----------------------#2023-02-04)

## 《使用DAG和贝叶斯回归进行因果推断入门》

[](https://mmgillin.medium.com/?source=post_page-----cac1372348f3--------------------------------)[![穆雷·吉林](../Images/40619967eb8911fa1d651503143b940c.png)](https://mmgillin.medium.com/?source=post_page-----cac1372348f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cac1372348f3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cac1372348f3--------------------------------) [穆雷·吉林](https://mmgillin.medium.com/?source=post_page-----cac1372348f3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa168322fa6bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-blocks-of-causal-inference-a-daggy-approach-using-lego-cac1372348f3&user=Murray+Gillin&userId=a168322fa6bf&source=post_page-a168322fa6bf----cac1372348f3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cac1372348f3--------------------------------) · 9分钟阅读 · 2023年2月4日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcac1372348f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-blocks-of-causal-inference-a-daggy-approach-using-lego-cac1372348f3&user=Murray+Gillin&userId=a168322fa6bf&source=-----cac1372348f3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcac1372348f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-blocks-of-causal-inference-a-daggy-approach-using-lego-cac1372348f3&source=-----cac1372348f3---------------------bookmark_footer-----------)

因果推断是一个引人入胜的话题。因果模型旨在创建对变量关系的机制性理解。最近我一直在阅读 [理查德·麦克艾利思的《统计重思》](https://xcelab.net/rm/statistical-rethinking/)，他优美而易懂的写作方式改变了我对回归、统计分析甚至生活的看法。

本文旨在探讨使用有向无环图（DAGs）和 [brms](https://paul-buerkner.github.io/brms/) ([Buerkner](https://www.jstatsoft.org/article/view/v080i01)) 进行因果建模。

![](../Images/fe10c108fa784cf687d96ac142ce244c.png)

照片由[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我发现了Ziegler等人写的一篇精彩文章，旨在教学校儿童多重线性回归模型的教学方法。我将使用的数据集来自于这篇文章，并且在CC BY 4.0许可下提供。

[](https://www.tandfonline.com/doi/full/10.1080/26939169.2021.1946450?scroll=top&needAccess=true&role=tab&source=post_page-----cac1372348f3--------------------------------) [## 利用LEGO积木数据构建多重线性回归模型

### 摘要 我们展示了一项创新活动，利用关于LEGO积木的数据帮助学生自我发现多个…

www.tandfonline.com](https://www.tandfonline.com/doi/full/10.1080/26939169.2021.1946450?scroll=top&needAccess=true&role=tab&source=post_page-----cac1372348f3--------------------------------)

## **加载包和数据**

```py
library(tidyverse)
library(tidybayes)
library(brms)
library(ggdag)
library(dagitty)

train <- read_csv('lego.population.csv')

train_parse <- 
train %>% 
  drop_na(Weight…
```
